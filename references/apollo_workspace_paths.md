Apollo workspace path and workflow notes:

## Directory meanings

- `/apollo_workspace/profiles/default` is a git repository for profile parameter/configuration work.
  Task work is separated by git branch, not by creating separate task profile directories.
  For example, use the `task1` branch in this repository for task1 profile/configuration changes.
- `/apollo_workspace/profiles/default/modules` is the default profile parameter/configuration directory to edit after checking out the correct task branch.
- `/apollo_workspace/modules` is the current workspace source-code directory.
  It is also a git repository. Different task source workspaces are managed with git branches.
  Use the same task branch name as the profile repository when a task requires source changes, so the final merge can combine task work cleanly.
  This source tree may also contain parameter/config files similar to those in the profile directory, but configuration should not be edited here by default.
- `/opt/apollo/neo/src` contains the full Apollo source code.
  It generally should not be edited directly because there is no suitable compile path and changes are hard to track.
  If a source change from this tree is truly necessary, copy the relevant source into the workspace first.
- `/apollo/modules` exposes the active profile configuration through symlinks.
  Use it to inspect what the active profile exposes, but make tracked edits under `/apollo_workspace/profiles/default/modules` on the correct task branch.
  Verified: editing an existing profile config under `/apollo_workspace/profiles/default/modules/...` is immediately reflected at the corresponding `/apollo/modules/...` path when the default profile is active.
- `/apollo_workspace/core/cyberfile.xml` lists package names in `repo_name`.
  Use those package names with `buildtool install <package-name>` only when installing/copying source packages into the workspace.

## Default edit scope

- Modify profile/configuration files by default.
- For this task, changes are generally expected to stay within the `planning` module unless investigation shows that another module must be touched.
- Before editing, inspect the current git branch in both `/apollo_workspace/profiles/default` and `/apollo_workspace/modules`.
- Keep task-specific changes on task-specific branches. Do not create task-specific profile directories such as `/apollo_workspace/profiles/task1`.
- Do not directly edit `/opt/apollo/neo/src`.
- If a source-code change under `/apollo_workspace/modules` is needed, ask the user for confirmation first.

## Profile configuration generation and updates

- Do not manually create missing profile package/config directories.
- If a needed config is absent from `/apollo_workspace/profiles/default/modules`, first ensure `/apollo_workspace/profiles/default` is on the correct task branch, then generate/update the profile config from `/apollo_workspace`:

  ```bash
  buildtool profile config init -p <package-names> --profile=default
  ```

- Profile parameters/configuration should be generated or updated with `buildtool profile config init ...`, not with `buildtool install`.
- To initialize the default profile with planning configuration, use:

  ```bash
  buildtool profile config init -p planning planning-base planning-open-space planning-park-data-center planning-interface-base planning-scenario-free-space planning-scenario-lane-follow-park planning-scenario-large-curvature planning-scenario-square planning-scenario-valet-parking-park planning-task-smooth-stop-trajectory-fallback planning-task-lane-borrow-path-generic planning-task-lane-change-path-generic planning-task-obstacle-nudge-decider planning-task-open-space-fallback-decider-park planning-task-open-space-path-planning planning-task-open-space-replan-decider planning-task-open-space-roi-decider-park planning-task-open-space-trajectory-optimizer-park planning-task-open-space-trajectory-post-process planning-task-reverse-path planning-task-reverse-speed planning-task-smooth-stop-trajectory-fallback planning-task-square-path planning-traffic-rules-speed-setting --profile=default
  ```

## Profile switching and task branches

- Use this command to make the default profile active:

  ```bash
  aem profile use default
  ```

- This changes what `/apollo/modules` exposes.
- Branches, not profile names, distinguish tasks. Switch task context with git branches inside `/apollo_workspace/profiles/default` and `/apollo_workspace/modules`.
- Even after switching profiles, make edits under `/apollo_workspace/profiles/default/...` so changes remain tracked.

## Task branch workflow

- For each task, use matching branch names in both repositories when possible:

  ```bash
  git -C /apollo_workspace/profiles/default switch <task-branch>
  git -C /apollo_workspace/modules switch <task-branch>
  ```

- If the branch does not exist yet and the user asks to start a new task branch, create it in both repositories with:

  ```bash
  git -C /apollo_workspace/profiles/default switch -c <task-branch>
  git -C /apollo_workspace/modules switch -c <task-branch>
  ```

## Build and install commands

- Run Apollo workspace, profile, and buildtool commands from `/apollo_workspace`.
- Compile and install the planning module workspace with:

  ```bash
  buildtool build -p modules
  ```

- The user will run builds. Do not run compilation by default unless explicitly asked.
- After making changes, tell the user the exact package-level build command they should run for the modified package or packages, using:

  ```bash
  buildtool build -p modules/<modified-package>
  ```

  For example, if only `modules/planning` was modified, output `buildtool build -p modules/planning`.
- Use `buildtool install <package-name>` only to install/copy source packages into the workspace.
- Do not use `buildtool install` to generate profile configuration.

## Permission and user-environment notes

- Most Apollo commands should run as the `ng` user.
- Do not bypass problems by changing environment variables such as `HOME`, `USER`, or `LOGNAME`.
- If `buildtool profile config init ... --profile=default` fails because it tries to write to `/home/ng/.bashrc`, request user authorization instead of working around it through environment changes.
