# Implementation notes

Status: planning only. No application implementation exists yet. This file records technical decisions and verification requirements; [roadmap.md](roadmap.md) owns integration stage order and status, and [product.md](product.md) defines acceptance.

## Established direction

- One repository, a web interface, and two independently deployable backend services.
- The application service owns scan lifecycle, access control, and the application database.
- The scanning service owns isolated browser execution and engine normalization.
- Lighthouse is the initial foundation; recommendations use measured evidence and deterministic guidance.
- Public single-page scans and comparisons are the first-version scope.

## Open decisions

Resolve each decision in its owning ticket before starting dependent implementation. Record the selected option, values, evidence, and rationale here, replacing the open entry. All decisions below remain open; preparing these tickets has not selected a stack or demonstrated feasibility.

| Decision or proof | Owning ticket | Required output / implementation consumer |
| --- | --- | --- |
| Initial runtime/frontend/tools and local/deployment constraints | [PA-025](tickets/PA-025.md) | Concrete candidate versions/setup; input to feasibility proofs. |
| Browser isolation and enforceable egress feasibility | [PA-026](tickets/PA-026.md) | Observed boundary matrix and provisional numeric limits before S1; reused by PA-010. |
| Engine fields and comparison evidence feasibility | [PA-027](tickets/PA-027.md) | Real-derived fixtures, field mapping, supported classifications, and explicit limitations before schemas. |
| Executable schemas and authenticated service transport | [PA-028](tickets/PA-028.md), [PA-029](tickets/PA-029.md) | Validated payloads, service credentials/configuration, and finite transport timeout before PA-001. |
| User identity/session behavior and implementation | [PA-030](tickets/PA-030.md), [PA-031](tickets/PA-031.md) | Real browser principals, finite session lifetimes, expiry/revocation and protection behavior before PA-004. |
| Application database, schema, and setup | [PA-032](tickets/PA-032.md) | Reproducible initialization and ownership/uniqueness constraints before PA-004. |
| Artifact backend/capture and safe data handling | [PA-005](tickets/PA-005.md), [PA-006](tickets/PA-006.md) | Private validated references, capture limits, authorized access, and safe logging. |
| Queue/result delivery, attempt ownership, numeric retry/deadline policy | [PA-033](tickets/PA-033.md) | Failure/event table, lease/renewal, deadlines, attempts, and delay values before PA-007 through PA-009. |
| Browser/scan resource budgets | [PA-034](tickets/PA-034.md) | Measured finite defaults with enforcement owners before PA-010. |
| Finding identity, consolidation, priority, and effort | [PA-013](tickets/PA-013.md), [PA-014](tickets/PA-014.md) | Versioned mappings using P0 evidence and deterministic recommendation rules. |
| URL identity and comparison matrix | [PA-035](tickets/PA-035.md) | Supported inputs/classifications and unverified fallbacks before PA-016/PA-017. |
| Admission quotas and retention/deletion policy | [PA-036](tickets/PA-036.md) | Numeric windows/caps/durations and deletion timeline before PA-019/PA-020. |
| Concrete hosting configuration and rollback | [PA-022](tickets/PA-022.md), [PA-023](tickets/PA-023.md) | Deployment within proven constraints and demonstrated supported recovery before PA-024. |

## Decision and proof completion

A decision output must name the chosen mechanism and any required numeric values, not merely list alternatives. Record value, unit, scope, rationale, enforcement owner, expected boundary behavior, and the relevant configuration key once implemented. Required fields cannot stay TBD when the owning ticket closes.

A proof output must identify environment/tool versions, controlled inputs, exact commands, observed outcomes, and unsupported cases. A failed proof or unavailable required environment blocks dependent work; a written proposal is not equivalent evidence.

Use provisional limits for P0, then explicitly reconcile them in PA-033/PA-034/PA-036. Avoid divergent defaults in docs and code: record decisions here and link to authoritative executable settings once they exist. Numeric choices remain open until those tickets are executed.

## Delivery sequence

Follow the [roadmap](roadmap.md) for integration boundaries, feature checklists, expected results, and verification at each stage. Resolve the decisions above as each working slice needs them; keep delivery status in the roadmap rather than duplicating it here.

Use the [implementation tickets](tickets/README.md) to select work and follow dependencies. Individual tickets own task status and completion evidence; the roadmap owns stage status.

## Verification strategy

Use the project's selected test tools once they exist. Keep routine checks deterministic and small; add integration coverage where a fixture cannot prove the boundary.

| Area | Required proof |
| --- | --- |
| Input and egress | Invalid schemes/credentials rejected; private/local destinations blocked across IPv4/IPv6, redirects, DNS changes, and subrequests; permitted public pages work. |
| Lifecycle | Valid transitions, duplicate jobs/results, conflicting results, stale attempts, worker loss, failed dispatch, out-of-order events, and retry exhaustion. |
| Normalization | Representative engine fixtures preserve evidence, source scores, unavailable data, manual review, and partial failures. |
| Comparison | Persistent/resolved/new outcomes with comparable coverage; missing checks, changed profiles/versions, and incompatible identities stay unverified where needed. |
| Privacy and rendering | Unauthorized report/comparison/artifact access rejected; page-derived markup cannot execute in report views; sensitive values excluded from logs. |
| User journey | Submission through retained report and rescan works using keyboard controls, including partial and failed states. |
| Operations | Resource limits take effect; abandoned work is recovered; retention/deletion covers artifacts and reports without leaving publicly accessible remnants. |

For documentation-only work, verify local links and consistency with the README. Do not describe a planned check as passing before an executable implementation exists.
