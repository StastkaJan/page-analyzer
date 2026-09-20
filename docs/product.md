# Product specification

Status: intended first-version behavior; not implemented. The [README](../README.md) owns the product vision and deferred scope.

## User journey

1. Submit one public HTTP or HTTPS page and explicitly choose mobile or desktop.
2. Receive a scan reference and see queued, running, completed, partially completed, or failed status.
3. Open a retained report with category results, prioritized findings, evidence, and practical fixes.
4. Request a new scan of the same page and compare it with the original.

The first version is complete when this journey works without an operator running tools manually.

## Acceptance criteria

| Area | Required behavior |
| --- | --- |
| Submission | Reject malformed URLs, unsupported schemes, URL credentials, and prohibited destinations with an actionable message. Choose a device profile before queuing. |
| Background work | Accept valid work without waiting for the browser scan. Refreshing the page does not create another scan. |
| Progress | Show the persisted lifecycle state and timestamps. Do not invent percentage progress. |
| Completion | A completed report accounts for all planned checks, including legitimate not-applicable and manual-review outcomes. |
| Partial results | Retain useful output when some checks fail or are skipped. Clearly show missing coverage and its reason. |
| Failure | If no usable analysis survives, show a safe error and a way to request a new scan. Never display failure as a clean report. |
| Report | Show requested/final URL, profile, timestamps, engine versions, conditions, distinct category results, and coverage limitations. |
| Findings | Each finding has its source, affected element/resource where available, evidence, impact, priority rationale, estimated effort or unknown, suggested fix, and verification step. |
| Scores | Preserve engine-provided category scores with source and scale. Missing scores stay unavailable. Do not create an unexplained overall score. |
| Prioritization | Use deterministic rules and explain priority. Keep engine severity separate from product priority; estimates are not guarantees. |
| Consolidation | Combine only findings that represent the same issue and target; retain every source reference. |
| History | Retain immutable scan snapshots. Rescanning creates a new scan rather than overwriting the old report. |
| Comparison | Identify resolved, persistent, and new findings only where coverage is comparable; mark remaining findings unverified. Warn about changed profiles, engines, conditions, and final destinations. |
| Privacy | Reports and artifacts require access authorization. An unrelated user cannot retrieve them by knowing their IDs. |
| Interface | Submission, status, reports, and comparisons support keyboard operation, labeled controls, visible focus, and readable error/status text. |

## Interpretation rules

- Label performance results as laboratory measurements and acknowledge run-to-run variation.
- Automated accessibility findings are incomplete; preserve manual-review items separately.
- Technical SEO results do not promise rankings or indexing. Best-practice checks are not penetration testing.
- A recommendation must identify its supporting finding. Do not claim a fix will produce a specific improvement without evidence.
- Use engine guidance and curated recommendations initially. No LLM dependency is needed for the first version.

## Out of scope

Multi-page crawling, authenticated target pages, automatic website edits, scheduled monitoring, billing, team workflows, full security testing, real-user data integration, and AI explanations are deferred. See the README for later milestones.

## Product validation

Before expanding scope, show representative reports and scan comparisons to developers and agencies. Check whether the priorities and verification steps save time in their existing workflow. Pricing and a broader feature set remain hypotheses.
