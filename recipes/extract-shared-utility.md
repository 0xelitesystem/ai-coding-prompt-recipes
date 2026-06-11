# extract-shared-utility

## When to use

You see duplicated code across files. You want to extract it to a shared utility without breaking callers.

## What to provide

- The files containing the duplicated code
- Where shared utilities live in your project
- Any naming conventions

## The prompt

```
There's duplicated logic across these files: [LIST FILES].

Step 1: Identify the duplication. Show me what's actually the same and
what's superficially similar. Be honest, sometimes "duplicated" code
is actually doing different things and shouldn't be merged.

Step 2: If there's real duplication worth extracting, propose:
- A name for the shared utility
- The exact signature
- Where it should live (which file, which module)
- Which call sites change and how

Step 3: Wait for approval before refactoring.

Step 4: Make the change as TWO commits:
- Commit 1: add the new utility (it's not used yet)
- Commit 2: switch each call site to use it

Don't proceed past step 1 without showing me the diff analysis. Some
"duplication" is coincidence and the right answer is leave it alone.
```

## What good output looks like

- Honest analysis: "files A and B are doing the same thing; file C looks similar but actually handles a different case"
- Proposed signature is general enough to cover all callers but no more
- Two-commit structure (add then switch)
- Tests still pass after each commit

## Common failure modes

- **Forces unification of things that shouldn't be unified.** Two functions that look similar but are doing different things end up sharing a "generic" utility with 5 boolean flags. Push back.
- **Extracts too aggressively.** A 3-line snippet repeated twice doesn't need a function. The threshold is judgment, not a number.
- **The shared utility becomes a kitchen sink.** Push back: "this utility should do one thing. If callers need different things, they need different utilities."
- **Breaks a caller silently.** Especially if the utility's behavior differs in edge cases from any single caller. Verify with tests.

## Variations

**For "this is duplicated, but the cases are subtly different":**
Don't extract. Add a comment to each instance explaining what's shared and what differs, so future-you doesn't make this mistake again.

**For test setup duplication:**
Test fixtures and helpers have different rules from production utilities. Often the right move is "shared test fixture file in tests/", not a generic utility.
