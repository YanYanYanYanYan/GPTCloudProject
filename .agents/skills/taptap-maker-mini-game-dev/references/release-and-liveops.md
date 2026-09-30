# Release And Live Operations

Read this reference before review/public submission, when preparing store assets, or when interpreting post-launch data.

## Release-Candidate Cleanup

- Disable or remove test entrances, cheat buttons, destructive cloud tools, debug/FPS overlays, temporary build labels, and developer-only copy according to the user's release scope.
- Search for hidden test gestures and title-tap backdoors, not only visible buttons.
- Keep a recoverable internal testing strategy, but do not expose it in the public runtime unless explicitly intended.
- Verify every destructive reset path has confirmation, cloud-sync handling, migration semantics, and honest leaderboard behavior.
- Remove references to retired items/features from shop definitions, runtime state, save migration, tutorials, tasks, analytics, and documentation.

## Startup And Runtime Readiness

- Cold launch should load only what the first screen needs. Lazy-load later music and heavy assets on first use.
- Verify all packaged fonts, images, audio, and case-sensitive paths from a clean build.
- Exercise the real first-run save path and an old migrated save. Test without relying on development defaults.
- Use trusted network time for daily/weekly resets where available and define offline fallback behavior.
- Check runtime logs after cold launch, page navigation, gameplay, settlement, ad return, and background/resume.

## Phone QA

Use at least one notched iPhone-class device and one materially different Android device when available. Cover:

- safe area and touch coordinates;
- font visibility and baseline clipping;
- longest translated/Chinese copy;
- long lists, scrolling, and repeated page entry;
- peak gameplay load and reduced-effects mode;
- audio switching and first-use load;
- all rewarded-ad outcomes;
- cloud timeout/offline/relogin;
- old-save migration, reset, reinstall, and public-account behavior.

A preview build is useful for iteration but can share cloud/leaderboard state with production depending on Maker configuration. Use versioned test keys or isolated test accounts/boards before generating scores that could pollute public rankings.

## Store Assets

Inspect the current TapTap/Maker publishing UI or official requirements before generating assets; dimensions and file constraints can change.

Common deliverables include:

- game icon;
- landscape cover;
- portrait cover;
- square promotional image;
- at least three real gameplay screenshots;
- short title, one-sentence hook, description, and review notes;
- optional short promotional video.

The first image should communicate the core action and emotional payoff in about one second. Marketing art may establish mood, but do not present non-gameplay concept footage as actual gameplay. Use the exact game name and visually check Chinese title glyphs after generation/editing.

## Final Build

1. Freeze the intended runtime file list.
2. Run syntax and focused behavior tests.
3. Inspect Git diff/status and preserve unrelated assets/workbooks.
4. Check Maker binding and current ad configuration.
5. Run Maker build with the exact project root and selective files.
6. Verify remote completion, preview refresh, commit, and runtime logs.
7. Generate/upload a mobile test build only when requested.
8. Complete real-device acceptance before claiming release readiness.

## Post-Launch Funnel

Read metrics in causal order:

```text
exposure -> store click -> store conversion -> first launch -> tutorial steps
-> early levels -> main-mode unlock -> later progression -> ads/retention
```

Interpretation examples:

- high exposure, low click: cover/title/topic mismatch;
- good click, low conversion: screenshots, description, trust, or value proposition;
- conversion but low launch: access, release state, loading, crash, or black screen;
- launch but tutorial drop: unclear target, weak guidance, or early interaction failure;
- early levels but no trial/main-mode completion: difficulty gate, session length, reward, or unlock clarity;
- low ad completion: placement, reward value, fill/return reliability, or pacing disruption.

For tiny samples, prioritize hard failures and qualitative reports. Do not infer precise retention percentages from a handful of players. Once volume grows, compare versioned cohorts rather than mixing users from materially different balance or onboarding versions.

## Live-Ops Safety

- Version leaderboard keys after exploit/test contamination when deletion is unavailable.
- Version analytics events when trigger semantics change.
- Keep economy changes reversible and compare no-ad/medium-ad/all-ad cohorts.
- Migrate old saves for changed caps, removed items, renamed IDs, and unlock rules.
- Avoid forcing existing players to repeat onboarding or lose configured equipment after schema changes.
- When a player reports a freeze on multiple pages, investigate shared render/update/cloud/UI infrastructure before patching only the named page.
