# AGENTS.md

## What this is
`index.html` is the entire app: inline CSS + HTML + JS, zero dependencies, no build system, no package.json, no tests, no lint/typecheck. Mobile-first (max-width 520px, safe-area aware), works from `file://`. Verify any change by opening `index.html` in a browser — there is no automated way to check.

## Where the plan lives
The workout data is JS constants in `index.html`, and everything (tabs, headers, exercise rows, wiki, muscle grid) renders from them:
- `W` — the 4 workout days (wid 1–4; header tabs come from `SCHEDULE`). Each day: `sections[]`, each section: `ex[]` with `{name, sets, reps, note, ...}`.
- `MUSCLES` — the "Muscle hit map" heatmap. `days[]` indexes the 8-day rolling cycle (D1–D8, mirror of `DAY_TYPES`), values 0 = untrained, 1 = secondary/light, 2 = heavy/direct.
- To change the plan, edit the data — not the render markup.

## Coaching context — the workout plan
This is the AI-level context for the user's plan. The `W`/`MUSCLES` data in `index.html` are the source of truth; **the prose below follows the data**. If anything here conflicts with the data, the data wins — fix the prose, not the data. When you change `W` or `MUSCLES`, update this section to match and add a dated bullet under **Key exercise decisions** so the reasoning survives.

### Coaching role
If the user asks for feedback on the plan ("rate my split", "should I change X", …), respond as an evidence-based personal trainer: think critically about muscle balance, recovery gaps, exercise redundancy and progressive overload; be direct and specific; engage seriously with pushback and concede when the user has a valid point; ground recommendations in the user's stated goals, constraints and confirmed equipment.

### User profile & hard constraints
- Focus: Biceps, Triceps and Abs are the primary goal; everything else is maintained as secondary. Legs are secondary.
- Constraints: max 4 workout days per cycle. No Romanian deadlift (user finds it boring).
- Equipment: hand gripper (adjustable GD Iron Grip, 25–80kg). Protocol: rest days D2/D4/D6 only — never on gym days; 3 sets per hand, stop 2–3 reps short of failure; 60–90s rest; full ROM; never on consecutive days; treat finger-joint pain or morning stiffness as overload warning signs.
- Grip weakness on record: during hammer curls the wrist joints gave out before the biceps. Dead hangs and gripper work are addressing it; lifting straps are an acceptable short-term workaround so grip never limits bicep stimulus.

### Structure
Rolling 8-day cycle (not anchored to calendar days): `D1 Push · D2 Rest · D3 Pull · D4 Rest · D5 Legs · D6 Rest · D7 Arms · D8 Rest`. Chosen over a fixed weekly schedule for even recovery distribution. Session order Push → Pull → Legs → Arms gives even 4+4 day gaps for the priority muscles (Chest, Shoulders, Biceps, Forearms); Triceps runs a 6+2 gap (Arms heavy → Push secondary) and Back a 2+6 gap (Pull heavy → Arms light) — both accepted tradeoffs. D5 Legs also carries the day's light upper-body maintenance (shoulder, chest, biceps, triceps).

### Frequency per 8-day cycle (mirrors `MUSCLES`)
- Chest 2× (D1 heavy, D5 light)
- Shoulders 2× (D1 heavy, D5 light)
- Triceps 4× (D1 secondary, D3 light pump, D5 light pump, D7 heavy)
- Biceps 4× (D1 light pump, D3 secondary, D5 light pump, D7 heavy)
- Back 2× (D3 heavy, D7 light)
- Abs 4× (D1, D3, D5, D7)
- Forearms 1× (D7 only)

**HIGH-FREQUENCY ARM RULE:** D1/D3/D5 arm work is pump/maintenance only — limited sets at 30–40% less weight than D7, never near failure. **D7 Arms is the only true growth session — protect it.**

