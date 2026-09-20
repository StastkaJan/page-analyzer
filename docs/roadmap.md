# Integration roadmap

Status: specifications prepared; all implementation stages are not started. This roadmap owns stage order, delivery status, and completion evidence. [Implementation notes](implementation.md) own technical decisions and verification requirements; [product.md](product.md) owns acceptance, and [contracts.md](contracts.md) defines domain semantics.

Each stage integrates a usable path across components and leaves earlier paths working. The first release delivers submission of a public URL, an actionable private report, and comparison with a later scan. Dates remain unset until the stack, operating constraints, and available capacity are known.

The [ticket backlog](tickets/README.md) contains 36 tasks with an early feasibility phase before the eight integration stages. Start with [PA-025](tickets/PA-025.md). IDs preserve earlier references; the backlog table defines execution order. Decisions and proofs must be completed before their dependent implementation.

## Stage overview

| Stage | Integration | Demonstrable result | Prerequisite | Status |
| --- | --- | --- | --- | --- |
| P0: Feasibility and initial decisions | Candidate environment -> isolated browser -> real engine evidence | Required execution boundaries and comparison assumptions have observed evidence. | None | Not started |
| S1: Contract and development flow | Interface -> application -> scanner fixture -> interface | A clearly labeled sample report travels through both services. | P0 | Not started |
| S2: Private persisted scans | Interface -> application -> database and private artifacts | The owner can reopen a saved sample report; another user cannot. | S1 | Not started |
| S3: Background execution | Application -> queue -> scanner -> result delivery -> application | A fixture scan runs asynchronously and survives delivery failures. | S2 | Not started |
| S4: Real browser scans | Scanner -> isolated Lighthouse -> public target -> report | A public URL produces a real retained report through the complete flow. | S3 | Not started |
| S5: Actionable reports | Engine normalization -> recommendation rules -> report interface | Findings explain the priority, evidence, fix, and verification step. | S4 | Not started |
| S6: Rescan and comparison | New scan -> retained history -> comparison -> interface | Users see supported resolved, persistent, new, and unverified findings. | S5 | Not started |
| S7: Operating controls | Request admission -> workers -> storage lifecycle -> diagnostics | Work stays bounded, recoverable, private, and subject to deletion. | S6 | Not started |
| S8: Release verification | Intended deployment -> complete user journey -> release evidence | A reproducible first version is ready for authorized publication. | S7 | Not started |

The previous milestones map to these stages: M1 = S1-S4, M2 = S5, M3 = S6, M4 = S7-S8. P0 performs controlled real-browser feasibility checks; S4 is the first real product scanning flow. Fixture execution in S1-S3 must be explicit, development/test-only, and unavailable as a production result fallback.

## P0: Feasibility and initial decisions

**Tickets:** [PA-025](tickets/PA-025.md), [PA-026](tickets/PA-026.md), [PA-027](tickets/PA-027.md).

**Integration:** verify the candidate runtime/deployment environment against actual isolated browser execution and real engine output before committing to application integration.

**Features**

- [ ] Select the initial stack and local/deployment constraints with reproducible proof-harness setup.
- [ ] Prove public-only connection-time egress and isolation with controlled browser targets and finite provisional budgets.
- [ ] Capture sanitized real Lighthouse evidence for both profiles and map report fields, coverage outcomes, and comparison cases.
- [ ] Demonstrate supported persistent/resolved/new cases and conservative unverified cases; record unsupported promises and resolve them before dependent work.

**Result:** the project has a workable execution environment and evidence that the initial engine can support the intended report/comparison semantics. No product submission endpoint or public service is enabled.

**Verification and handoff:** retain runnable boundary checks, commands, versions, provisional numeric limits, sanitized engine fixtures, and an evidence matrix. A missing environment, failed boundary, or unsupported required comparison blocks dependent work. A design document alone does not complete this phase.

## S1: Contract and development flow

