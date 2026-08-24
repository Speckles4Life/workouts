# CLAUDE.md — Workout Tracker

Context for Claude Code working in this repo. Read before making changes.

## What this is
A single-file offline workout tracker. One `index.html`, no build step, no dependencies. Hosted on GitHub Pages at `https://speckles4life.github.io/workouts/` (public repo). Used on a phone, added to the home screen. Currently themed as "BJJ Prep Log" — training toward starting Brazilian jiu-jitsu.

## Hard constraints — do not violate
- **One file only.** Everything (HTML, CSS, JS) lives in `index.html`. No separate files, no bundler, no framework, no npm.
- **Fully offline.** No external scripts, CDNs, web fonts, analytics, or network calls of any kind. System font stack only. If a change needs the network, it's the wrong change.
- **No login, no PII, no cloud.** Data lives in the browser via `localStorage` (key `ppla_sessions_v1`). Never rename that key — it breaks every existing user backup.
- **35-minute session cap** is a real training constraint. Adding exercises that push a day past ~35 min means replacing, not appending.
- **Preserve accessibility:** 48px min tap targets, ARIA labels, visible focus outlines, `prefers-reduced-motion` support, safe-area insets. Don't regress these.
- **Validate before delivering:** the JS must parse (`new Function(scriptBody)`), braces must balance.

## How the owner works
Senior Product Owner, ex-.NET, directs and reviews rather than hand-codes. Wants: simplicity over features, weak points flagged, no over-engineering, and contradictions surfaced *before* you edit. Keep diffs small and explain trade-offs.

## Program structure
Two phases, switched manually via a toggle (Phase 2 triggers on the first BJJ class, not a date).

**Phase 1 · PPL** (strength days): Push, Pull, Legs
- Push: Barbell Bench Press 3×8 · Dumbbell Shoulder Press 3×8 · Dumbbell Incline Press 3×10 · Dumbbell Lateral Raise 3×12 (60s rest)
- Pull: Barbell Bent-Over Row 3×8 · Pull-Up Progression/Lat Pulldown 3×8 · Dumbbell Single-Arm Row 3×10 · Dead Hangs 3× (60s rest)
- Legs: Barbell Back Squat 3×8 · Barbell Romanian Deadlift 3×8 · Dumbbell Walking Lunge 3×10/leg · Dumbbell Calf Raise 3×15 (60s rest)

**Phase 2 · Full Body** (strength days): Full Body A (lower bias), Full Body B (upper bias)
- A: McGill Curl-Up (warm-up) · Back Squat 3×6 · Bench Press 3×8 · Pull-Ups 3×8 · Suitcase Carry
- B: Bird Dog (warm-up) · RDL 3×6 · Shoulder Press 3×8 · Bent-Over Row 3×8 · Dead Hangs

**Always available (both phases):**
- Zone 2 — conditioning (30–40 min conversational cardio)
- Stretches — physio checklist (Cat & Camel, Child's Pose, Piriformis, Lower Trunk Rotation, Bridge on the Floor)

Default rest is 90s (60s where marked). Default reps carried per exercise.

## Three log types
1. **strength** — weight/sets/reps. Pre-fills the last logged values for that exercise; +/- steppers (weight 2.5kg, sets/reps 1). Per-exercise rest label shown; floating timer has 60s/90s presets.
2. **activity** (Zone 2) — duration in minutes + optional note.
3. **checklist** (Stretches) — tick which items were done.

## Data model (localStorage array, newest-first)
- strength: `{id,date,phase,day,dayLabel,kind:'strength',ex:[{n,w,s,r}]}`
- activity: `{id,date,phase,day,dayLabel,kind:'activity',duration,note}`
- checklist: `{id,date,phase,day,dayLabel,kind:'checklist',done:[names],total}`

## Dates
Stored as ISO `YYYY-MM-DD` (required for correct sorting and the progress chart). **Displayed** as DD/MM/YY via `fmtDate()`. Never change the stored format — DD/MM/YY sorts wrong.

## Progress tab
Line chart of weight over time per exercise, for any lift logged with weight > 0, across both phases. **Known gap:** pull-ups, dead hangs, and stretches are bodyweight/timed and don't appear here. A rep/time-based progress view is a possible future feature, not yet built.

## Backup
JSON export/import in the History tab. This is the only safety net — `localStorage` is wiped by clearing browsing data, switching browsers, or reinstalling.

## Deploy
Commit and push to `main`. GitHub Pages redeploys automatically within ~1 min. No build.

## Version tag
Header shows a small `v1.N` tag top-right of the title (`.titlerow` in `index.html`). `N` is the total git commit count — **before committing, run `git rev-list --count HEAD` and set the tag to that number + 1** (the commit you're about to make). Hardcoded string, not computed at runtime (no build step, no network). Lets the owner confirm on their phone that a deploy actually landed.

## Open / unresolved
- **Bench contradiction:** owner said the gym has no tilting bench, then chose Dumbbell Incline Press (which needs one). Resolve before trusting the Push day.
- **Legs day** (Squat + RDL + Lunge + Calf) may exceed 35 min — flagged, not yet cut.
- **No periodisation reminders** — deferred; a localStorage app can't push notifications. Owner leaning toward a phone-calendar reminder instead.
