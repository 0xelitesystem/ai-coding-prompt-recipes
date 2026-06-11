# refactor-without-breaking

## When to use

Restructuring existing working code. The risk is the agent "improves" things and breaks behavior the tests didn't cover.

## What to provide

- The file(s) to refactor
- The reason for the refactor (architecture, performance, readability, be specific)
- Existing test coverage status (if any)

## The prompt

```
Refactor the file(s) below. Goal: [SPECIFIC GOAL, e.g. "extract
duplicated parsing logic into a single function"].

Hard rules:
1. External behavior MUST stay identical. Same inputs produce same outputs.
2. Public function signatures don't change unless I explicitly approve.
3. No new dependencies.
4. If you find a bug while refactoring, point it out, but DO NOT fix it
   in the same change. Fixes happen in a separate step.

Before writing the refactored code:
- List the existing tests that cover this code (or note "none found").
- If coverage is weak, list 3-5 test cases I should add BEFORE
  refactoring, with input/output pairs.

After the refactor:
- Diff summary: what changed structurally vs textually
- What I should verify manually because tests can't catch it
```

## What good output looks like

- Lists existing tests (or honestly says none)
- If tests are weak, suggests specific tests with concrete I/O
- Refactored code preserves behavior (verify by running existing tests + new ones)
- Diff summary mentions structural changes specifically

## Common failure modes

- **Adds "improvements" silently.** Renames a parameter that's referenced elsewhere. Always grep for callers after.
- **Changes a behavior the agent thinks is a bug.** Catch by running tests and comparing manually-tested edge cases.
- **Introduces a dependency you didn't ask for.** Always check the new imports.
- **Refactors AND fixes a bug in one diff.** Reject and ask for separate diffs. The two changes need to be reviewed independently.

## Variations

**For untested legacy code:**
Add: "If the existing code has no tests, write characterization tests FIRST that capture current behavior including any bugs. Run them. Then refactor. The tests should still pass."

**For performance refactors:**
Add: "Include before/after performance numbers. Use the existing benchmark suite (or write a minimal one). Refactor must not regress benchmarks beyond [tolerance]%."
