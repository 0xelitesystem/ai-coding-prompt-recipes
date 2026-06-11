# code-review-this-pr

## When to use

You have a diff (your own or a teammate's) and want a quick AI review pass before requesting human review.

## What to provide

- The diff (unified format)
- Context about what the change is supposed to do
- The repo conventions if not obvious from the diff

## The prompt

```
Review this diff. The change is supposed to: [GOAL].

Review structure:
1. Does the change actually do what it claims? Verify by reading the
   code, not the description.
2. Bugs or correctness issues you can spot from this diff alone.
3. Tests: are the new ones meaningful? Do they actually test the
   behavior or just exercise the code?
4. Security flags: input validation, auth, data exposure, injection,
   secrets in code, missing rate limits.
5. Style/idiom issues that matter (skip nitpicks like spacing).
6. Things that aren't in the diff but probably should be (missing
   error handling, missing log statements, missing migration).

Output format:
- Verdict: [APPROVE / REQUEST CHANGES / NEEDS DISCUSSION]
- Top 3 issues (most important first)
- Other observations (less critical)
- Questions for the author

Be specific. "X is bad" is not feedback. "Line 42: X is bad because
Y; suggest Z" is feedback.

Diff:
```
[PASTE]
```
```

## What good output looks like

- Verdict is decisive
- Top 3 issues are actually issues, not bikeshedding
- Each issue has line reference, problem, and suggested fix
- Security flags are explicit if relevant
- Questions for the author cover real ambiguities

## Common failure modes

- **Approves everything.** Catch by asking: "is there ANYTHING you'd flag in this diff?" If still nothing, the agent is in sycophant mode.
- **Hallucinates issues that aren't there.** "Line 42 has a SQL injection" when line 42 is a comment. Always verify against the actual diff.
- **Misses the obvious.** A glaring N+1 query or a deleted test that the agent didn't flag. The fix: pair AI review with human review, not replace.
- **Style nitpicks dominate.** Push back: "skip style. What are the real issues?"

## Variations

**For security-critical code:**
Replace the prompt with: "Review this diff with a security mindset. Assume the change is in a security boundary. Flag every input that crosses a trust boundary. Check auth, authorization, injection (SQL/HTML/shell/path), secret handling, rate limiting, and audit logging. Be paranoid."

**For performance-sensitive code:**
Add: "This code is in a hot path. Flag any allocation in the hot loop, unnecessary copies, suboptimal data structures, or sync I/O that could be async."
