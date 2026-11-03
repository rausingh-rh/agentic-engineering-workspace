---
name: aew
description: "Assess an engineering request from a direct prompt or Jira context, resolve and inspect the relevant repository, and produce an evidence-based feasibility and validation plan. Use before implementation for features, enhancements, migrations, architecture changes, or unclear technical work."
---

# Agentic Engineering Workspace

Turn an engineering request into a concise, reviewable technical discovery report. Accept direct requests, pasted Jira content, or Jira links.

## Inputs

Require a clear objective. Treat the following as optional context:

- Jira key, link, or text;
- component, image, repository, branch, or release;
- scope boundaries and non-goals;
- required validation or live-cluster expectations.

Do not require a repository URL. Use `repository-resolution` before selecting one.

## Workflow

Read [workflow.md](references/workflow.md) and follow its phases. Use the sibling `repository-resolution` skill to resolve a repository and ref before making code-level recommendations.

## Safety boundaries

- Do not guess a repository or branch when the evidence is ambiguous.
- Do not modify code, create branches, open pull requests, update Jira, or use a live cluster unless the user explicitly asks for that action.
- Clearly separate observed repository evidence from assumptions.
- Keep credentials out of commands, logs, reports, and responses.

## Output

Return a feasibility report containing the objective, selected repository and confidence, relevant code areas, recommended approach, validation plan, risks and limitations, open questions, and next action.
