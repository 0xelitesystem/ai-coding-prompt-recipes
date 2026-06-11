# translate-between-languages

## When to use

Porting code from one language to another. Common: prototype Python -> production Go, JavaScript -> TypeScript with strict types, legacy PHP -> modern Node.

## What to provide

- The source code
- The target language version
- Any libraries that should be used in the target (or "use stdlib only")
- Performance/style constraints

## The prompt

```
Translate the code below from [SOURCE LANG] to [TARGET LANG VERSION].

Target conventions:
- Style: [IDIOMATIC / WHATEVER]
- Libraries allowed: [LIST or "stdlib only"]
- Error handling: [IDIOMATIC FOR LANG, e.g. "Go errors as values, no panics"]
- Tests: [INCLUDE / SEPARATE / NONE]

Hard rules:
1. Translation must preserve behavior. Same inputs -> same outputs.
2. Use idiomatic [TARGET LANG] patterns, not source-language patterns
   transliterated. (Don't translate Python's `for x in list` as Go's
   `for x := range list` if a `for i := range list` makes more sense.)
3. If you encounter source-language features that have no clean
   equivalent in the target, flag them and propose 2-3 options.
4. Don't add features. Don't fix bugs. Translation only.

Output:
- The translated code
- A "differences" section: things that work differently in the target
  language and how this translation handles them
- A "concerns" section: places I should review carefully
```

## What good output looks like

- Idiomatic in the target language (use `for...range` in Go where appropriate, not transliterated loops)
- Behavior preserved (verify with parallel tests if possible)
- Honest "differences" section about things like null vs undefined, integer overflow behavior, string encoding, error model
- Doesn't fix bugs from the source

## Common failure modes

- **Hallucinated library APIs.** Especially common in less-popular target languages. Verify every imported function actually exists in the version you specified.
- **Subtle integer overflow / signed-unsigned issues.** When translating between languages with different default integer types. Test boundary values.
- **String encoding assumptions.** Source might be UTF-8 implicit; target might be UTF-16 implicit (looking at you, Java). Test non-ASCII.
- **Mutability differences.** Translating immutable data to a mutable language (or vice versa) without thinking.

## Variations

**For prototype to production:**
Add: "Source is a prototype. Production target needs: input validation, error handling for all I/O, no panics on bad input, log levels appropriate for production."

**For legacy modernization:**
Add: "Source uses [DEPRECATED PATTERN]. Replace with modern equivalent. List every replacement made so I can verify."
