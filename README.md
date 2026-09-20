# Ascent v3 — Adaptive Calisthenics PWA

Local-first calisthenics coach for handstand, L-sit, pull-up, legs and planche, now with hold timers, rest timers and alerts.

## Files
index.html, manifest.json, sw.js, icon-192.png, icon-512.png, icon-maskable-512.png

## Run
Serve this folder over HTTP(S) (`python3 -m http.server 8000`, then open http://localhost:8000). Installing as a PWA and offline mode need HTTP(S), not file://.
Updating from v2: replace all files and reopen the app twice (the service worker cache is now `ascent-v12`). Your v2 progress migrates automatically; back up first via Log > Export.

## What's new in v3
Fixes
- Tapping a Readiness dropdown started a "NaN:NaN" rest timer (both used `data-r`). Fixed.
- Effort / Pain level / Pain area dropdowns reset visually when the page re-rendered, while the old values were still being saved. They now restore from your draft.
- Desktop: the nav bar rendered at the bottom of the page. It is now fixed and centred on every screen size.
- Rest bar sat under the header on notched phones and lost its state on re-render. Timers now live outside the page views.
- iPhone zoomed the page when focusing dropdowns (font under 16px). Fixed.
- "Best" mixed seconds and reps across stages; it is now stored per stage.
- Progress chart mixed seconds and reps; it now shows target completion %.
- Validation error showed on later exercises even when they were fine.
- Date maths used UTC (off by one near midnight); now local.
- Streak ignored how long ago your last session was.
- Baseline test could not re-run and did not check the planche gate. Both fixed.
- Timezone-safe dates, storage error toast, service-worker offline fallback.

Progression
- New push stages: High Incline Push-up and Low Incline Push-up (3x12) before Push-up (3x15). A 3x9 push-up person now starts on a stage they can actually progress from.
- Very close (>=80% of target on every set) counts as "no penalty, repeat" instead of a miss.
- Each exercise shows your last numbers and best set.
- Undo last session (Log) brings your numbers back so you can fix a typo.

Timers & alerts
- ▶ Timer on every time-based set: get-ready countdown (default 0/3/5/10 s, plus a per-exercise override 0–30 s on each timed exercise card, e.g. 6 s for handstand), count-up hold, chime when you reach the target, keep going if you can, STOP & SAVE writes your real seconds into the set.
- Rest timer with -15/+15/Skip, ends with a ringtone + vibration. Auto-starts after each set you enter or save (optional).
- Countdown beeps for the last 3 seconds. Four ringtones (Chime, Bell, Beep, Alarm), three volumes.
- Sounds are generated in the app (no audio files) and use audio elements, so they still play with the iPhone silent switch on.
- Optional notifications when the app is in the background; screen wake lock while a timer runs.
- All settings in Log > Timer & alerts.

## v3.1 progression-engine fixes
- Recovery / long-break volume now really drops: 3 sets -> 2, 4 -> 3, 5 -> 4, 6 -> 4. Before, ceil(0.7 x 3) left every 3-set exercise unchanged.
- Reduced-volume sessions (recovery, low readiness, 14+ days away) are logged but earn no progression credit and never count as misses. They also no longer wipe your clean-session streak.
- Skill holds (handstand, planche) count as clean when most sets hit the target (about 60%, e.g. 3 of 4, 5 of 8), not every set. No near-miss shortcut for skills.
- Planche and Tuck L-sit and beyond need a self-checked form pass to earn credit. Target reached without the check = repeat, no penalty.
- Placement check: after a baseline, your first two full-volume sessions on each exercise can move you back one stage immediately on a clear miss. Each step-back re-arms a fresh two-session check on the new stage. The card shows "Placement check 1 of 2".
- Imported/stored history is now validated entry by entry, so a corrupted backup can no longer stop the app from rendering.
- Form self-check wording is specific to L-sit and planche.
- Return-from-break message now describes what actually happens. Roadmap shows "Advance when" for your current stage.

## v3.2 polish
- Exercise cards are slimmer: Pain, pain area and get-ready live under "Pain & timer settings", which opens by itself once pain is logged. Effort and the form self-check stay visible because they change progression credit.
- Guide wording now matches the engine: any effort short of Near max counts; three clean full-volume sessions advance you; reduced days are skipped.
- Progress chart fades reduced-volume sessions and marks them with *.
- Undo is disabled for old records with no undo snapshot instead of half-undoing. Import shows how many sessions were recovered and how many damaged records were dropped.
- Once, after your first completed session, the app asks the browser to protect its stored data (navigator.storage.persist). The browser decides. Keep exporting backups.

## v3.3 readiness fix
- Soreness was scored backwards in the readiness check (Low soreness added 0, High soreness added 2), so an average-sleep, average-energy, not-sore day was classed "low" and cut your volume with no progression credit. All three inputs now add the same way (1 = worst, 3 = best). One poor input alone stays normal volume; two or more poor inputs reduce volume. Regression tests added.

## Limits (web platform, not bugs)
- Nothing can ring if the phone is locked or the app is fully closed. Keep the screen on; the timer holds it awake.
- Vibration is unsupported on iPhone Safari.
- Notifications on iPhone need the app installed to the Home Screen (iOS 16.4+).
