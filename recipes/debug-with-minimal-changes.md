# debug-with-minimal-changes

## When to use

You have a specific bug. You want it fixed surgically. You do NOT want the agent to also "clean up" 5 unrelated things while it's there.

## What to provide

- Reproduction steps
- Expected vs actual behavior
- Relevant log output or stack trace
- Any debugging you already did

## The prompt

```
Bug: [DESCRIBE, be specific]

Reproduce:
1. [STEP]
2. [STEP]
3. [STEP]

Expected: [BEHAVIOR]
Actual: [BEHAVIOR]

Stack trace / log:
```
[PASTE]
```

What I tried already: [STEPS]

Investigate this bug. Hard rules:
1. Find the root cause before proposing a fix.
2. Propose the SMALLEST change that fixes it.
3. Don't refactor surrounding code in the same change.
4. If the surrounding code has unrelated issues, list them as separate
   follow-ups but DO NOT touch them.

Output format:
- Root cause: [1-3 sentences]
- Why it manifests as [the symptom]: [explanation]
- Proposed fix: [diff or description]
- Test that should be added so this doesn't regress: [code]
- Other issues spotted but NOT fixing: [list]
```

## What good output looks like

- Identifies a specific root cause (not "this might be related to X")
- Proposed fix is small (often 1-5 lines)
- Test case demonstrates the bug failing AND the fix passing
- Doesn't touch unrelated code

## Common failure modes

- **Agent "improves" surrounding code.** Push back: "I asked for a minimal fix. Why are you also changing X?"
- **Proposed fix doesn't address the actual root cause.** Catch by asking: "If this is the cause, why does removing the fix bring back the bug? Walk me through it."
- **Test doesn't actually exercise the bug path.** Verify the test fails before the fix is applied. If it doesn't fail, it's not a real regression test.
- **Multiple proposed fixes.** Pick one. Iterate on the others later.

## Variations

**For intermittent bugs:**
Add: "This bug is intermittent. Before proposing a fix, identify the timing or state condition that causes it to manifest. The fix must address that condition, not just patch the symptom."

**For bugs that involve external systems (DB, API):**
Add: "Determine whether the bug is in our code, in our usage of the external system, or in the external system itself. Fix only what's ours."
