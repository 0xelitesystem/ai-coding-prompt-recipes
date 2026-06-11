# generate-tests-for-untested-code

## When to use

You have a function (or module) that has no tests. You want to add tests so you can refactor or modify with confidence.

## What to provide

- The file(s) to test
- The test framework being used in the project
- Any conventions (file naming, mock patterns, etc.)

## The prompt

```
The file below has no tests. Generate a test file that covers it.

Approach: characterization tests, not aspirational tests. The tests
should capture what the code currently does, including edge cases
and any bugs. We'll refactor or fix bugs in separate steps.

For each public function:
1. Test the documented happy path
2. Test edge cases I can derive from reading the code (empty inputs,
   nulls, boundary values, error conditions)
3. Test the actual bugs you spot (write a test that captures the
   buggy behavior, with a comment explaining the bug)

Framework: [JEST / PYTEST / GO TEST / WHATEVER]
Conventions: [TEST FILE LOCATION, NAMING, MOCK PATTERNS]

Output:
- The test file
- A list of bugs spotted (with comments referencing the test name
  that captures each)
- A list of behaviors that are ambiguous from reading the code
  (where I should clarify before refactoring)
```

## What good output looks like

- Tests actually run (don't have made-up imports or APIs)
- Tests cover real code paths, not trivial getter/setter checks
- Bugs are flagged separately with the captured-behavior tests marked
- Test names describe behavior, not implementation

## Common failure modes

- **Tests test the implementation, not behavior.** "It calls method X" instead of "given input Y, output is Z." Push back.
- **Mocks everything.** A test that mocks every dependency tests nothing. Push back: "which of these mocks are actually necessary? Reduce to the minimum."
- **Made-up matchers/assertions.** Especially common with newer test frameworks. Verify every assertion exists.
- **Tests that pass regardless.** Run them. If they all pass first try, run a coverage report. Low coverage = trivial tests.

## Variations

**For pure functions:**
Property-based tests can be high-leverage. Add to the prompt: "If the function is pure and operates on simple types, suggest 1-3 property-based tests using [hypothesis / fast-check / quickcheck]."

**For code with side effects (DB, network, filesystem):**
Add: "Tests that hit real external systems are out of scope here, those go in integration tests. Mock the boundaries; test the logic."

**For code you don't trust to characterize:**
Run the tests against the original code first. They should all pass. If they don't, the tests are wrong (not the code).
