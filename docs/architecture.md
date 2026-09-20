# Architecture specification

Status: intended architecture. Technology and transport choices remain open in [implementation.md](implementation.md).

## Ownership

| Component | Owns | Boundary |
| --- | --- | --- |
| Web interface | Submission, progress, reports, comparisons | Uses the application service; never connects directly to the queue or scanner. |
| Application service | Request validation, access control, scan lifecycle, normalized reports, prioritization, comparisons | Sole owner of the application database and authority for accepted scan state. |
| Scanning service | Job consumption, isolated browser runs, audit modules, engine normalization, artifact production | Delivers versioned results; does not write the application database directly. |
| Job queue | Work buffering and redelivery | Delivery can occur more than once; transport does not determine final scan state. |
| Artifact storage | Raw engine reports and selected evidence | Private access, bounded retention, and deletion tied to report policy. |

Keep two backend services in one repository. Do not split each category or recommendation rule into a service. Use Lighthouse first; add another engine only for a specific missing capability.

## Scan flow

1. The application validates the URL and profile, establishes report ownership, and persists a queued scan.
2. It schedules a versioned job. A persisted scan must not remain queued forever if publishing fails: use a recoverable dispatch mechanism and define it with the queue choice.
3. The scanner claims an authorized attempt, validates the job, and reports that execution has started. Duplicate delivery must not launch unlimited browser runs.
4. The browser executes within public-only network and resource boundaries. Modules capture check outcomes, findings, conditions, and safe error details.
5. The scanner writes permitted artifacts and delivers a versioned result for that attempt.
6. The application validates the result, accepts it idempotently, persists the report and terminal state together, and makes it available to its owner.
7. A later scan creates a separate snapshot. Comparison uses findings plus explicit check coverage.

See [contracts.md](contracts.md) for lifecycle and result invariants.

## Reliability

- Define finite queue wait, execution, and result-delivery deadlines. Recover abandoned jobs and attempts after worker failure.
- Use bounded retries for transient failures. Do not retry deterministic input rejection or prohibited network targets.
- Give each execution attempt a distinct identity. Fence expired attempts so late results cannot replace accepted data.
- Preserve usable output if a module fails. Report incomplete coverage explicitly rather than assigning a successful score.
- Acknowledge result delivery only after durable acceptance. Repeated delivery must return the existing outcome without duplicate findings.
- Define reconciliation for failed publishing and orphaned artifact writes. Choose the smallest mechanism supported by the selected infrastructure.
- Log scan and attempt IDs, lifecycle transitions, timings, and safe error codes. Exclude page bodies, credentials, and sensitive URL values from routine logs.

## Trust boundaries

These are implementation requirements, not claims that a particular deployment already enforces them.

- Accept HTTP/HTTPS only; reject embedded credentials. Validate and parse URLs consistently across submission and execution.
- Enforce public-only egress at connection time for both IP families, initial navigation, redirects, and every browser subrequest. Handle DNS changes and alternate address representations. Reject destinations that cannot be classified safely.
- Do not rely on a preflight DNS lookup alone. Select an enforceable network boundary before allowing arbitrary user-supplied URLs.
- Run browser processes with isolation and finite time, memory, transfer, and concurrency budgets. The browser sandbox must not have access to application credentials or internal services.
- Validate queue and result payloads; authenticate service-to-service delivery. The application validates artifact references rather than trusting arbitrary paths supplied in results.
- Escape page-derived text when rendering reports. Never serve captured target HTML as trusted application content.
- Authorize report, comparison, and artifact access. Keep storage private and restrict any temporary download links.
- Minimize captured data and scrub sensitive URL values from presentation/logging according to a defined policy. Do not silently change the actual target URL in a way that changes the page being analyzed.
- Define report ownership, retention duration, deletion behavior, request limits, and deployment budgets before exposing the service publicly.

## Local development

Keep local startup reproducible, with one documented path to start the interface and both services plus required dependencies. Use a safe environment-variable example with no real secrets. Prefer recorded engine fixtures for routine tests; real browser integration tests need the same network boundary as production, with controlled targets provisioned outside protected networks.
