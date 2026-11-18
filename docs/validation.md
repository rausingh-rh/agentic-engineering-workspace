# Validation record

Validated on 19 November 2026 (Asia/Kolkata). This records development validation of the skills and helper, not qualification of a live product or cluster.

## Automated checks

`python3 -m unittest discover -s tests -v` passed **29 tests** on local Python 3.14.7 and Python 3.12. The tests cover record initialization, missing/escaping evidence, requirement coverage, recommended versus rejected approaches, blocked required tests, cleanup/retention, malformed inputs, explicit cluster targeting, bounded preflight, suppressed raw errors, skill metadata, relative links, and local symlink installation.

All three skills passed the skill-creator frontmatter validator. Python syntax, mapping/workflow YAML parsing, and `git diff --check` passed. CI is configured to run the offline suite on Python 3.10 and 3.14; hosted CI and Python 3.10 were not run during this local validation.

## Independent local PoC

A fresh agent context received the [Counter fixture](../tests/behavioral/fixtures/counter/README.md), the `poc` skill, and the local-execution request in the [behavioral scenarios](../tests/behavioral/README.md). It was not given the evaluator's rubric or an intended implementation. It used local Python/SQLite, without network, cluster access, external dependencies, or changes to the fixture.

The agent researched the code, recorded requirements and alternatives, produced runnable prototypes, executed 11 experiments, preserved observations, cleaned up runtime resources, and wrote an implementation recommendation:

| Approach/case | Observed result |
| --- | --- |
| Existing behavior | Retrying a lost +5 response produced total 10; baseline defect reproduced |
| Memory cache | Same-process retry worked; fresh-process retry duplicated the increment |
| Separately committed receipt | A synchronized race and a crash between effect and receipt both duplicated the increment |
| Atomic effect and receipt | Six required experiments passed, including replay/conflict handling, concurrent processes, postcommit/precommit process death, existing-data initialization, and lock-timeout recovery |

Eight experiment assertions passed and three failed, providing evidence to reject alternatives. Twelve concurrent duplicate processes produced one increment under the recommended approach. All 13 resource-ledger entries were resolved: runtime resources deleted and the review artifacts explicitly retained. The original fixture's hashes were unchanged. The final report limited its recommendation to the tested local behavior and identified API, retention, and production integration work.

The forward-test found a real defect: `--ready` accepted an empty `plan.md` despite the documented requirement. The helper now rejects blank/unchanged plans and blank/unchanged report/runbook files; regression tests cover these cases. A separate consistency fix rejects completion when required experiments were blocked or skipped. The helper was updated during evaluation; its hash changes and the historical failing probe were retained. Counter experiment code was run-local and unaffected. The finished run passed both readiness and completion checks against the final helper.

The local review copy of the full run is under `.work/validation/poc-forward-test/` in the development checkout (ignored by Git). Its historical commands refer to the original temporary run location. The scenario and fixture are committed so another reviewer can run a fresh evaluation with their chosen agent host.

## Remaining qualification

No live Kubernetes/OpenShift deployment or teardown was performed. Preflight command behavior was checked with mocked CLI responses. No host picker integration or end-to-end execution was tested in Claude Code, Cursor, or Copilot; their documented discovery paths are provided, and the instructions avoid host-specific APIs. Research-before-access, cleanup drift, and conflicting-evidence scenarios are available for further behavioral evaluation. A live pilot with a selected feature and authorized environment is the next qualification step.
