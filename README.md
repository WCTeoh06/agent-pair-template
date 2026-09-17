# Build / Triage agent pair

A pattern for splitting AI coding assistance into two disciplined roles instead of one
generalist: a **build engineer** that only implements against a stated outcome, and a
**triage analyst** that only diagnoses. Written as Claude Code subagent definitions, but
the pattern applies to any agent framework that supports role-scoped system prompts.

This is a generalized, sanitized version of a pair I built and used on a real production
codebase at work. The internals below are illustrative placeholders, not that codebase —
this repo exists to show the pattern, not any particular company's system.

## Why split the role at all

A single "do anything" coding agent tends to blur two different jobs that want different
incentives:

- **Diagnosing** a bug rewards being slow, skeptical, and evidence-bound. Guessing a root
  cause and being wrong is worse than admitting you don't know yet.
- **Building** a change rewards momentum once the outcome is clear, but punishes drifting
  from the codebase's existing conventions — a technically-correct diff that doesn't match
  house style is a diff that gets sent back in review.

Keeping them separate means each system prompt can lean all the way into its own
incentive without the other one diluting it. It also creates a natural handoff: the
triage analyst's report *is* the build engineer's brief.

## The pieces

```
agents/
  triage-analyst.md   -> diagnoses, cites evidence, proposes a fix, never applies one
  build-engineer.md   -> implements a stated outcome, matches conventions, proposes a diff
playbooks/
  conventions.md       -> "how this repo actually works", with room for verbatim evidence
  known-traps.md        -> a running list of footguns specific to this codebase
  debug-loop.md          -> the triage analyst's step-by-step procedure
  build-loop.md          -> the build engineer's step-by-step procedure
identity/
  SOUL.md                -> shared behavioral rules both agents read first
  TRUST.md                -> what stage of autonomy the agents currently have
```

The two agent files are short by design — almost everything they need to know about
*this particular codebase* lives in `playbooks/`, which they're instructed to read fresh
every time rather than work from memory. That's deliberate: the agent definition stays
generic and reusable, and the codebase-specific knowledge lives in files a maintainer can
edit without touching the agent's prompt at all.

## Adapting this to your own codebase

1. Replace the illustrative examples in `playbooks/conventions.md` and
   `playbooks/known-traps.md` with real ones from your repo — ideally with a file:line
   citation for each, the same discipline the triage analyst is held to.
2. Adjust `identity/TRUST.md` to match how much you trust an agent to act unsupervised —
   this template defaults to "propose a diff, never apply it" as the safe starting stage.
3. Point the "pull in as needed" references in each agent file at your actual repo-map,
   verification steps, and issue tracker integration.

## What I actually learned building the real version

- The single highest-leverage rule for the triage side turned out to be "evidence or
  silence" — every claim has to cite a file, a line, or command output, and anything else
  gets labeled as inferred or unknown. It's the difference between a diagnosis you can act
  on and a plausible-sounding guess.
- For the build side, "read the nearest existing thing end to end before writing anything"
  did more to keep changes consistent with house style than any amount of style-guide
  prose could.
- Splitting "propose" from "apply" as an explicit trust stage — rather than an implicit
  assumption — made it possible to loosen that constraint later without redesigning
  anything, just by editing one file.
