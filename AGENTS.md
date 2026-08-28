# AGENTS.md

## What this is
`index.html` is the entire app: inline CSS + HTML + JS, zero dependencies, no build system, no package.json, no tests, no lint/typecheck. Mobile-first (max-width 520px, safe-area aware), works from `file://`. Verify any change by opening `index.html` in a browser — there is no automated way to check.

## Where the plan lives
The workout data is JS constants in `index.html`, and everything (tabs, headers, exercise rows, wiki, muscle grid) renders from them:
- `W` — the 4 workout days (wid 1–4; header tabs come from `SCHEDULE`). Each day: `sections[]`, each section: `ex[]` with `{name, sets, reps, note, ...}`.
- `MUSCLES` — the "Muscle hit map" heatmap. `days[]` indexes the 8-day rolling cycle (D1–D8, mirror of `DAY_TYPES`), values 0 = untrained, 1 = secondary/light, 2 = heavy/direct.
- To change the plan, edit the data — not the render markup.

## AI_CONTEXT comment — keep it in sync
The huge HTML comment above `<!DOCTYPE html>` is the coaching decision log and is meant to be handed to AI sessions so coaching can resume. When you change the workout, update this comment too. It has drifted from the data before (e.g. it still says "No abs on Legs+Light day" while day 4 actually has an Abs section, and `MUSCLES` counts legs-day abs). **When prose conflicts with the `W`/`MUSCLES` data, the data wins.**

## Progress tracking (localStorage)
- Keys: `curWid` (1–4) and `done` — JSON `done[wid][sectionIndex][exerciseIndex]` booleans.
- Ticks are position-keyed: inserting/removing/reordering exercises or sections silently reassigns saved ticks to different exercises. Prefer appending to the end of a section; if you must reorder, expect saved state to be wrong.

## Derived UI — don't hardcode
- Rest time and load label ("Heavy", "Moderate", …) are auto-derived from the rep range by `getRestTime`/`getLoadLabel` (~line 464). Change `reps`, not those functions.
- Sections named `Stretching` are excluded from exercise/set counts and always rendered last. `Activation` gets fixed 30s rest.
- Supersets: two adjacent exercises joined by `supersetWith` (partner's exact `name`) + `supersetRole: "first"`/`"second"`. The renderer uses `ex[i+1]`, so the pair must be contiguous and in order.
- `warmup: true` adds a "Warm-Up" badge (mark main compounds you ramp up before working sets); `altSides: true` forces 45s rest; `dayNote` renders a warning box (currently used by no day).