**Tickets:** [PA-028](tickets/PA-028.md), [PA-029](tickets/PA-029.md), [PA-001](tickets/PA-001.md), [PA-002](tickets/PA-002.md), [PA-003](tickets/PA-003.md).

**Integration:** connect the interface, application service, and scanner through a development-only fixture flow. Keep services independently runnable; introduce no empty future components.

**Features**

- [ ] Use P0's proven constraints and record versions/rationale in [implementation.md](implementation.md). Implement executable schemas in PA-028 and authenticated transport in PA-029 before connecting fixture execution in PA-001.
- [ ] Define executable job/result schemas for scan ID, attempt ID, contract version, profile, check outcomes, engine metadata, findings, and safe errors.
- [ ] Add a minimal interface to select a known sample and mobile/desktop profile, invoke the application, and display the scanner fixture response.
- [ ] Provide deterministic successful, partial, and failed sample outputs. Label the entire flow as a simulation; never imply a URL was measured.
- [ ] Validate payloads at both service boundaries and establish service authentication for the chosen transport.
- [ ] Add reproducible startup, a safe configuration example, check commands, and CI for implemented behavior.
- [ ] Use labeled keyboard-accessible controls, visible focus, and escaped report text from the first screen.

**Result:** a developer starts both services and the interface, selects a sample, and sees the expected response pass through the actual service boundary. No arbitrary-URL browser execution or retained user reports exist in this stage.

**Verification and handoff:** confirm all three sample outcomes, unsupported contract rejection, malformed payload rejection, service authentication, and unavailable-scanner error presentation. Record actual startup/check commands and schema locations. Subsequent stages reuse this result contract; the fixture adapter remains test-only.

## S2: Private persisted scans

**Tickets:** [PA-030](tickets/PA-030.md), [PA-031](tickets/PA-031.md), [PA-032](tickets/PA-032.md), [PA-004](tickets/PA-004.md), [PA-005](tickets/PA-005.md), [PA-006](tickets/PA-006.md).

**Integration:** connect application-owned scan/report storage and private artifact storage to the interface. The scanner never writes the application database.

**Features**

- [ ] Define identity/session behavior in PA-030, implement actual sessions in PA-031, and establish database/schema setup in PA-032 before retaining reports. Select private artifact storage in PA-005.
- [ ] Persist requested URL, profile, status, attempt identity, timestamps, accepted result, and immutable terminal report data according to the contracts.
- [ ] Save permitted sample artifacts with opaque references and validate ownership, type, and size. Do not expose arbitrary storage paths.
- [ ] Add owner-authorized scan/status/report retrieval and artifact access; knowing an ID must not grant access.
- [ ] Validate HTTP/HTTPS URL syntax and reject credentials and prohibited literal addresses without navigating anywhere. Execution stays limited to explicit fixtures until S4.
- [ ] Restore saved reports after refresh or service restart. Refreshing a status/report view must not submit another scan.
- [ ] Exclude sensitive URL values, page content, and credentials from normal logs; escape page-derived report content.

**Result:** the owner can reopen the same saved sample report and permitted evidence. A separate user is denied access to both the report and its artifacts.

**Verification and handoff:** prove persistence across restart, unchanged terminal snapshots, rejected invalid submissions, safe rendering of hostile text, and denied cross-user access. Record the ownership model, schema setup, and artifact capture policy. Retention duration and automated deletion are finalized in S7 before public exposure.

## S3: Background execution and recovery

**Tickets:** [PA-033](tickets/PA-033.md), [PA-007](tickets/PA-007.md), [PA-008](tickets/PA-008.md), [PA-009](tickets/PA-009.md).

**Integration:** replace the development request/response execution path with application -> queue -> scanner -> result delivery. Continue using fixtures to verify delivery and lifecycle behavior deterministically.

**Features**

