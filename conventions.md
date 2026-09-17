# Conventions — how this codebase actually works

Illustrative placeholder. In a real repo, every rule here should carry a `file:line`
citation to the code that demonstrates it — conventions asserted from memory are exactly
what this file exists to prevent.

## Example entries (replace with your own)

- **Routes are registered in one place, in one style.** e.g. `server/routes/index.js`
  mounts every module with the same `app.use('/api/<module>', ...)` shape. A new module
  that registers itself differently (a stray `app.get` in its own file) is a convention
  break, not a style choice.
- **Errors have one shape across the whole API.** e.g. every failing route responds
  `{ error: { code, message } }` — never a bare string, never an HTTP body with no `error`
  key.
- **One logging call signature, everywhere.** e.g. `log.info(event, { ...context })`, not
  `console.log`, not a one-off format string.
- **Config is never read directly from the environment inside business logic.** It goes
  through one loader (e.g. `config.get('featureX.timeout')`), so there's one place that
  knows every setting that exists.

## How to keep this file honest

Every time an agent (or a human) discovers a convention the hard way — by having a diff
sent back in review, say — add it here with the citation, so the next pass through this
codebase doesn't repeat the same rejected diff.
