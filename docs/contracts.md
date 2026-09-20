# Domain and service contracts

Status: proposed semantic contract for the first version. Field names below are logical names, not a committed wire schema. Define one executable schema alongside the first producer and consumer; do not maintain a second handwritten schema in these docs.

## Core records

| Record | Required information |
| --- | --- |
| Scan | `scan_id`, private owner/access reference, requested URL, profile, status, creation/start/finish times as applicable, current attempt, accepted result reference, safe failure code when relevant. |
| Job | `contract_version`, `scan_id`, `attempt_id`, validated target URL, profile/configuration version, execution deadline. |
| Result | `contract_version`, `scan_id`, `attempt_id`, completion outcome, requested/final URL, timestamps, engine versions, actual scan conditions, category results, check coverage, findings, artifact references, safe errors. |
| Check outcome | Stable engine/rule identity, outcome, and reason where needed. Outcomes: passed, finding, not applicable, manual review, failed, skipped. |
| Category result | Category identifier, engine/source, optional score with original scale, coverage state, and limitations. |
| Finding | Stable rule identity, category, source references, target identity, evidence, severity or unknown, product priority and rationale, effort estimate or unknown, suggested fix, verification step. |
| Artifact reference | Opaque identifier, owning scan/attempt, content type, size, and retention metadata. Never a public bucket URL by default. |

Use stable IDs and unambiguous UTC timestamps. Missing values are distinct from zero. Engine-specific details may be retained as evidence without becoming required fields for all engines. Validate supported contract versions at both boundaries; reject unsupported versions without attempting partial interpretation.

## Lifecycle

Wire status names: `queued`, `running`, `completed`, `partially_completed`, `failed`.

| From | Allowed destination | Condition |
| --- | --- | --- |
| queued | running | An eligible attempt starts execution. |
| queued | failed | Work cannot start within its dispatch/deadline policy. |
| running | queued | A transient failure permits another bounded attempt; prior attempt is fenced. |
| running | completed | All planned checks have an accounted-for usable outcome; no execution gaps remain. |
| running | partially_completed | Usable analysis exists but at least one planned check failed or was skipped. |
| running | failed | No usable analysis remains and retry is unavailable or exhausted. |

Terminal states are immutable. A rescan always gets a new `scan_id`. Passing, reported findings, not-applicable checks, and explicit manual-review outcomes can all belong to a completed scan; completed does not mean the page has no problems.

The application reconciles out-of-order start/result delivery without losing a valid current result; a missing start notification must not strand an otherwise finished scan. Accept a result only for the current eligible attempt. Re-delivery of the same result is a no-op; conflicting payloads for an accepted attempt are rejected and recorded. Old attempt events cannot regress state or overwrite a report.

## Findings and priority

- Use a stable rule ID plus normalized affected target to match findings; never use translated titles, recommendation prose, or measured numeric values as identity.
- Version the matching/normalization rules. Preserve original evidence and source references separately.
- Consolidate cross-engine findings only with an explicit mapping and compatible targets. Different issues on the same element remain separate.
- Keep source severity and scores intact. Define product priority/effort rules with representative fixtures when implementing report presentation; unknown effort stays unknown.
- Prefer deterministic recommendations based on rule IDs and observed evidence. Documentation references should come from curated or trusted engine guidance.

## Comparison

Only compare scans the requester can access and that refer to the same requested page under the defined URL identity policy. Preserve meaningful query parameters; do not drop them merely to force a match. Define that policy in implementation before enabling comparisons.

| Classification | Evidence required |
| --- | --- |
| Persistent | The same stable finding appears in both scans. |
| Resolved | A previous finding is absent and the later scan successfully evaluates equivalent rule and target coverage. |
| New | A later finding is absent from an earlier scan that successfully evaluated equivalent rule and target coverage. |
| Unverified | Missing/failed/skipped/manual coverage, incompatible identity rules, or changed conditions prevent a supported conclusion. |

Absence alone never proves resolution. If the engine cannot provide sufficient evidence of comparable coverage, use unverified. Label not-applicable or removed-target cases explicitly instead of assuming the original defect was fixed. Include warnings for changed device profiles, engines, scan conditions, or final destinations; only classify affected findings when equivalence can be established. Show score changes as observations, not proof of causation.

## Errors

Use stable machine-readable codes plus safe user-facing messages. Distinguish invalid input, prohibited target, queue/deadline failure, navigation failure, resource limit, engine failure, and unsupported contract. Internal diagnostics can carry correlation IDs but must not expose secrets, captured content, or internal network details to report viewers.