- [ ] Complete PA-033's delivery/ownership decision and finite numeric deadline/retry policy before implementing recoverable scan persistence/job dispatch.
- [ ] Return a scan reference without waiting for execution; show persisted queued, running, and terminal states with timestamps and no fabricated percentage.
- [ ] Claim one eligible attempt for execution and reject malformed, unsupported, or unauthenticated job/result messages.
- [ ] Add finite queue/execution/delivery deadlines, bounded transient retries, worker-loss recovery, and expired-attempt fencing.
- [ ] Save the accepted report and terminal state atomically; acknowledge results only after durable acceptance.
- [ ] Handle repeated jobs/results, conflicting results, missing start notifications, and out-of-order events without duplicate reports or terminal-state regression.
- [ ] Preserve useful partial fixture output; show a safe failed state when nothing usable remains. Do not retry deterministic input rejection.

**Result:** a user launches a simulated background scan, leaves or refreshes the page, and later retrieves its final result. Worker or delivery failure resolves through bounded recovery or explicit failure instead of leaving work queued indefinitely.

**Verification and handoff:** interrupt workers and result delivery, fail publishing, replay messages, and deliver an old attempt after a newer attempt finishes. Verify one accepted report, bounded attempts, protected terminal state, and all lifecycle outcomes. Document dispatch reconciliation, retry/deadline settings, and attempt ownership.

## S4: Real isolated browser scans

**Tickets:** [PA-034](tickets/PA-034.md), [PA-010](tickets/PA-010.md), [PA-011](tickets/PA-011.md), [PA-012](tickets/PA-012.md).

**Integration:** connect the scanner job handler to isolated Lighthouse execution and feed real results into the existing delivery, storage, and report path.

**Features**

- [ ] Integrate P0's proven public-only egress into the worker and verify IPv4/IPv6, DNS changes, redirects, and every browser subrequest before enabling arbitrary target URLs.
- [ ] Set concrete browser budgets in PA-034, then enforce them in the isolated worker for execution time, memory, transfer/download, and concurrency. Keep application secrets and internal services inaccessible.
- [ ] Replace fixture selection with public URL submission and explicit mobile/desktop profiles in the real product flow. Keep fixture mode separate and disabled for production users.
- [ ] Run Lighthouse's initial performance, technical SEO, accessibility, and best-practice coverage without adding overlapping engines.
- [ ] Normalize enough output to retain check coverage, category scores/scales, findings/evidence, requested/final URLs, engine versions, timings, and actual conditions.
- [ ] Preserve usable results after partial engine failure; distinguish navigation failure, prohibited destinations, resource limits, and engine errors.
- [ ] Retain only permitted raw reports/evidence in private storage and show source results, coverage gaps, and laboratory measurement labels.

**Result:** a real permitted public page works through submission, background progress, report delivery, and later retrieval without operator-run tools. Both device profiles work; unavailable results never appear as successful checks.

**Verification and handoff:** demonstrate real scans on controlled public targets. Verify blocked private/local targets, DNS changes, alternate address forms, redirect/subrequest bypass attempts, resource exhaustion, and partial output. Browser checks use the enforced network boundary. Record engine/browser versions, profiles, limits, and results. S1-S4 completes the original M1.

## S5: Actionable reports and recommendations

**Tickets:** [PA-013](tickets/PA-013.md), [PA-014](tickets/PA-014.md), [PA-015](tickets/PA-015.md).

**Integration:** connect normalized engine output to deterministic prioritization and guidance in the application service, then render the complete report.

**Features**

- [ ] Complete stable rule/target identities with versioned normalization; retain original evidence and sources.
- [ ] Group findings by category and merge only demonstrated duplicates for the same issue and target.
- [ ] Preserve engine severity and category scores; add separate product priority, a short rationale, and estimated effort or unknown.
- [ ] Include affected elements/resources where available, measured values, why the finding matters, suggested fix, trusted documentation, and verification step.
- [ ] Keep unavailable scores, skipped/failed checks, not-applicable outcomes, and manual-review items visible and distinct.
- [ ] Verify report navigation, keyboard operation, focus, readable status/error text, and safe rendering across outcome types.
- [ ] Prepare representative reports for product feedback using measured findings and curated guidance without an LLM dependency.

