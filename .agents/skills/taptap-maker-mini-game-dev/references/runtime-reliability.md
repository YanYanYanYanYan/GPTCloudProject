# Runtime Reliability And Phone UX

Read this reference for freezes, stutters, white screens, ads, cloud saves, leaderboards, daily resets, fonts, safe areas, long lists, or other device-only failures.

## Diagnose Before Tuning

1. Establish the affected page/mode, device, OS, reproduction frequency, and whether audio/input/animation all stop.
2. Inspect runtime logs and the actual frame/update/render/data path. A modern phone does not rule out an infinite loop, synchronous cloud call, repeated allocation, runaway UI rebuilding, or callback deadlock.
3. Separate launch black screen, Lua render exception, main-thread stall, async waiting UI, and GPU overdraw. They require different fixes.
4. Add narrow timing/counter logs around suspected transitions; remove noisy per-frame logs after diagnosis.

## Frame-Time Rules

- Never perform cloud/network requests, JSON encode/decode of large state, full-list sorting, full save serialization, asset loading, or UI-tree reconstruction every frame.
- Avoid per-frame table churn, string formatting, text measurement, gradient creation, and repeated traversal of hidden items. Cache stable layout/text/data and invalidate it only when inputs change.
- Long lists need viewport clipping and item culling. Fetch or render pages rather than the whole server dataset. A scrollable list that still draws every off-screen card can freeze on a flagship phone.
- Distinguish tap from scroll after a movement threshold. Clamp scroll offsets; reset or preserve them intentionally when pages/filters change.
- Coalesce dense minor floating rewards and offer a reduced-effects mode, but keep danger cues and dragged objects visually dominant.
- Load only the first required music/large assets at startup; lazy-load later tracks on first use. Preload only the assets needed for an imminent transition.
- Explicitly dispose owned objects/subscriptions/timers where the API requires it. Do not start unmanaged background loops that survive page or game teardown.

## Rewarded Ads

Before editing or diagnosing ads, query the current project's Maker ad configuration with `get_ad_config` when the tool is available. Never reuse another project's app/ad IDs. Treat `sdk:ShowRewardVideoAd(...)` as asynchronous and potentially delayed across an app background/resume cycle.

Use one idempotent request state machine per ad attempt:

- unique request ID;
- `inFlight` guard to prevent double taps;
- one-shot/callback-handled flag;
- lifecycle-safe loading state;
- timeout that restores the UI;
- late-callback filtering by request ID;
- `pcall` around platform callbacks and UI restoration;
- exactly-once reward accounting and save.

Default rule: grant only after the rewarded-video callback reports success. A timeout should recover the page/button, not blindly issue a reward. If the product deliberately guarantees rewards after a confirmed platform launch, persist a pending claim and design explicit anti-duplication/abuse semantics before implementing it.

Do not rebuild the entire active UI immediately when iOS returns from an ad. Restore the minimum page state, defer heavier refresh work, and tolerate the app losing focus/resuming before the ad callback arrives. Analytics failure must never withhold an earned reward. Show the concrete earned reward with a toast after return unless the user requests a modal.

PC preview cannot prove mobile ad behavior. Test success, close/cancel, no fill, failure, timeout, background/resume, delayed callback, and repeated tapping on a real TapTap phone build.

## Cloud Saves And Migration

- Give save data an explicit schema version. Normalize and validate finite/ranged values on load.
- Prefer a few stable partition blobs: critical profile/currency, progression/configuration, inventory/collections, and optional statistics. Lazy-load noncritical partitions and write only dirty partitions.
- Avoid one ever-growing blob and avoid thousands of per-item keys. Both create latency and failure amplification.
- During migration, read new format first, fall back to old data, normalize, and temporarily dual-write when rollback compatibility matters. Do not delete old keys in the first release.
- Use batch/transactional writes for state that must change together. Keep local state recoverable when a noncritical cloud write fails, and expose a retry state instead of freezing interaction.
- Load critical saved configuration before gameplay starts, or keep the UI clearly disabled until synchronization resolves. A player should not have to reconfigure because gameplay began from empty defaults and later overwrote the cloud state.
- Route real currency through a single helper and storage type that supports its range. Keep large cash values separate from bounded leaderboard integers; test 2^31, 2^32, and expected endgame values.
- Resetting local/cloud profile does not necessarily remove a historical rank row. Use versioned leaderboard keys or an active/tombstone key so reset players can be filtered, and verify the current platform's actual delete capability before promising physical deletion.

## Leaderboards And Identity

- Key identity by stable user ID, never nickname.
- Ranking APIs may return `userId` and score only. Batch nickname lookup, normalize number/string forms, deduplicate IDs, cache results, and let failure fall back to a neutral player name without blocking the page.
- Fetch pages with explicit start/limit, provide the player's own rank separately, and discard callbacks for stale page/filter requests.
- State whether aggregates are complete or sampled. If an API returns only the top 100, label it as a top-100 sample rather than a full-server total.
- When an exploit or test build pollutes a board and rows cannot be deleted, migrate to a versioned board and stop reading the old key.
- Cap or encode scores within the storage API's safe integer range and test tie/order behavior.

## Time-Based Features

- Daily/weekly tasks, free claims, ad limits, and rotations should prefer trusted server/network time.
- Cache the last valid server time plus monotonic elapsed time for temporary offline continuity. Do not move time backward when a later response is older.
- Define the reset timezone and boundary explicitly, such as 05:00 local product time, and use one helper for all systems.
- If trusted time is unavailable, decide deliberately whether to disable claiming, use a clearly bounded fallback, or allow offline play without reward reset. Never silently mix device dates across systems.

## Fonts, Rendering, And Safe Areas

- Bundle a Chinese-capable font and load it through a centralized multi-candidate resolver. Check resource existence, create the font, measure a Chinese probe string, log the selected fallback, and keep a safe built-in fallback.
- One missing primary font must not stop all text rendering. Resource paths are project-relative and case-sensitive on some targets; inspect the actual packaged path.
- A helper used by rendering must exist before the render path can call it. Treat repeated render exceptions as a black-screen cause, not a cosmetic issue.
- Safe-area handling should reserve the reported notch/status inset, not add a second blanket top margin. Test the switch both enabled and disabled on notched and non-notched devices.
- Essential text must wrap or scroll; do not hide requirements, prices, or effects behind ellipses. Use font baseline padding so digits and Chinese glyph bottoms are not clipped.

## Phone Regression Matrix

At minimum exercise:

- cold launch and returning launch;
- short/tall and notched screens;
- page opening/closing 20+ times;
- longest market/inventory/leaderboard list and rapid scrolling;
- gameplay during peak entity count with effects on/off;
- pause/background/resume;
- ad success/failure/no-fill/timeout/late return;
- offline, slow network, cloud timeout, and old-save migration;
- large currency/score values and a reset account.

Measure frame time and transition duration where possible. “It is an iPhone 15 Pro” is evidence that raw device power is unlikely to be the sole cause, not evidence that the game loop is healthy.
