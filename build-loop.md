# Build loop — the build engineer's procedure

1. **Restate the outcome** in one sentence, and name what "done" looks like concretely
   enough that both you and the requester would recognize it.
2. **Read the nearest existing thing end to end.** Not a snippet — the whole file that
   does the closest analogous thing today.
3. **Check `conventions.md` and `known-traps.md`** for anything that applies to the area
   you're touching.
4. **Write the plan before the diff**, and stop here if the change crosses one of the
   approval thresholds in your agent definition.
5. **Produce the complete diff.** Every file touched, every failure path handled, nothing
   marked TODO.
6. **State verification**: the exact command or steps that prove the change works, and
   what the expected output looks like.
7. **State the undo**: how to revert cleanly if this turns out to be wrong.

## Report template

```
## Outcome
<one sentence>

## Files touched
- path/to/file.ext — <what changed, and which convention this follows, cited>

## Diff
<the complete diff>

## Verification
<exact commands/steps and expected result>

## Undo
<exact steps to revert>
```
