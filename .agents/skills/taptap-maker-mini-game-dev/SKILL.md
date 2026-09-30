---
name: taptap-maker-mini-game-dev
description: Develop, diagnose, tune, validate, build, publish, and operate TapTap Maker / 创意工坊 Lua mini games. Use for new Maker games and existing Maker repositories, including gameplay, UI, economy, ads, cloud saves, leaderboards, phone performance, analytics, release assets, KTG builds, and QR/mobile testing.
---

# TapTap Maker Mini Game Dev

## Cloud Execution

Read [cloud setup and command equivalents](../../../docs/taptap-cloud.md) when working in the cloud. Resolve the actual Linux game directory and use the documented Bash equivalents for Windows commands. Check Maker authentication, project binding, and tool availability separately.

Build TapTap Maker games as phone products, not merely scripts that compile. Preserve the player's existing data and the repository's current truth while improving gameplay, stability, presentation, and release readiness.

## Start With The Real Project

1. Resolve the exact project root. Do not infer it from the current shell directory or reuse another game's project ID, cloud keys, ad config, asset paths, or build target.
2. Read applicable `AGENTS.md`, `CLAUDE.md`, the latest handoff, and only the task-relevant documents. Treat current code and runtime behavior as newer than old plans or conversation summaries.
3. Inspect `git status --short --branch`, recent commits, `.project/project.json`, and Maker binding before editing. Preserve unrelated tracked changes, workbooks, generated media, and untracked folders.
4. Trace the actual state/data path before changing behavior: definition -> unlock/config -> runtime use -> UI feedback -> settlement/statistics -> save/load/migration.

For exact Maker/Git/build procedure, read [references/maker-workflow.md](references/maker-workflow.md).

## Choose The Work Mode

- **Explain or audit:** inspect and report evidence; do not mutate or build unless asked.
- **Diagnose:** reproduce or trace the failure and identify the root cause before implementing a fix.
- **Implement:** make the smallest complete change, add focused verification, and stop when the requested behavior is covered.
- **KTG:** perform local validation, Maker status/sync, selective Maker submit/build, and post-build status/log checks. A normal Git commit is not KTG.
- **KTGS:** perform KTG first, then generate/upload the latest mobile test version. For the elevator-dispatch project, do not display the unchanged QR code unless asked.
- **Visual or feel change:** local correctness is insufficient; explicitly require Maker preview or phone acceptance for appearance, touch feel, and frame pacing.

If the project's handoff defines different shorthand, follow that local definition.

## Implement Complete Features

- Make every requested effect real, not only visible in copy. A progression item must cover its data definition, unlock, purchase, equip/configure path, runtime effect, feedback, save/load, old-save migration, and relevant statistics.
- When removing or replacing content, remove both presentation and runtime definitions, then migrate old inventory/state. Hiding a card is not removal.
- Avoid speculative frameworks. Follow the repository's existing architecture. If `main.lua` is already large or near Lua's top-level local-variable limit, add a focused module and minimal wiring instead of more top-level locals.
- Keep authoritative values in one configuration source when practical. If a workbook is the balance source of truth, update it together with runtime configuration only when requested and safe to edit.
- Never silently change adjacent mechanics, formatting, or user-owned assets.

## Phone-First Quality Gates

Before calling a UI or interaction finished, check:

- short and tall portrait screens, narrow width, and notched safe areas;
- no clipped baselines, ellipsized essential copy, overlap, or controls outside the hit area;
- content that can grow uses scrolling or paging, with clipping/culling and drag-versus-tap separation;
- dragged objects render above ordinary content, while scene depth still follows the intended spatial ordering;
- primary actions have clear state, feedback, and recovery; important unlocks/rewards use a modal, toast, red dot, or animation appropriate to their importance;
- actual touch thresholds are tested on a phone, not inferred from mouse preview.

For ads, saves, fonts, time, performance, async work, large lists, and leaderboards, read [references/runtime-reliability.md](references/runtime-reliability.md) whenever the task touches those systems or a freeze/white-screen/data-loss report.

## Design Around Player Comprehension

- In the first 10 seconds, the player should understand the goal; within 30 seconds, complete a meaningful action and receive positive feedback; within one minute, see a next objective or growth choice.
- Teach the first interaction as short, forced-in-context actions rather than a long explanation. Make the target unmistakable without hiding the game.
- Prefer the long-term arc `manual play -> assisted play -> automated execution -> player strategy`. Automation should remove repetition while leaving meaningful priorities, routing, risk, and timing decisions.
- Tune difficulty against the player's unlock stage and realistically affordable upgrades, not only against maximum theoretical power. Preserve early readability and late-game power fantasy while using multiple understandable pressure sources to limit infinite automation.
- Balance with at least three player cohorts: no ads, roughly half of optional ads, and all ads. Ads should accelerate or rescue, not become the only viable progression path.
- Never generate daily tasks, content, or goals before the required mechanic is unlocked.

For progression, economy, difficulty, retention, feedback, and analytics reasoning, read [references/game-design-and-economy.md](references/game-design-and-economy.md).

## Release Discipline

Treat release as a separate acceptance mode. Check test entrances, FPS/debug UI, destructive save tools, developer copy, ad configuration, cloud migration, safe areas, first-load resources, phone ads, and analytics before submission. Promotional assets must honestly reflect the game; use real gameplay screenshots for gameplay claims.

Read [references/release-and-liveops.md](references/release-and-liveops.md) for release preparation, phone QA, analytics funnels, and post-launch interpretation.

## Definition Of Done

Do not collapse these into one claim:

1. **Local validation:** syntax plus focused behavior checks passed.
2. **Maker build:** the exact bound project was selectively submitted; remote build and preview refresh succeeded.
3. **Runtime check:** watcher/logs show no new relevant error after exercising the path.
4. **Phone acceptance:** the feature was tested on the target device for ads, safe areas, touch, visuals, or performance when those matter.
5. **Release confirmation:** a public/review/mobile build is available only when that separate workflow actually completed.

Report only the important outcome: behavior changed, checks run, Maker/build state, commit hash, phone-test state, and preserved unrelated files. State limitations honestly; syntax success is not gameplay proof, and a successful remote build is not phone acceptance.
