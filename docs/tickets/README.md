# Implementation tickets

This local backlog contains 36 tickets for the [roadmap](../roadmap.md): an early feasibility phase followed by eight integration stages. No ticket has been implemented as part of this planning work. Individual files own status and completion evidence; these are not published external issues.

Start with [PA-025](PA-025.md). Ticket IDs are stable references, not execution order: PA-001 through PA-024 are preserved, and PA-025 through PA-036 refine their prerequisites. Follow the table order and dependencies below. PA-025 is the only ticket with no prerequisites.

## What this refinement resolves

| Gap | Work that resolves it |
| --- | --- |
| Stack selection, schemas, authentication, and integration bundled together | [PA-025](PA-025.md), [PA-028](PA-028.md), [PA-029](PA-029.md), then narrowed [PA-001](PA-001.md). |
| Browser/deployment feasibility proven too late | [PA-026](PA-026.md) before service integration; [PA-010](PA-010.md) later integrates and rechecks the proven mechanism. |
| Comparison assumptions lack real engine evidence | [PA-027](PA-027.md) captures evidence early; [PA-035](PA-035.md) fixes classification policy before [PA-017](PA-017.md). |
| Private ownership without a defined identity/session flow | [PA-030](PA-030.md) defines behavior; [PA-031](PA-031.md) implements it before [PA-004](PA-004.md) persists reports. |
| Database selection/setup bundled with report behavior | [PA-032](PA-032.md) establishes schema and setup before [PA-004](PA-004.md). |
| Vague deadlines, resource budgets, quotas, and retention | [PA-033](PA-033.md), [PA-034](PA-034.md), and [PA-036](PA-036.md) must record finite values, units, rationale, and enforcement owners before dependent implementation. |

## Backlog in execution order

| Phase/stage | Ticket | Task | Depends on |
| --- | --- | --- | --- |
| P0 | [PA-025](PA-025.md) | Select the initial stack and deployment constraints | None |
| P0 | [PA-026](PA-026.md) | Prove browser isolation and connection-time egress | [PA-025](PA-025.md) |
| P0 | [PA-027](PA-027.md) | Prove engine evidence and comparison feasibility | [PA-026](PA-026.md) |
| S1 | [PA-028](PA-028.md) | Implement executable scan and result schemas | [PA-027](PA-027.md) |
| S1 | [PA-029](PA-029.md) | Implement authenticated service transport | [PA-028](PA-028.md) |
| S1 | [PA-001](PA-001.md) | Connect the validated services to fixture execution | [PA-028](PA-028.md), [PA-029](PA-029.md) |
| S1 | [PA-002](PA-002.md) | Connect the accessible interface to the fixture flow | [PA-001](PA-001.md) |
| S1 | [PA-003](PA-003.md) | Make local startup and S1 verification reproducible | [PA-002](PA-002.md) |
| S2 | [PA-030](PA-030.md) | Define user identity and session behavior | [PA-003](PA-003.md) |
| S2 | [PA-031](PA-031.md) | Implement user sessions and authenticated request context | [PA-030](PA-030.md) |
| S2 | [PA-032](PA-032.md) | Establish application database schema and setup | [PA-030](PA-030.md) |
| S2 | [PA-004](PA-004.md) | Persist owner-scoped scans and immutable reports | [PA-031](PA-031.md), [PA-032](PA-032.md) |
| S2 | [PA-005](PA-005.md) | Store artifacts privately and authorize retrieval | [PA-004](PA-004.md) |
| S2 | [PA-006](PA-006.md) | Integrate validated submission and saved report views | [PA-004](PA-004.md), [PA-005](PA-005.md) |
| S3 | [PA-033](PA-033.md) | Decide delivery ownership and numeric retry policy | [PA-006](PA-006.md) |
| S3 | [PA-007](PA-007.md) | Dispatch scans to the queue and show persisted progress | [PA-033](PA-033.md) |
| S3 | [PA-008](PA-008.md) | Accept results atomically and reject stale or conflicting delivery | [PA-007](PA-007.md) |
| S3 | [PA-009](PA-009.md) | Bound retries and recover abandoned scans | [PA-007](PA-007.md), [PA-008](PA-008.md), [PA-033](PA-033.md) |
| S4 | [PA-034](PA-034.md) | Set measured browser and scan resource budgets | [PA-009](PA-009.md) |
| S4 | [PA-010](PA-010.md) | Enforce scanner isolation and public-only egress | [PA-034](PA-034.md) |
| S4 | [PA-011](PA-011.md) | Integrate Lighthouse and normalize real scan results | [PA-010](PA-010.md) |
| S4 | [PA-012](PA-012.md) | Enable the real submission-to-report journey | [PA-010](PA-010.md), [PA-011](PA-011.md) |
| S5 | [PA-013](PA-013.md) | Stabilize finding identities and consolidate demonstrated duplicates | [PA-012](PA-012.md) |
| S5 | [PA-014](PA-014.md) | Add deterministic priorities and evidence-backed guidance | [PA-013](PA-013.md) |
| S5 | [PA-015](PA-015.md) | Deliver the complete accessible actionable report | [PA-013](PA-013.md), [PA-014](PA-014.md) |
| S6 | [PA-035](PA-035.md) | Specify URL identity and the comparison decision matrix | [PA-015](PA-015.md), [PA-027](PA-027.md) |
| S6 | [PA-016](PA-016.md) | Add immutable rescans and private scan history | [PA-035](PA-035.md) |
| S6 | [PA-017](PA-017.md) | Compare findings using explicit coverage equivalence | [PA-013](PA-013.md), [PA-016](PA-016.md), [PA-035](PA-035.md) |
| S6 | [PA-018](PA-018.md) | Integrate comparison views and prove the rescan journey | [PA-016](PA-016.md), [PA-017](PA-017.md) |
| S7 | [PA-036](PA-036.md) | Define numeric admission and retention policies | [PA-018](PA-018.md), [PA-034](PA-034.md) |
| S7 | [PA-019](PA-019.md) | Enforce request quotas and bounded admission | [PA-036](PA-036.md) |
| S7 | [PA-020](PA-020.md) | Implement retention and deletion without result resurrection | [PA-036](PA-036.md) |
| S7 | [PA-021](PA-021.md) | Integrate safe diagnostics and operational recovery | [PA-019](PA-019.md), [PA-020](PA-020.md) |
| S8 | [PA-022](PA-022.md) | Prepare the reproducible deployment configuration | [PA-021](PA-021.md) |
| S8 | [PA-023](PA-023.md) | Document and exercise deployment recovery and rollback | [PA-022](PA-022.md) |
| S8 | [PA-024](PA-024.md) | Verify the release candidate against all first-version criteria | [PA-022](PA-022.md), [PA-023](PA-023.md) |

