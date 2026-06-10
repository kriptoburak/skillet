---
name: test-writing
description: Write tests that catch real regressions — behavior-focused, minimal mocking, edge cases chosen from the code's actual branches. Use when adding tests for new code or backfilling tests for a fix.
version: 1.0.0
license: MIT
keywords: [testing, unit-tests, tdd, quality, regression]
---

# test-writing

A test's value is the bug it catches later. Test **behavior through the public
surface**, not implementation details — a good test survives a refactor; a bad
one breaks with it.

## Procedure

1. **Match the house style first**: open 2–3 existing test files; copy their
   runner, naming, setup helpers and assertion idioms exactly.
2. **One behavior per test**, named as a claim: `install skips a broken entry
   and installs the rest` — readable as a spec when the suite prints.
3. **Choose cases from the code's branches**, not from imagination. Read the
   function: every `if`, every early return, every `catch` is a candidate.
   Priority order:
   - the happy path (one test, not five variations)
   - each error/edge branch (empty input, missing file, duplicate, too-long,
     zero/negative, unicode)
   - the bug you just fixed (regression test that fails without the fix —
     verify it actually fails by stashing the fix once)
4. **Real objects over mocks.** Mock only true boundaries (network, clock,
   randomness). If a test needs four mocks, the unit is wrong — test one level
   higher. For clock/randomness, inject (`now`, `seed`) rather than patching
   globals.
5. **Make failure diagnosable**: assert on specific values, not just
   truthiness; add a message when the assertion isn't self-explanatory
   (`assert.ok(hit1 >= 0.88, 'ranking change regressed recall')`).
6. **Independence**: each test builds its own state (tmp dirs via `mkdtemp`,
   fresh fixtures) and cleans up. Order-dependence is a flake factory.

## Anti-patterns

- Asserting a function was *called* instead of what it *did*.
- Snapshot tests for logic (they assert "nothing changed", not "it's correct").
- Testing private helpers directly — route through the public API.
- A test that can't fail (asserting the mock you just configured).
