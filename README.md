# Agentic Engineering Workspace

Agentic Engineering Workspace (AEW) is a portable collection of agent skills for turning an engineering request into an evidence-based implementation path.

It accepts a direct request or Jira context, resolves the relevant repository when possible, inspects code and documentation, and produces a feasibility and validation plan. It does not modify application code, open pull requests, update Jira, or use a cluster unless a user explicitly requests that next phase.

## Included skills

| Skill | Invocation | Purpose |
| --- | --- | --- |
| `aew` | `/aew` | Main engineering-discovery workflow: request intake, repository investigation, feasibility, risks, and validation planning. |
| `repository-resolution` | `/repository-resolution` | Resolves a supported component or image to the correct repository and release reference without guessing. |

The repository separates the main workflow from repository mapping. New skills, such as implementation, validation, or Jira reporting, can be added beside them without changing the `aew` interface.

## Layout

```text
.
├── .agents/
│   └── skills/                         # canonical, portable source
│       ├── aew/
│       │   ├── SKILL.md
│       │   └── references/workflow.md
│       └── repository-resolution/
│           ├── SKILL.md
│           └── references/component-repository-map.yaml
├── examples/
│   └── ztwim-federation-disable.md
└── README.md
```

`.agents/skills/` is the only canonical skill location in this repository. Do not keep copied skill bodies in tool-specific directories; use a symlink or copy at installation time where a tool needs a different discovery path.

## Quick start

Clone this repository, open it in an agent that supports `SKILL.md`, and invoke:

```text
/aew

Assess this engineering request. Planning only: do not change code or use a cluster.
<paste the Jira ticket or direct request>
```

Example:

```text
/aew

Assess the feasibility of disabling ZTWIM federation without recreating the resource.
Target component: ZTWIM.
Planning only: do not change code or use a cluster.
```

## Installation

### Cursor

Cursor discovers project skills from `.agents/skills/`, so opening this repository in Cursor is enough. Start a new Agent chat, type `/`, and choose `aew`.

To use AEW while working in another repository, link both skills into that repository:

```bash
mkdir -p .agents/skills
ln -s /path/to/agentic-engineering-workspace/.agents/skills/aew .agents/skills/aew
ln -s /path/to/agentic-engineering-workspace/.agents/skills/repository-resolution .agents/skills/repository-resolution
```

Reload the Cursor window or open a new Agent chat after adding the links.

### Claude Code

Claude Code discovers project skills from `.claude/skills/`. From the project where you want to run AEW, link the canonical skills:

```bash
mkdir -p .claude
ln -s /path/to/agentic-engineering-workspace/.agents/skills .claude/skills
```

Start a new Claude Code session and invoke `/aew`.

### Codex

Codex supports repository-scoped skills from `.codex/skills/`. Link the canonical skills into the target repository:

```bash
mkdir -p .codex
ln -s /path/to/agentic-engineering-workspace/.agents/skills .codex/skills
```

Start a new Codex session and ask it to use `$aew`, or make an engineering-discovery request that matches the skill description.

### GitHub Copilot

GitHub Copilot supports project skills from `.agents/skills/`, so the repository works directly in supported Copilot clients. In Copilot CLI, start a new session or run `/skills reload`, then use `/aew` in your request.

For another repository, use the same `.agents/skills/` symlink commands shown for Cursor. GitHub Copilot cloud agent and supported IDE agent modes can load skills committed in that location.

## Repository resolution

`repository-resolution` uses only confirmed mappings. It resolves an explicit repository first, then known component/image mappings, and handles release repositories by reading the pinned submodule revision. If the repository or branch cannot be determined safely, it returns `not_found` or `ambiguous` instead of guessing.

Extend `component-repository-map.yaml` only after the component owner confirms the image/component name, repository URL, and release pattern.

## Adding future skills

Add a skill as a sibling under `.agents/skills/`:

```text
.agents/skills/<skill-name>/SKILL.md
```

Use lowercase hyphenated names. Keep the skill focused on one repeatable job. Likely next additions are `implementation`, `validation`, and `jira-context`; create them only when their workflow and boundaries are agreed.

## Safety

AEW is planning and discovery by default. Any implementation, branch creation, pull request, Jira update, or live-cluster action requires explicit user approval.

## License

Add the team-approved license before publishing the repository externally.
