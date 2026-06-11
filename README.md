# ai-coding-prompt-recipes

Specific prompt recipes for common dev tasks with coding agents (Claude Code, Cursor, Aider, Continue, etc.). Each recipe is opinionated about the situation, the prompt, what to verify, and where it commonly fails.

## Why this exists

Most "prompt collections" online are decontextualized one-liners. A recipe is different: it tells you when to use it, what to provide alongside it, what good output looks like, and what failure modes to watch for. That's the gap.

## Recipes

| Recipe | Use when |
|---|---|
| [scope-this-task](./recipes/scope-this-task.md) | Before letting an agent loose on something nontrivial |
| [refactor-without-breaking](./recipes/refactor-without-breaking.md) | Restructuring existing code |
| [add-feature-incrementally](./recipes/add-feature-incrementally.md) | Adding capability to a working app |
| [debug-with-minimal-changes](./recipes/debug-with-minimal-changes.md) | Stuck on a bug, want surgical fix |
| [generate-tests-for-untested-code](./recipes/generate-tests-for-untested-code.md) | Filling test gaps in legacy code |
| [translate-between-languages](./recipes/translate-between-languages.md) | Porting code from one language to another |
| [code-review-this-pr](./recipes/code-review-this-pr.md) | Have AI review a diff |
| [explain-this-foreign-codebase](./recipes/explain-this-foreign-codebase.md) | First contact with unfamiliar code |
| [generate-types-from-data](./recipes/generate-types-from-data.md) | Creating typed interfaces from samples |
| [migrate-deprecated-API](./recipes/migrate-deprecated-API.md) | Library/API has changed, code needs updates |
| [extract-shared-utility](./recipes/extract-shared-utility.md) | Pulling a reusable function out of duplicated code |
| [security-audit-this](./recipes/security-audit-this.md) | Quick security pass on AI-generated code |

## Recipe format

Every recipe follows this structure:

```markdown
# Recipe name

## When to use
[Specific situation, not "general purpose"]

## What to provide
[Files, context, constraints the agent needs]

## The prompt
[Actual prompt text, as a fenced block, ready to paste]

## What good output looks like
[Specific things to verify in the response]

## Common failure modes
[How this recipe fails and what to do]

## Variations
[Adjustments for different stacks or sizes]
```

## Contribute

PRs welcome. See [CONTRIBUTING.md](./CONTRIBUTING.md). The bar:

- Recipe must address a SPECIFIC situation, not a generic one
- Must include a concrete prompt, not a description of one
- Must include verification steps
- Must include known failure modes (every recipe has them)

## License

MIT.

## Related

- [vibe-coding-anti-patterns](https://github.com/0xelitesystem/vibe-coding-anti-patterns) - what AI does badly + how to catch it
- [ai-generated-code-review-rubrics](https://github.com/0xelitesystem/ai-generated-code-review-rubrics) - rubrics for reviewing AI-generated code
- [agentic-workflow-patterns](https://github.com/0xelitesystem/agentic-workflow-patterns) - working with coding agents
- [prompt-templates](https://github.com/0xelitesystem/prompt-templates) - general (non-coding) prompt templates
