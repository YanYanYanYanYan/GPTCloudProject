# Game Design, Economy, And Retention

Read this reference for MVP design, difficulty, progression, rewards, ads, automation, retention, balance sheets, or live player feedback.

## Core Loop Before Feature Count

A technically complete shell can still feel dull. Validate:

- a frequent player decision;
- readable pressure or opportunity;
- immediate animation/sound/value feedback;
- a visible short-term goal;
- a reason to try another run.

Prototype the smallest version that proves the distinctive action. Menus, modes, shops, achievements, and social systems amplify a good loop; they do not create one.

## Onboarding And Guidance

- Split the first tutorial into contextual actions: point at the exact object, require the action, celebrate success, then reveal the next action.
- Use a dimmed mask, spotlight, pointer, anchored instruction card, and blocked unrelated taps when the action would otherwise be missed. A weak yellow tint and center-screen tip are not enough.
- Teach an unusual gesture when the first relevant character/object appears, not before and not only in a help page.
- After returning home, show the next unlock in a prominent compact card. Keep copy short enough to fit without ellipsis.
- New-content red dots should appear at the entry that leads to the feature and clear on the meaningful action, such as purchase or inspection, not merely on page open when the player has not understood it.

## Progression And Automation

A durable management-game arc is:

```text
manual execution -> targeted assistance -> automation -> strategy optimization
```

Automation should solve repetitive motor work while preserving decisions about priority, routing, thresholds, capacity, risk, and resource allocation. Emergency tools should rescue mistakes rather than become the default optimal loop.

Unlock rules must be internally consistent:

- tasks reference only unlocked content;
- a feature begins dropping rewards only after its announced unlock condition;
- delayed upgrades clearly state when they take effect;
- the home goal continues to point to the next actionable unlock;
- old saves are normalized into the new unlock state.

## Difficulty Curves

Model difficulty as more than total entity count:

- arrival cadence and burst size;
- simultaneous exception types;
- patience/deadline pressure;
- route/capacity conflicts;
- equipment failures and recovery;
- automation blind spots;
- number of meaningful decisions per second.

Avoid dumping all remaining entities into the final seconds. Use a target curve plus bounded catch-up that depends on progression and automation strength; do not rely on a rigid universal rule such as “at most three every five seconds.”

Compare each floor/chapter's first week with the preceding stage. A newly unlocked stage may briefly ease complexity, but an unexplained drop in all pressure metrics feels inconsistent. Test C through the highest difficulty with the upgrades a normal player can actually afford at that point.

For endless modes, preserve the late-game power fantasy before countering infinite automation. Escalate understandable pressure sources gradually: throughput, conflicting VIP/special rules, unidentified exceptions, failures, and shrinking patience. Avoid arbitrary per-wave damage or silently disabling owned upgrades. Cap runaway currency separately if needed while preserving score/reward meaning.

## Economy Method

Build a table by day/floor/chapter and player cohort:

- guaranteed base income;
- skill/performance income;
- ad income for 0%, about 50%, and 100% optional-ad participation;
- expected expenses, unlock prices, and upgrade prices;
- cash remaining and percentage of available progression purchasable.

Choose a target progression pace before changing prices. Do not fix a shortage by increasing every reward if the real issue is front-loaded slot cost or one mandatory bottleneck. Lower early installments while retaining total lifetime cost when early access is the problem.

Reward and penalty values should scale with stage income. A fixed tiny bonus becomes irrelevant late; a fixed complaint penalty also becomes meaningless. Use a transparent function of stage/run reward with a humane cap, then test common complaint counts rather than balancing only one extreme sample.

When a run's rewards increase, re-audit every related sink and feedback number: per-trip bonus, task reward, repair, reroll, slot expansion, skins, penalties, and leaderboard score. Maintain a source-of-truth workbook when the game has many coupled curves.

## Optional Ads

- Ads should provide convenience, acceleration, recovery, or an optional efficiency boost.
- Design the no-ad route to remain playable and the medium-ad route to fund a healthy share of newly unlocked growth.
- Do not make players watch ads during the most time-sensitive interaction if the reward can be granted as pre-run inventory instead.
- Scale repeatable ad rewards by progression when a flat reward becomes trivial, but add limits and recheck inflation.
- Show the exact reward after the successful return and attribute completion counts by source.

## Feedback And Presentation

- Important rewards need hierarchy: amount, source, and consequence. Merge dense tiny floats so they do not hide hazards.
- A purchase description should state what changes in play, not repeat the item name. Upgrade preview should show the concrete before -> after difference.
- Let players inspect complaint/failure reasons at settlement; in wave modes, show the current wave's reasons before the continue/quit decision.
- Celebrate personal records, new ranks, major unlocks, and claimed milestones. Preview the next meaningful reward so the player has a reason to continue.
- Cosmetics should noticeably change the scene and may carry a small, clearly disclosed bonus. Preview them at useful scale with real gameplay entities to prove readability.

## Retention Without Huge New Systems

Low-cost improvements include:

- prominent next objective;
- personal-record celebration;
- next milestone/reward preview;
- clearer item/upgrade comparisons;
- contextual dialogue variety and small conditional easter eggs;
- daily free claim with a red dot;
- unlock calendar markers;
- skins, titles, frames, or lobby themes as currency sinks;
- detailed result breakdown and replay shortcut.

Use live feedback as evidence, not a command to overreact. A sample of 22 players is enough to find a broken funnel or impossible gate, but not enough for precise retention or economy conclusions. Instrument the path, inspect failures, then make the smallest reversible correction.

## Analytics Contract

For each key event define:

- event name and version;
- exact trigger point;
- once-per-account, once-per-run, or repeatable semantics;
- dimensions such as mode, stage, difficulty, ad source, exit point, complaints, and result;
- idempotency key when callbacks/retries can duplicate it;
- privacy-safe identifiers and bounded value ranges.

Minimum early funnel:

- first entry;
- each tutorial step completion;
- early level starts/completions/failures;
- main mode unlock/start;
- major chapter/floor arrival;
- run exit position and complaints;
- ad request/show/success/failure/timeout by source.

Make analytics failure nonblocking. A leaderboard-derived aggregate is not automatically an analytics backend, and top-N samples must be labeled as samples.
