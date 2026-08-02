---
name: apollo
description: Work in the Apollo autonomous driving workspace with branch-based task isolation, buildtool, source-editing, and scenario constraints. Use when a user asks about Apollo workspace changes, planning module configuration, task branch files, Apollo build/profile commands, or the 2026 contest task scenarios.
---

# Apollo Workspace

Use this skill before changing or investigating Apollo workspace files under `/apollo_workspace`, `/apollo/modules`, or `/opt/apollo/neo/src`.

## Required References

- Read `references/apollo_workspace_paths.md` before editing files, running Apollo workspace commands, initializing profile config, switching profiles, installing packages, or discussing which Apollo paths should be modified.
- Read `references/apollo_task_scenarios.md` when the task involves contest scenarios, task1 scoring behavior, scenario JSON files, map IDs, start/end coordinates, or planning behavior for specific problems.

## Operating Rules

- Prefer profile/configuration changes under `/apollo_workspace/profiles/default/modules` on the task-specific git branch.
- Do not edit source code under `/apollo_workspace/modules` unless the user confirms that a source-code change is acceptable.
- Do not edit `/opt/apollo/neo/src` directly.
- Do not manually create missing profile config directories. Use the documented `buildtool profile config init ... --profile=default` flow from the workspace reference.
- Run Apollo workspace, profile, and buildtool commands from `/apollo_workspace`.
- Do not run compilation by default; the user normally runs builds unless they explicitly ask Codex to build.
- If a build or profile command fails because it needs to write `/home/ng/.bashrc`, request user authorization instead of changing `HOME`, `USER`, `LOGNAME`, or similar environment variables.

## Practical Workflow

1. Load the relevant reference file or files.
2. Inspect the current files with `rg`, `sed`, `ls`, or similar read-only commands.
3. Confirm the active branches in `/apollo_workspace/profiles/default` and `/apollo_workspace/modules` match the task before editing.
4. If only configuration is needed, edit existing files under `/apollo_workspace/profiles/default/modules`.
5. If required profile configuration is missing, initialize it with the package names from the workspace reference before editing.
6. Ask before source-code edits or builds unless the user has already requested them.