**Result:** a user can identify a priority issue, inspect its evidence, understand a concrete fix, and explain how to verify it. Scores retain their source and scale; no unexplained overall score hides failures.

**Verification and handoff:** compare successful, partial, and failed fixtures with rendered reports. Check evidence traceability, deterministic priority, unknown effort, source preservation when combining duplicates, and manual coverage. Record rule/mapping locations and representative outputs. Collect and record developer/agency feedback when outreach is explicitly authorized; do not claim validation before it happens. This stage completes M2.

## S6: Rescan, history, and comparison

**Tickets:** [PA-035](tickets/PA-035.md), [PA-016](tickets/PA-016.md), [PA-017](tickets/PA-017.md), [PA-018](tickets/PA-018.md).

**Integration:** connect explicit rescan actions to the existing job path, then compare two owner-authorized immutable reports through the application service and interface.

**Features**

- [ ] Add rescan actions that create new scan IDs, preserve earlier reports, and allow explicit profile selection.
- [ ] Provide access to the page's retained scans and select an earlier baseline and later scan.
- [ ] Complete PA-035's versioned URL identity and comparison decision matrix using P0 engine evidence before implementing history/comparison. Preserve meaningful query parameters and validate same-page inputs.
- [ ] Match stable rule/target identities and classify persistent, resolved, new, and unverified findings using explicit comparable coverage.
- [ ] Warn about changed profiles, engines, conditions, or final destinations; withhold conclusions where equivalence cannot be established.
- [ ] Label removed-target and not-applicable cases explicitly; absence alone cannot prove a fix. Treat score changes as observations, not proof of causation.
- [ ] Require access to both reports and prevent comparison endpoints from leaking another owner's data.

**Result:** a user rescans after a controlled page change and sees which findings persisted, were resolved, appeared, or could not be verified. Both original reports remain unchanged and accessible to their owner.

**Verification and handoff:** demonstrate resolved, persistent, and new findings with comparable coverage plus an unverified case caused by missing coverage. Verify failed/skipped checks, differing profiles/versions, removed targets, meaningful query differences, and cross-user denial. Record matching policy and fixtures. This stage completes M3 and the functional first-version journey.

## S7: Operating limits, retention, and diagnostics

**Tickets:** [PA-036](tickets/PA-036.md), [PA-019](tickets/PA-019.md), [PA-020](tickets/PA-020.md), [PA-021](tickets/PA-021.md).

**Integration:** connect request admission, bounded queue/worker capacity, storage lifecycle, and safe diagnostics. Privacy and scanner isolation remain required throughout.

**Features**

- [ ] Complete PA-036's concrete admission/retention policy and reconcile it with delivery and browser budgets before implementing quotas or cleanup.
- [ ] Show actionable responses when capacity or quotas prevent a scan; do not accept unbounded work.
- [ ] Define and implement report/artifact retention, authorized deletion, and orphaned-artifact cleanup.
- [ ] Coordinate deletion with active/retried jobs and late results so deleted data is not recreated. Remove comparison access to deleted data consistently.
- [ ] Record safe scan/attempt correlation, queue wait, execution/delivery timings, outcome counts, and resource-limit events using the selected platform's existing facilities.
- [ ] Provide recovery instructions for failed dispatch, worker loss, delivery failure, and storage failure; exclude sensitive target data from diagnostics.
- [ ] Recheck report/artifact access, service authentication, and network restrictions under concurrency and failure conditions.

**Result:** accepted work completes or fails within configured limits, operators can trace failures without captured secrets, and expired/deleted reports and artifacts become inaccessible and are removed according to policy.

