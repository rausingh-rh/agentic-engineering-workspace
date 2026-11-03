---
name: repository-resolution
description: "Resolve a supported OpenShift component or container image to its source repository and exact release ref. Use before code investigation when a direct prompt or Jira ticket does not include a repository URL."
---

# Repository Resolution

Resolve source repositories without guessing. This skill is shared by investigation workflows such as PoC/spike discovery, CVE analysis, and bug triage.

## Inputs

Accept any available evidence:

- explicit repository URL and optional branch;
- component or image name from a direct prompt;
- Jira summary, labels, component, image name, or target version/branch.

## Resolution order

1. An explicit full repository URL is authoritative. Validate the requested branch/ref if supplied.
2. Match a supported image/component using [component-repository-map.yaml](references/component-repository-map.yaml): exact match first, then its defined prefix rules, then its defined keyword rules. When Jira context supplies a release branch covered by a `release_submodules` rule, prefer that release mapping; otherwise use the direct repository mapping.
3. For a release-repository mapping, clone the release repository at the verified release branch, read `.gitmodules`, and use the pinned submodule commit. Never use a `.gitmodules` branch as a substitute for the pinned commit.
4. If no mapping applies, return `not_found`; provide the evidence needed for the caller to add a confirmed mapping or supply a repository URL.

Do not use broad fuzzy matching or infer a repository from a namespace that maps to more than one source repository.

## Branch and release rules

Use an explicit branch/ref first. Otherwise use the map's default branch or derive the release branch only with the documented rule. Verify the candidate ref exists. If it does not exist, stop and ask for the correct branch; do not fall back to a different release or default branch silently.

## Result

Return and record:

```yaml
status: resolved | not_found | ambiguous
input: <image/component/url>
repository_url: <url>                 # when resolved
ref: <branch or pinned commit>         # when resolved
repository_pattern: direct | release_submodule
confidence: user_provided | exact_match | prefix_match | keyword_match
evidence: <why this match was selected>
```

`keyword_match` is valid only for patterns marked unambiguous in the map. If several repositories remain plausible, return `ambiguous` and ask the caller to choose.

## Maintenance

The map contains only mappings carried over from the validated compliance workflow. Add a mapping only after its component owner confirms the image/component, repository URL, and branch/release pattern.