PA-031 and PA-032 can be implemented independently once PA-030 passes. PA-019 and PA-020 can be implemented independently once PA-036 passes. Other dependencies remain explicit; parallel work does not waive stage integration checks.

## Stage completion

| Phase/stage | Tickets | Required integrated result |
| --- | --- | --- |
| P0 | [PA-025](PA-025.md), [PA-026](PA-026.md), [PA-027](PA-027.md) | A feasible environment, proven browser boundary, and real engine/comparison evidence exist before application integration. |
| S1 | [PA-028](PA-028.md), [PA-029](PA-029.md), [PA-001](PA-001.md), [PA-002](PA-002.md), [PA-003](PA-003.md) | Accessible, clearly labeled fixtures cross authenticated validated services; startup and relevant checks are reproducible. |
| S2 | [PA-030](PA-030.md), [PA-031](PA-031.md), [PA-032](PA-032.md), [PA-004](PA-004.md), [PA-005](PA-005.md), [PA-006](PA-006.md) | Actual verified sessions protect persisted reports and artifacts; unrelated users cannot retrieve them. |
| S3 | [PA-033](PA-033.md), [PA-007](PA-007.md), [PA-008](PA-008.md), [PA-009](PA-009.md) | A concrete delivery policy is enforced; background work reaches a valid outcome despite duplicate delivery or outages. |
| S4 | [PA-034](PA-034.md), [PA-010](PA-010.md), [PA-011](PA-011.md), [PA-012](PA-012.md) | Both profiles produce real private product reports through the proven boundary with numeric budgets enforced. |
| S5 | [PA-013](PA-013.md), [PA-014](PA-014.md), [PA-015](PA-015.md) | Findings provide evidence, priorities, fixes, and verification without hiding missing coverage. |
| S6 | [PA-035](PA-035.md), [PA-016](PA-016.md), [PA-017](PA-017.md), [PA-018](PA-018.md) | An explicit comparison matrix drives immutable rescans and supported classifications. |
| S7 | [PA-036](PA-036.md), [PA-019](PA-019.md), [PA-020](PA-020.md), [PA-021](PA-021.md) | Numeric capacity and retention policies are enforced with deletion and safe recovery under failure. |
| S8 | [PA-022](PA-022.md), [PA-023](PA-023.md), [PA-024](PA-024.md) | The intended deployment has actual evidence for all first-version criteria. |

P0 is a prerequisite for S1. Complete each preceding stage and its integrated evidence before advancing to the next integration stage. P0 may use real controlled browser/engine checks; it does not expose a product scanning endpoint. S4 remains the first real product scan.

## Ready to implement

A ticket is ready when its explicit prerequisites and preceding phase/stage are complete, its required decision outputs exist, and its acceptance checks can be run in the available environment. Not started describes delivery status and does not imply readiness.

Decision tickets must name the selected mechanism and required values. Proof tickets must produce observed evidence, not a proposed design. A dependent implementation ticket cannot fill a missing prerequisite by silently choosing different behavior.

Numeric-policy outputs must state the value, unit, per-user/per-scan/global scope, configuration owner, rationale, enforcement point, and boundary behavior. Transport waits and session lifetimes are decided in their owning tickets; retry, browser, and operating policies are decided in PA-033, PA-034, and PA-036. Required fields cannot remain TBD when those tickets close.

## Workflow

- Read the relevant stage and owning specifications before implementation. Tickets divide the work; they do not override product, architecture, or domain requirements.
- Use Not started, In progress, Blocked, or Done in each ticket. Record blockers and next actions. An unavailable required environment or failed feasibility proof blocks dependents; unrun checks never count as passed.
- Implement the smallest complete scope and preserve prior behavior. Record technology choices and numeric policies in [implementation notes](../implementation.md), with executable configuration as the implementation source of truth once it exists.
- Fill completion evidence with changed files/PR links, exact checks/results, demonstrations, and limitations. Check acceptance items only when verified; mark Done only when the expected result is proven.
- Update [roadmap](../roadmap.md) checkboxes and stage status from actual evidence. Completing tickets alone does not waive integrated demonstrations.
- If a proof contradicts a required product promise, record the gap and resolve the design or scope explicitly. Do not quietly loosen acceptance to mark work done.
- Estimates and assignees remain unset until stack/capacity are known. Refine oversized work using actual implementation evidence, preserve IDs/references, and update dependencies when splitting.
- Preparing local tickets does not publish external issues or authorize outreach, paid provisioning, or service publication. Perform those actions under an authorized task.
- Post-release candidates stay in the roadmap; this backlog covers the first version and its necessary feasibility checks.
