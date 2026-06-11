# explain-this-foreign-codebase

## When to use

First contact with an unfamiliar codebase. Onboarding to a project. Inheriting code from a contractor.

## What to provide

- Repo access (file tree minimum, ideally read access)
- Any existing docs (README, architecture docs)
- The specific question you're trying to answer

## The prompt

```
I'm new to this codebase. Help me understand it.

Specific question I'm trying to answer: [QUESTION, e.g. "where do
HTTP requests get handled" or "how is auth done" or "what does the
worker process do"]

Approach:
1. First, give me the project's high-level structure. What's the
   entry point? What's the main loop? Where does external input come in?
2. Trace the path from external input to the place that answers
   my specific question.
3. Identify the 3-5 files that are most relevant to my question.
4. Flag any conventions or patterns that aren't obvious from a
   first read.
5. Flag anything that looks unusual, fragile, or like potential
   tech debt.

Don't make stuff up. If you can't tell from the files, say "unclear
from this code, you'd need to ask the author about X."
```

## What good output looks like

- High-level structure matches reality (verify by spot-checking files mentioned)
- Path traced is correct
- Files flagged are the right ones for the question
- Honest about what isn't clear

## Common failure modes

- **Makes up structure that "ought to" exist.** Especially in agents that pattern-match common frameworks. Verify by checking each file actually exists and contains what was claimed.
- **Confuses framework patterns with project patterns.** "This uses MVC because of the folder structure", but the folder structure is a coincidence and there's no MVC. Verify with actual code.
- **Misses domain-specific patterns.** Generic "this is a web app" when actually this is a web app with a complex queueing system the agent didn't notice.
- **Confidently wrong about non-obvious code.** Recursion through dynamic dispatch, metaclasses, code generation, agents struggle. Spot-check.

## Variations

**For "what does this do" on a single function:**
Smaller prompt: "Read this function. In 3-6 sentences, explain: what it does, what calls it, what it calls, and any non-obvious behavior."

**For "is it safe to delete this":**
Different prompt: "Is anything currently using [FILE/FUNCTION]? Search the codebase for all references. Distinguish between: definitely used, possibly used (dynamic dispatch), and dead. If anything is unclear, list it."
