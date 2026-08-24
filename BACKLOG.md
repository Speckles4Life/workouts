# Feature Backlog — Workout Tracker

Prioritised. Every item must respect the app's constraints: one file, offline, no cloud, no login, no frameworks. Reject anything that breaks them.

## Next up

1. **PR tracking** — detect all-time best weight per exercise and flag "PR" on save and in the Progress tab. Reuses the same history lookup as the (now shipped) weight nudge; weight-based PRs first.

## Later

2. **Rep/volume-based progress** — the Progress tab is weight-only, so pull-ups and dead hangs never chart; add a reps/seconds trend for bodyweight and timed lifts.
3. **Bodyweight tracking** — log a number + date, show a trend line, same shape as the activity log, no cloud.

## Done

- **Auto-compare + weight nudge** (2026-08-25) — ▲/▼ indicator comparing today's entered weight to last session's, plus an "Add weight" / "Hold · +reps" suggestion based on whether last session hit the target rep count (simplified double progression — no rep-range field in the data model, so this uses last-reps-vs-target as a proxy). Uses the existing history lookup, no new storage. Confirmed working on-device.
- **Version tag** (2026-08-25) — `v1.N` marker top-right of the title, `N` = total git commit count, hardcoded per commit (no build step). Lets the owner confirm a deploy landed at a glance. Confirmed working on-device.
- **Corruption guard** (2026-08-25) — found while investigating a data-loss incident (History showed only one stale entry from 11 Aug, everything since gone; code audit ruled out the app as the cause, but surfaced this). `load()` no longer silently treats unreadable localStorage as empty; normal saves are now blocked when storage is flagged corrupted, and a raw-bytes export lets the owner recover whatever's left manually. Confirmed working on-device.
- **Import confirmation** (2026-08-25) — Import used to replace all sessions with a file's contents with zero warning. Now confirms session counts (current vs incoming) before replacing anything. Confirmed working on-device.

## Considered and rejected — do not re-litigate

- **Oura/recovery integration** — breaks offline + no-cloud + no-login.
- **Auto-build PPL from most-used exercises** — fights the hand-designed BJJ split.
- **Daily recovery+bodyweight+training summary** — depends on Oura data we're not pulling.
