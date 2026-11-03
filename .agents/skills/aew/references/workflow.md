# AEW Discovery Workflow

## Phase 1: normalise the request

Extract the objective, component names, scope, constraints, desired outcome, validation expectations, and live-cluster requirement. If the request is too broad to investigate, ask for a concise objective.

## Phase 2: resolve the repository

Run `repository-resolution` with all available repository, component, image, Jira, and release evidence.

- Continue when it returns one resolved repository and reference.
- Stop and request clarification when it returns `not_found` or `ambiguous`.
- Do not infer a repository from generic keywords alone.

## Phase 3: inspect and assess

Inspect the resolved repository before recommending an implementation. Find relevant APIs, controllers/services, configuration, tests, documentation, ownership, and release constraints.

Classify the result as one of:

- **feasible** — a bounded approach is supported by evidence;
- **feasible with constraints** — viable, with explicit dependencies or risks;
- **needs discovery** — insufficient evidence for a responsible recommendation.

## Phase 4: define validation

Propose the smallest proof that validates the objective: existing automated tests first, then local/integration validation, then a disposable live-cluster plan only when requested. State setup, expected observations, cleanup, and rollback boundaries for any live-cluster work.

## Phase 5: report

Provide:

1. executive summary and feasibility classification;
2. objective and scope;
3. repository URL, branch/commit, confidence, and resolution evidence;
4. relevant implementation areas;
5. recommended approach and alternatives;
6. validation plan;
7. risks, limitations, and open questions;
8. recommended next action.

Planning ends here. Implementation and external actions require a new explicit request.
