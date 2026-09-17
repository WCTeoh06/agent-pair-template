# Known traps

A running list of footguns specific to this codebase — the things that look correct at a
glance and aren't. Illustrative examples below; replace with real ones as you find them.
Each one exists because it actually cost someone real debugging time once.

1. **A cache invalidation that fires before the write it's supposed to follow commits.**
   Looks correct in isolation; only shows up under load, as stale reads.
2. **A retry loop with no backoff and no cap**, added to "make it more reliable," that
   turns one downstream outage into a self-inflicted denial-of-service against your own
   service.
3. **An error handler that swallows the exception with no log line at all** (e.g.
   `promise.catch(() => {})`), so a job can fail on every run and leave a clean log.
   Treat a suspiciously quiet log as "unknown," never as "healthy."
4. **A path built with string concatenation that works in dev and breaks in the packaged
   build**, because the working directory isn't what the developer's machine assumes.
5. **A "temporary" hardcoded value** (a port, a limit, a feature flag) that shipped and is
   now load-bearing, three modules away from where anyone would think to look for it.
6. **Pagination that's off by one at the boundary**, only visible on the last page of a
   result set — which is exactly the page nobody's test data reaches.

## How to add one

State the symptom, the actual cause, and the file:line where it lives. A trap without a
citation is a rumor, not a documented trap.
