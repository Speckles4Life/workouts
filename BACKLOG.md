# Feature Backlog — Workout Tracker

Prioritised. Every item must respect the app's constraints: one file, offline, no cloud, no login, no frameworks. Reject anything that breaks them.

## Next up

1. **Auto-compare + weight nudge** — already pre-fills last session's weight/sets/reps and shows a "Last:" line; add a ▲/▼ indicator once today's numbers are entered plus a "add weight / hold" suggestion based on rep range (double progression). Uses existing history lookup, no new storage.
2. **PR tracking** — detect all-time best weight per exercise and flag "PR" on save and in the Progress tab. Same history lookup as item 1, so build them together; weight-based PRs first.

## Later

3. **Rep/volume-based progress** — the Progress tab is weight-only, so pull-ups and dead hangs never chart; add a reps/seconds trend for bodyweight and timed lifts.
4. **Bodyweight tracking** — log a number + date, show a trend line, same shape as the activity log, no cloud.

## Considered and rejected — do not re-litigate

- **Oura/recovery integration** — breaks offline + no-cloud + no-login.
- **Auto-build PPL from most-used exercises** — fights the hand-designed BJJ split.
- **Daily recovery+bodyweight+training summary** — depends on Oura data we're not pulling.
