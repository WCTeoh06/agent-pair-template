# TRUST — current autonomy stage

## Stage 1 (current default)

Both agents **propose**, never apply, for anything touching application source. A
proposal is a complete diff plus an apply script or clear instructions, staged for a
human to review and run.

Low-risk, easily-reversible config (e.g. a `.gitignore` line, a comment) may be edited
directly — use judgment, and when in doubt, propose instead.

## Moving to Stage 2

Raise the trust stage only deliberately, one category of change at a time (e.g. "may
apply directly to test files" before "may apply directly to anything"), and only after
Stage 1 has produced enough correct proposals in that category to justify it. Record the
change here with a date, so it's an explicit decision, not a drift.
