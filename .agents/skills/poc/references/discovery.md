# Discovery that informs experiments

Start with the user's intended decision: learn how the feature works, choose an approach, prove an integration, expose implementation risk, or reject an unsuitable design. Write the user's own understanding as falsifiable statements; do not silently treat it as fact.

## Sources and concepts

Use official specifications, versioned upstream docs, maintained implementations, and the selected repositories. Search for prior art before proposing custom code. Use available connectors or browsing; if offline, use local source and label external claims unverified. Jira access is optional: accept pasted context when no connector is available. Do not require an OpenShift mapping for an explicitly supplied project.

For each important concept, explain its role, inputs/outputs, trust or ownership boundary, lifecycle, and relationship to adjacent concepts. Follow one request through the system, including initialization and steady state. Draw a small topology or sequence diagram if it clarifies the mechanism. Track bootstrapping separately from ongoing reconciliation, renewal, recovery, and teardown.

Record sources as `S1`, `S2`, etc., with URL or local path, commit/tag/version, access date, relevant symbol/section, and the claim they support. Prefer commit permalinks for code. Record dirty working-tree changes when they affect behavior. A citation to `main` does not identify the version tested. When docs and code disagree, investigate the deployed version and state what remains uncertain.

## Feature-focused repository map

Resolve repositories independently and assign roles: target product, upstream implementation, controller/operator, API/schema, client/SDK, deployment charts, test harness. Follow dependencies only as far as they affect the question. An unresolved optional repository need not block independent work.

Inspect applicable repository instructions, current commit and local changes, relevant APIs and validation, configuration generation, execution/reconciliation, persistence, external calls, ownership, tests, and release pins. Explain:

| Area | What exists today | Proposed interaction/change | Evidence | Validation gap |
| --- | --- | --- | --- | --- |
| Relevant path/symbol | Current mechanism | Feature's effect | Source ID and ref | Experiment needed |

Distinguish upstream support from what the product exposes or ships. A plugin existing upstream is insufficient if its binary, API, permissions, configuration, or lifecycle is unavailable in the selected product release.

## Alternatives and unknowns

Compare the smallest existing supported path, reuse of another upstream mechanism, and custom extension where they are plausible. Evaluate correctness, capability gaps, security/privilege requirements, portability, bootstrap dependencies, steady-state behavior, recovery, operational burden, performance, maintenance, and upstream acceptance risk. Mark estimates as estimates; do not fabricate timings or line counts.

For each approach, state what must be true, the hardest implementation step, what evidence would reject it, and the cheapest discriminating test. An approach may work technically but lose on deployment complexity or maintainability. Keep rejected approaches and their reasons in the report.

Research is ready for experiment planning when the agent can explain the mechanism and repository integration, identify the unresolved claims, and derive test prerequisites. It need not read the entire repository or exhaust every possible design. If uncertainty only a live experiment can resolve remains, carry it explicitly into the plan.
