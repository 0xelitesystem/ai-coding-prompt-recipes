# migrate-deprecated-API

## When to use

A library or API you depend on has changed. Old code uses deprecated patterns. You want a systematic migration.

## What to provide

- The deprecated pattern (with link to the deprecation notice if available)
- The new pattern
- A sample of code that uses the deprecated pattern
- The blast radius (one file? whole codebase?)

## The prompt

```
[LIBRARY] has deprecated [OLD PATTERN] in favor of [NEW PATTERN].

Old:
```
[EXAMPLE]
```

New:
```
[EXAMPLE]
```

Source: [LINK TO DEPRECATION NOTICE if you have one]

Goal: migrate all usage in [SCOPE].

Approach:
1. Find all uses of the old pattern. List them with file:line.
2. Group by complexity:
   - "Trivial": straightforward 1:1 replacement
   - "Needs review": requires choices about parameters or error handling
   - "Complex": pattern doesn't map cleanly, needs case-by-case
3. Show me the list before making any changes.
4. After approval, do the trivial ones first as one commit.
5. Then we'll go through "needs review" items together.
6. "Complex" items get individual decisions.

Don't migrate "complex" items autonomously. Ask.
```

## What good output looks like

- Complete list of usages (verify with your own grep, agents miss things)
- Honest grouping (the "trivial" ones really are trivial)
- One commit per category, not all-or-nothing
- Tests still pass after each commit

## Common failure modes

- **Misses usages.** Especially indirect ones (re-exports, dynamic dispatch). Always grep yourself.
- **Categorizes "complex" as "trivial" to seem productive.** Spot-check.
- **Migrates code that should be deleted instead.** Some code uses deprecated APIs because it's deprecated itself. Worth asking: "should we migrate or delete?"
- **Loses behavior subtly.** Old API's defaults differ from new API's defaults. Verify behavior with tests.

## Variations

**For framework version bumps (e.g. React 17 -> 18):**
Use this recipe per breaking change rather than as one big migration. Easier to review, easier to roll back individual pieces.

**For libraries with codemods available:**
Run the official codemod first. Then use this recipe for whatever the codemod missed.
