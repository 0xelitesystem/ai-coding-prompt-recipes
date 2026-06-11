# add-feature-incrementally

## When to use

Adding a new capability to a working application. The wrong approach is "add the whole feature in one diff." The right approach is multiple small steps, each one shippable on its own.

## What to provide

- What the feature does (user-facing)
- Where it fits in the existing app
- Which files probably need to change

## The prompt

```
I want to add [FEATURE] to the app. Before writing any code, break
the work into 3-7 small steps where each step:

- Is a coherent change reviewable in <100 lines
- Leaves the app in a working state when committed
- Has a clear verification you can run (test, manual check, command)

Format the plan as:

Step 1: [short title]
- What changes: [files + what]
- Why this is step 1: [dependency reasoning]
- How to verify: [command or check]
- Commit message: [proposed]

Step 2: ...

Once the plan is approved, do step 1. Stop. Wait for me to verify.
Then do step 2.

Constraints:
- [LIST]
```

## What good output looks like

- 3-7 steps, not 1 step or 15 steps
- Each step has a clear verification
- Steps build on each other in dependency order
- Each step's commit message reads like a real commit, not "step 1 of feature X"

## Common failure modes

- **Step 1 changes 8 files.** Push back: "step 1 is too big. Can you break it up?"
- **Steps aren't independently shippable.** If step 2 leaves the app broken until step 3, that's not incremental, that's chunked-up monolithic work.
- **Verification is "the tests pass" with no specifics.** Push back: "what specific test or check verifies this step?"
- **Agent does step 2 before you verify step 1.** Force the pause: "I haven't verified step 1 yet. Wait."

## Variations

**For features that need data migration:**
Add: "If any step requires a data migration or schema change, that step must include both forward migration AND rollback path."

**For features with feature flags:**
Add: "Wrap the new behavior in a feature flag. Step 1 should add the flag (defaulting OFF). The feature should not be visible to users until the final step."
