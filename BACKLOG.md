# Feature Backlog — Workout Tracker

Prioritised. Every item must respect the app's constraints: one file, offline, no cloud, no login, no frameworks. Reject anything that breaks them.

## Data safety (critical)

Found 2026-08-25 while investigating a user-reported data-loss incident (History showed only one stale entry from 11 Aug, everything since gone). Code audit ruled out the app itself as the cause — the storage key never changed and nothing auto-clears — but it surfaced two real weaknesses that could make a *future* storage problem worse or unrecoverable.

A. **Corruption guard** — `load()` used to silently swallow unreadable/corrupted localStorage and return an empty array; the very next save would then permanently overwrite the corrupted (but possibly still partially recoverable) raw value with just the new entry, with zero warning. Now: corrupted storage is detected and flagged, normal saves are blocked with a toast, and a "raw storage" export lets you pull out whatever bytes are still there for manual recovery. *Status: implemented and pushed to prod.*
B. **Import confirmation** — Import silently replaced ALL current sessions with the file's contents, no warning, no undo. Now shows a confirm dialog stating how many sessions on-device will be replaced by how many in the file, before proceeding. *Status: implemented and pushed to prod.*

## Next up

1. **Auto-compare + weight nudge** — already pre-fills last session's weight/sets/reps and shows a "Last:" line; add a ▲/▼ indicator once today's numbers are entered plus a "add weight / hold" suggestion based on rep range (double progression). Uses existing history lookup, no new storage. *Status: implemented and pushed to prod, awaiting owner confirmation on-device.*
2. **PR tracking** — detect all-time best weight per exercise and flag "PR" on save and in the Progress tab. Same history lookup as item 1, so build them together; weight-based PRs first.
3. **Version tag** — small version marker (e.g. `v1.0`) top-right of the title, so the owner can tell at a glance whether the phone is showing the latest deploy. Hardcoded string, bumped manually on every shipped change — no build step to derive it from. *Status: implemented and pushed to prod, awaiting owner confirmation on-device.*

## Later

4. **Rep/volume-based progress** — the Progress tab is weight-only, so pull-ups and dead hangs never chart; add a reps/seconds trend for bodyweight and timed lifts.
5. **Bodyweight tracking** — log a number + date, show a trend line, same shape as the activity log, no cloud.

## Considered and rejected — do not re-litigate

- **Oura/recovery integration** — breaks offline + no-cloud + no-login.
- **Auto-build PPL from most-used exercises** — fights the hand-designed BJJ split.
- **Daily recovery+bodyweight+training summary** — depends on Oura data we're not pulling.
