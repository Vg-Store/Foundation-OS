# Ascent v3 QA

Run in headless Chromium (Playwright, over local HTTP) plus a scripted jsdom suite.

Verified
- JS syntax (`node --check`), manifest JSON, service worker syntax, icon dimensions.
- No console or page errors on load, setup, session flow, timers, tabs.
- Readiness dropdown click no longer starts a timer.
- Effort/pain selects survive re-render.
- Near-miss (10/10 on a 12 target) = no penalty; per-stage best saved.
- Hold flow: ready countdown, tick beeps, hold, target-reached state, STOP & SAVE writes seconds, rest dock opens.
- Rest finish: ring plays, GO shown, dismiss works. Get-ready "None" skips the countdown.
- Undo restores counters, stages and the entered numbers.
- v2 to v3 migration keeps the same exercise (Pike stays Pike) and remaps undo snapshots.
- Assessment: 9 push-ups → Low Incline, 16 → Pike. Planche gate works from stage names.
- Layout checked at 390px and 1200px.

v3.1 engine checks (scripted)
- Recovery volume 3 -> 2 sets. Reduced sessions keep the clean streak and record no miss.
- Skill N-of-M rule (3 of 4 = clean, 2 of 4 = miss, no near-miss for skills).
- Form gate: reps without the form check = no credit and no miss; with the check = credit.
- Placement check: clear miss steps back at once; on-target keeps stage; normal single miss does not regress.
- Visual check of the new card lines and roadmap line at 390px.

- Readiness classification for known input combinations (found and fixed an inverted soreness term).

Not testable here
- Real speaker output, vibration, notifications and wake lock on a physical phone.