### Key exercise decisions
- **Triceps (Jul 2026):** Push day keeps one pushdown (2×10–12) as maintenance — triceps are pre-fatigued by pressing. All triceps growth happens on Arms day: close-grip machine chest press 4×8–10 (heavy compound) → overhead cable rope extension 3×10–12 → rope pushdown 2×12–15. Cable overhead extension removed from both days.
- **Shoulder press (Jul 2026):** Push day uses the Standing dumbbell Arnold press 4×8–10 (heavy rotational compound, full stabilizer demand); Legs day the Seated Arnold press 2×12–15 — two genuinely different shoulder-press stimuli per cycle.
- **Back:** Pull day heavy block is Gravitron assisted wide-grip pull-ups 4×8–10, narrow-grip seated row, dumbbell shrugs and seated machine back extensions. Arms day has a light back block including the Standing cable lat pullover 2×12–15 — the program's only straight-arm lat isolation (no bicep involvement). An early close-grip underhand lat pulldown trial on Pull day is no longer on the program.
- **Chest (Aug 2026):** Chest dips were removed from Push day after recurring right-shoulder pain (deep shoulder extension was the trigger) and replaced with Cable Fly Crossovers 3×10–12 — lower-chest emphasis, constant tension and peak contraction without the shoulder strain. Heavy chest: horizontal barbell bench 4×6–8 + 30° incline DB press 3×10–12. Light chest on Legs day: bench only, 2×12–15 technique practice, no fly.
- **Biceps:** Pull day runs a single Zottman curl 3×10–12 (biceps supinated on the way up, brachioradialis pronated on the way down) — distinct from Arms day. Arms day order: Seated dumbbell curl 3×6–8 (heavy bilateral, done first while freshest) → Dumbbell hammer curl 3×10–12 (brachialis) → One-arm dumbbell preacher curl 3×10–12 (unilateral). Hammer curls are intentionally 2×/cycle (Legs light + Arms heavy), not triple-covered.
- **Rotator cuff / rear delts:** Standing shoulder external rotation is Activation on Push day (2×12–15) and strength work on Pull day (4×10–12). Internal rotation is deliberately omitted — compound pressing already over-trains it. Rear delts: Machine reverse fly on Pull (3×12–15) and Legs (2×12–15).
- **Side delts:** Machine lateral raise on Push day (3×12–15) + one-arm cable lateral raise on Legs day (2×12–15) for a second weekly hit — side delts get far less indirect volume from pressing than front delts.
- **Abs:** every abs day = flexion (machine crunch) + anti-rotation (Pallof press outward rotations) + a third movement for the angle the other two miss: lateral-flexion side bend on Push/Legs, dynamic rotation (lever kneeling twist) on Pull/Arms. All four training days include abs with the same set structure.
- **Legs (Sep 2026 — trimmed):** Legs are a secondary goal, so the day was cut from 53 to 46 work sets (~2h30 → ~2h). Quad block: seated leg press 4×8–10 (only heavy leg compound, 2 min rest) → Bulgarian split squat **4×10–12** (2 sets per leg, down from 6; Romanian deadlift replacement — user finds RDL boring; explicit `rest:"60s"` override because it is a heavy compound despite `altSides`) → seated leg extensions **2×10–12** (loads terminal knee extension that pressing never tensions — complements rather than duplicates the press). Prone leg curl **4×10–12** (the program's only direct hamstring work — do not cut further). Calf raises 3×15–20; inner adductor + leg abduction **3+3 straight sets**, each with `rest:"45s"` — the two are separate machines, so they were un-supersetted (two machines cannot be held at once at peak hour; straight sets cost ~1.5 min back, offset by the 45s rest). With only 2 sets per leg on split squats, those sets must be genuinely hard (2–3 RIR) — volume was traded for intensity. Light upper-body sections ride on the same day.
- **Legs light blocks (Sep 2026):** Shoulders (light) keeps all three exercises at 2 sets — the Seated Arnold press and one-arm cable lateral raise each add a distinct second-weekly stimulus, and Machine reverse fly 2×12–15 is kept as the 2nd rear-delt hit per cycle against ~15 pressing sets (it is the designated first cut if the day runs long again). `Biceps (light)` and `Triceps (light)` were merged into a single `Arms (light)` superset section (seated hammer curl + standing dumbbell triceps extension) — a station change and ~3 min saved; it survives the share-a-station superset rule because both movements run off the same pair of dumbbells. The merge shifted `done[4]` section keys; expect stale Legs-day ticks.
- **Glutes (Sep 2026):** Hip thrust (machine) 3×10–12 added to Legs day, appended last in the section renamed `Calves, adductors & glutes` (append-only, so existing `done[4]` ticks are unaffected). The day's only prior hip-extension work was the Bulgarian split squat, which loads glutes at long/stretched lengths; the leg press barely extends the hip and prone curls are pure knee flexion — the thrust is the sole short-length/peak-contraction glute stimulus and lets glutes progress without adding split-squat sets (deliberately capped at 2/leg for time). Placed last in the leg blocks so it can't compromise the squat or the light upper-body sections; rest auto-derives to 75s from the 10–12 rep bucket; single machine, no superset. Takes the day back to 49 work sets (~2h → ~2h15min) — a deliberate exception to the trim above, accepted because this was the program's one genuine coverage gap.
- **Forearms:** Arms day only — single-arm dumbbell wrist curl superset (reverse/extension + underhand/flexion, 2×12–15 each). No pronation/supination work (not available in the user's app historically; forearms already covered on Arms day).
- **Obliques / rotational core:** Pallof press (anti-rotation), lever kneeling twist (produces rotation), side bend (lateral flexion) — no additions needed.

### Warm-up, cardio, stretching
- Warm-up: Stepper 5 min every day. Cardio: treadmill 20 min — 125–130bpm on Push/Pull/Arms; 110–120bpm on Legs day (walking pace only, legs are fatigued).
- Stretching: rendered from each day's `Stretching` section — 45s holds, done after cardio, all from exercises available in the user's workout app.

## Progress tracking (localStorage)
- Keys: `curWid` (1–4) and `done` — JSON `done[wid][sectionIndex][exerciseIndex]` booleans.
- Ticks are position-keyed: inserting/removing/reordering exercises or sections silently reassigns saved ticks to different exercises. Prefer appending to the end of a section; if you must reorder, expect saved state to be wrong.

## Derived UI — don't hardcode
- Rest time and load label ("Heavy", "Moderate", …) are auto-derived from the rep range by `getRestTime`/`getLoadLabel` (~line 383). Change `reps`, not those functions.
- Rest precedence in `getRestTime`: `Stretching` none → **explicit `rest:"60s"` field** (opt-in per-exercise override, wins over everything below) → `Activation` 30s → `altSides` 45s → `Rotator cuff` 45s → note containing "main compound" 2 min → **any section named `…(light)` 45s** → rep buckets (>12→60s, 11-12→75s, 9-10→100s, ≤8→2 min). Use `rest:` only when the derivation is wrong for the exercise (e.g. Bulgarian split squat needs more than `altSides`' 45s); never add it where a `reps` or section-name change would do.
- Sections named `Stretching` are excluded from exercise/set counts and always rendered last. `Activation` gets fixed 30s rest.
- Supersets: two adjacent exercises joined by `supersetWith` (partner's exact `name`) + `supersetRole: "first"`/`"second"`. The renderer uses `ex[i+1]`, so the pair must be contiguous and in order, and both must live in the same section — joining exercises from two sections requires merging those sections (which shifts `done` keys). **Gym constraint: only pair movements that share a station or use the same free weights.** Never supersets two machines — the user cannot hold two machines at peak hour (this is why adductor + abduction run as straight sets).
- `warmup: true` adds a "Warm-Up" badge (mark main compounds you ramp up before working sets); `altSides: true` forces 45s rest unless a `rest:` field overrides it; `dayNote` renders a warning box (currently used by no day).