**Verification and handoff:** exercise quotas, capacity limits, queue backlog, retry exhaustion, partial storage failures, expiration, deletion during scanning, and late delivery after deletion. Record limits, retention durations, cleanup evidence, recovery steps, and operating limitations. This stage completes the operational portion of M4.

## S8: Deployment integration and release verification

**Tickets:** [PA-022](tickets/PA-022.md), [PA-023](tickets/PA-023.md), [PA-024](tickets/PA-024.md).

**Integration:** assemble the interface, both services, queue, database, and private artifact storage in the intended deployment configuration and verify the complete journey there.

**Features**

- [ ] Select hosting and wire service identities, secrets, network policies, persistent storage, and configuration using established boundaries.
- [ ] Make installation, schema setup, startup, checks, and deployment reproducible with documented commands and required environment variables.
- [ ] Define health/readiness checks for actual dependencies and document recovery, rollback, and data/schema compatibility constraints.
- [ ] Verify fixture mode is unavailable to production users and reports identify actual engine versions and conditions.
- [ ] Run the complete submission/report/rescan/comparison journey for both profiles, including partial and failed scans and keyboard operation.
- [ ] Re-run environment-sensitive egress, isolation, authentication, quota, retention, deletion, and recovery checks in the intended deployment.
- [ ] Record release evidence against all [product acceptance criteria](product.md#acceptance-criteria) and applicable [verification requirements](implementation.md#verification-strategy), including known limitations.

**Result:** a reproducible release candidate provides the complete first-version journey within verified operating limits. The intended deployment has recorded evidence supporting release readiness.

**Verification and handoff:** deliver setup/configuration instructions, the verification record, sample real reports/comparisons, recovery/rollback steps, and unresolved limitations. If the intended environment is unavailable or checks cannot run, record that blocker and leave the stage incomplete. Provisioning paid/external resources and publication occur under an authorized task. Completion finishes M4; readiness alone does not publish the service.

## After the first release

These are candidate integrations, not committed stages. Use feedback and evidence of recurring value to select the next one; validate willingness to pay before adding billing.

| Candidate | Integrated features | Expected result | Entry evidence |
| --- | --- | --- | --- |
| Site coverage | Bounded same-site crawler -> scan queue -> page grouping and shared-template findings. | Inspect recurring issues across a small site. | Repeated related-page scanning and proven single-page operating limits. |
| Continuous monitoring | Scheduler -> existing scans/comparisons -> regression notifications. | Learn about supported regressions without manually starting scans. | Recurring comparison demand and understood measurement variability. |
| Agency workflow | Projects/team access -> client reports -> selected exports/integrations. | Manage a demonstrated multi-client workflow. | Repeated agency usage and a concrete missing capability. |
| Deeper guidance | Framework-specific rules or optional AI -> existing evidence -> evaluated explanations. | Receive more useful advice grounded in actual findings. | A demonstrated guidance gap and a way to evaluate accuracy/usefulness. |

Authenticated target pages, automatic website edits, full security testing, and large-scale crawling remain outside committed scope. Additional engines/services require a demonstrated coverage or deployment need.

## Tracking and stage handoff

- Use Not started, In progress, Blocked, or Done in the overview. For blocked work, record the blocker and next action.
- Check a feature only when its deliverable exists and relevant verification has run. Mark a stage done only when its stated result is demonstrated across integrated components.
- At each handoff, link changed code/contracts, exact commands and outcomes, demo/report evidence, and limitations. Planned checks and fixture scans are not evidence of real browser behavior.
- Keep earlier checks passing. Document technical choices in implementation notes and behavior/contract changes in their owning specs; update this file for stage order and status.
- Next action: complete PA-025's initial stack/environment decision, then PA-026/PA-027's observed feasibility proofs. Start S1 only after P0 passes; keep arbitrary product URL execution disabled until S4 integration checks pass.
