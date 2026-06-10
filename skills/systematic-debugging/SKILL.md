---
name: systematic-debugging
description: Root-cause a bug with a hypothesis-driven loop instead of shotgun edits. Use when a bug isn't obvious after the first look, or when asked to "find out why" something fails.
version: 1.0.0
license: MIT
keywords: [debugging, root-cause, methodology, bisect, testing]
---

# systematic-debugging

Never fix what you can't reproduce; never explain what you haven't observed.
The loop is: **reproduce → observe → hypothesize → test the hypothesis →
narrow**. One variable at a time.

## Procedure

1. **Reproduce first.** Find the smallest, fastest command that shows the
   failure deterministically. If it's flaky, make the loop tight
   (`while ./repro; do :; done`) and treat flakiness itself as a clue
   (timing, ordering, shared state).
2. **Read the actual error.** The full message, the *first* stack frame in
   your own code, and the line right before things went wrong. Resist
   pattern-matching to a familiar failure — verify this one.
3. **State a falsifiable hypothesis** ("the cache returns stale entries after
   a restart") and pick the *cheapest* observation that could kill it: a log
   line, an assertion, a debugger breakpoint, one curl.
4. **Bisect the space**, whichever axis is cheapest:
   - history: `git bisect run ./repro`
   - data: half the failing input, recurse
   - stack: confirm the bad value at the boundary between two layers, then
     descend into the guilty one only
5. **The fix must explain the symptom.** Before writing it, say *why* this
   cause produces *exactly* this behavior. If the explanation is fuzzy, you've
   found *a* bug, maybe not *the* bug.
6. **Prove it**: the repro from step 1 now passes, AND a new regression test
   fails without the fix. Then look for siblings — the same mistake usually
   exists elsewhere (`grep` for the pattern you just fixed).

## Anti-patterns

- Changing two things between observations — you learn nothing from the result.
- "It works now" without knowing why it failed — it will be back.
- Adding sleeps to fix timing issues — that's hiding the race, not fixing it.
- Debugging through the framework before confirming your own code's inputs
  and outputs at the boundary.
