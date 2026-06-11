# scope-this-task

## When to use

Before letting a coding agent loose on anything beyond a simple file edit. The biggest cause of bad agent output is bad scope: the agent decides to refactor your entire architecture when you wanted a one-line fix.

## What to provide

- The high-level goal in one sentence
- The specific files involved (or "I don't know which files yet")
- Hard constraints (don't touch X, must keep Y working)

## The prompt

```
I want to [GOAL]. Before any code, do this:

1. Read the relevant files and tell me, in 3-6 bullets, what
   currently exists that's relevant to this task.
2. List the smallest set of files that need to change.
3. Outline the plan as numbered steps.
4. Flag anything you're unsure about as an explicit question.
5. STOP and wait for me to confirm before writing any code.

Constraints I care about:
- [LIST: don't touch X, keep Y working, no new dependencies, etc.]

Do not propose a full refactor. Default to the minimum change that
solves the problem.
```

## What good output looks like

- The agent confirms what currently exists (proves it actually read the files)
- The file list is short (1-4 files for most tasks)
- The plan is concrete steps, not vague phases
- There ARE questions, or an explicit "no questions"
- No code yet

If the response immediately starts writing code, the agent skipped the scoping step. Repeat: "stop, scope first."

## Common failure modes

- **Agent invents files that don't exist.** It read SOME files and extrapolated the rest. Push back: "you mentioned `src/auth/middleware.ts` but I don't think that exists. Show me the actual file paths you read."
- **Plan is too ambitious.** Says "refactor the auth module to use the new pattern." Push back: "you're proposing more than I asked. What's the minimum change that addresses the goal?"
- **No questions when there should be questions.** If the goal is genuinely ambiguous and the agent has no questions, it's about to make incorrect assumptions silently. Ask: "what assumptions are you making about [the ambiguous part]?"

## Variations

**For greenfield code (no existing files to read):**
Skip step 1. Replace with: "1. Restate the goal in your own words including any assumptions you're making about the stack and conventions."

**For very small tasks:**
Don't use this recipe. Just describe the task and let the agent edit. The recipe overhead isn't worth it for one-line changes.

**For agents that don't have file-reading tools:**
You provide the files. Paste them. Then run the recipe.
