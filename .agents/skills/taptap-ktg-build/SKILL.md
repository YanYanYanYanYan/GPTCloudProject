---
name: taptap-ktg-build
description: Use when the user says `KTG` or asks for a fast Maker submit/build for the confirmed elevator-dispatch project, using its actual local or cloud root.
---

# KTG 快速提交构建

## Cloud Execution

Read [cloud setup and command equivalents](../../../docs/taptap-cloud.md). This shortcut remains specific to elevator-dispatch. Confirm that game's actual source and Maker binding before using it in the cloud; this skills repository does not include that game.

Use this skill for the elevator-dispatch project only.

## Fixed Flow

1. Confirm the exact project and branch state: `git status --short --branch`, `.project/project.json`, and Maker readiness.
2. Validate only the changed Lua files with `npx -y luaparse` and `git diff --check`.
3. If Maker reports the branch is behind by 1-2 commits, do one bounded `stash -> pull --rebase -> pop` cycle for the edited Lua files only, then revalidate.
4. Submit/build through Maker MCP with `maker_status_lite` followed by `maker_build_current_directory`, using `target_dir`, `entry: "main.lua"`, `scriptsPath: "scripts"`, and only the changed script files.
5. Report only the result that matters: success or blocker, commit hash, preview refresh/build status, and any remaining untracked files.

## Rules

- Keep retries tight. If the first build attempt is blocked by remote sync, fix sync once and retry once.
- Do not switch to a generic git push as a substitute for Maker submit/build.
- Do not spend time on broad repo searches, design discussion, or unrelated files unless the user explicitly asks.
- If Maker MCP is unavailable, say so directly instead of inventing a different build path.
