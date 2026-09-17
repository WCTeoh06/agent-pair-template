# Debug loop — the triage analyst's procedure

1. **Restate the symptom precisely** — what was expected, what actually happened, and
   under what conditions. Vague symptoms produce vague diagnoses.
2. **Check `known-traps.md` first.** A fast negative match is as useful as a fast
   positive one — it tells you to look elsewhere with confidence.
3. **Check the tracker for prior reports on this feature**, including closed ones. A
   returning symptom is a regression, and the old report is a head start.
4. **Form two or three candidate causes**, not one. A single hypothesis pursued to the
   end is how confirmation bias produces a fabricated root cause.
5. **Find the cheapest experiment that discriminates between the candidates** — a log
   line, a targeted read, a command whose output rules candidates in or out — and run it.
6. **Cite the file:line for the confirmed cause.** If nothing confirms cleanly, report the
   candidates and what would be needed to discriminate further; do not pick one anyway.
7. **Propose the fix. Do not apply it.**

## Report template

```
## Symptom
<what was expected vs what happened, and the reproduction steps>

## Root cause
<file:line, with the evidence that confirms it>
(or: candidates + the experiment that would discriminate between them, if unconfirmed)

## Proposed fix
<the change, not applied>

## How to confirm a fix worked
<one command or click>
```
