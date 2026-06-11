# Contributing a Recipe

## What makes a good recipe

A recipe addresses a SPECIFIC situation, not a generic one. "Generate code from a prompt" is not a recipe; "generate types from JSON samples in TypeScript with strict optional handling" is a recipe.

Every recipe must include:

1. **When to use** - the specific situation (not "general purpose")
2. **What to provide** - context the agent needs
3. **The prompt** - actual prompt text in a fenced block, ready to paste
4. **What good output looks like** - verifiable success criteria
5. **Common failure modes** - how this recipe fails in practice
6. **Variations** - adjustments for related situations

## Submission

1. Add a new file in `recipes/`
2. Use the existing recipes as templates
3. Add a row to `README.md` table
4. Open a PR

PR title: `add recipe: name-of-recipe`

## What gets rejected

- Recipes that are really general advice ("be specific in your prompts")
- Recipes without verifiable success criteria
- Recipes that don't acknowledge failure modes (every recipe has them)
- Recipes that recommend a specific paid tool by name
- Recipes that promise specific outcomes ("this prompt gets perfect code")
- Recipes copied from blog posts without rewriting and adding the structure

## What gets accepted gladly

- Recipes for stacks not yet covered (Rust, Elixir, Swift, Kotlin, Clojure)
- Recipes for specific tools (Aider, Continue, Zed AI, Codeium) where they differ meaningfully from the general approach
- Recipes for situations the existing 12 don't cover
- Improvements to existing recipes based on field experience
- Translations (recipes in non-English languages, with the same structure)

## Updating an existing recipe

PR title: `update recipe: name`. Explain what changed and why.

If the change is large enough that it's a different recipe, consider keeping the old one and adding a new one instead.

## License

By contributing, you agree your contribution is MIT-licensed.
