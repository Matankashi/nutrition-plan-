# Nutrition Plan — rules for every session

## Structure
- `index.html` = production, reads/writes `nutrition/` (`DB_ROOT`).
- `test/index.html` = test, reads/writes `nutrition_test/`. Otherwise identical.
- Test first, always. Port to production only after the user verifies `/test/` live.

## Do not change without being asked
- Meal calories 438 / 197 / 549 / 489 / 253 (total 1,926, 167 g protein) were
  verified against nutrition databases.
- `PHASE1_WEEKS`, `PHASE1_RATE`, `PHASE2_RATE` are research-based.

## Body fat
- Navy formula on waist minus neck (`calcBodyFat`, `HEIGHT_CM` = 175).
- Very sensitive to the neck term: 1 cm of neck moves it ~0.8%. Waist is the
  reliable signal. Use averages, not single readings, for anything that
  ratchets or alerts.

## Security
- Email/password sign-in only; database rules locked to the owner UID. Same
  standard as all the user's apps (see `~/app-registry`).

## Process
- Back up `nutrition/matan` to `~/firebase-backups/` before any structural change
  (`firebase database:get /nutrition/matan --project nutrition-plan-5ce1f`).
- Never tag a version before the user has verified it live.
- If something looks wrong or ambiguous, stop and say so before executing.
- Firebase drops empty nodes: every key under `_admin/users` must keep at
  least one child field, or the user entry vanishes.
- Registry: whenever this app's version or status changes, update
  `~/app-registry/index.html` (row, dated changelog line, "last updated") and
  republish it.

## Known limitations (fix as one task after v2 is live)
- Per-user constants are hardcoded to Matan and apply to every profile:
  `GOAL_START` (07.10.2026, 77.8 kg), `START_W` (86), `HEIGHT_CM` (175),
  `BIRTHDAY`. Another profile with dated entries would get Matan's target
  line, body-fat formula height and age.
