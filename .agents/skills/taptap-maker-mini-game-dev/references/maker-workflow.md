# Maker Project And Build Workflow

Read this reference for project setup, implementation, KTG/KTGS, remote synchronization, or build failures.

## Project Intake

1. Resolve the exact repository root and run all commands there.
2. Read applicable project instructions and the latest handoff. Use UTF-8 explicitly for Chinese handoffs if PowerShell decoding is wrong.
3. Inspect:

   ```powershell
   git status --short --branch
   git log -5 --oneline --decorate
   git diff --stat
   git diff --check
   Get-Content -LiteralPath .project/project.json -Raw
   ```

4. Record the project ID, entry, scripts path, current version, branch, tracked edits, and untracked user assets. Never borrow those values from another Maker game.
5. Search first with `rg`; read the smallest coherent code regions. Verify all call sites before changing shared state or helpers.

## New Project

When creating a new Maker project, first verify the Maker CLI/MCP environment, then initialize a bound project using the currently installed Maker workflow. Do not hard-code a remembered CLI version or assume the newest package is compatible.

Start with a playable, measurable MVP:

- one obvious core action;
- one success/failure loop;
- one visible progression choice;
- one short contextual tutorial;
- one natural optional-ad point;
- one comparison target such as a personal record or leaderboard.

A menu and passive event cards may compile but still fail as a game. Test tension, agency, feedback, and replay motivation before building large meta systems.

## Editing Rules

- Use `apply_patch` for manual edits and preserve current formatting/style.
- In large Lua entry files, watch the 200-local limit. Prefer a focused module with a small integration surface when the entry is already crowded.
- Lua functions can return multiple values. When an API such as a button constructor is inserted into a table and only its first result is intended, parenthesize the call.
- Do not guess Maker resource paths, enum numbers, coordinate systems, physical sizes, or NanoVG lifecycle. Read the local engine docs/examples and inspect the asset tree.
- Initialize and probe fonts before drawing text. Do not call an undeclared drawing helper merely because a similarly named helper exists in another project.
- Centralize cross-cutting state changes such as cash, saves, ads, and analytics to prevent one feature bypassing migration or telemetry.

## Local Validation

Validation must match risk. The minimum for changed Lua files is:

```powershell
npx.cmd -y luaparse <changed-file.lua> > $null
git diff --check
git status --short --branch
```

For multiple files, parse every changed Lua file or all `scripts/**/*.lua`. Add focused behavior harnesses for configuration, migration, reward, ranking, and state transitions. Fengari's Node API can compile Lua when a CLI wrapper is broken; do not describe a wrapper failure as a Lua failure. If `emmylua_check` is missing, report the dependency gap instead of claiming LSP validation.

Static parsing does not prove runtime behavior. For a bug, retain a minimal reproduction or assertion that fails before and passes after the change.

## Maker Status And Remote Sync

Before building, use Maker status for the exact `target_dir`. Verify binding and remote sync state.

If remote is ahead while local work exists:

1. identify only the files changed for the current task;
2. stash those files, and separately preserve overlapping user-owned tracked files if needed;
3. `git pull --rebase origin main`;
4. pop the scoped stashes;
5. resolve only real overlaps and rerun every local check.

Example pattern:

```powershell
git stash push -m "codex-maker-work" -- scripts/changed_file.lua
git pull --rebase origin main
git stash pop
```

Do not recursively delete, reset, or clean the workspace to solve sync issues. Keep `.codex_tmp`, output renders, workbooks, and local asset folders out of submission unless explicitly requested.

## Maker Build

Use the Maker MCP build/submit tool, normally `maker_build_current_directory`, with:

- exact absolute `target_dir`;
- selective changed runtime files;
- short Chinese or project-appropriate message;
- `timeout_ms: 600000` unless the environment dictates otherwise.

Respect the project's configured `entry` and `scriptsPath`. If `scriptsPath` is `scripts`, an entry commonly remains `main.lua`; passing `scripts/main.lua` may become `scripts/scripts/main.lua` on the service.

If Maker MCP is unavailable, search the available tools once and report the limitation. Do not substitute ordinary `git commit/push` and call it a Maker build.

After success, verify:

- remote build reached completion;
- preview refresh succeeded;
- returned commit is on the expected branch/remote;
- only intended files entered the commit;
- runtime watcher/log state has no new relevant error after the path is exercised.

## KTG And KTGS

- **KTG:** local checks -> Maker status/safe sync -> selective Maker submit/build -> remote/preview status -> concise report.
- **KTGS:** complete KTG first -> generate/upload the latest mobile test version -> confirm mobile-test status. QR display is a user/project preference, not required when the QR is stable and already saved.

Never claim a feature is live, public, or phone-tested merely because KTG succeeded.
