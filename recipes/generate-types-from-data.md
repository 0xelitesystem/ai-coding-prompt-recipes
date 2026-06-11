# generate-types-from-data

## When to use

You have JSON / YAML / sample data, you need typed interfaces (TypeScript, Python TypedDict / Pydantic, Go structs, etc.).

## What to provide

- One or more samples of the data
- The target language and type system
- Any conventions (snake_case vs camelCase, optional handling, enum representation)

## The prompt

```
Generate [TARGET TYPE SYSTEM] types for the data below.

Target: [TYPESCRIPT / PYTHON PYDANTIC / GO STRUCT / etc.]
Naming convention: [camelCase / snake_case / mixed]
Optional handling: [strict optional / nullable / Maybe]

Samples:
```
[PASTE 1+ samples]
```

Rules:
1. If a field appears in some samples but not others, mark it optional.
2. If a field can be one of N values across samples, generate a union/enum.
3. Comment any field whose type is uncertain (e.g. "this is a string in
   sample 1 but null in sample 2 - might be optional or might be a
   sentinel").
4. Don't invent fields not in the samples.
5. For nested objects, generate separate types and reference them.

Output:
- The types
- A "uncertainty" section listing fields where you guessed and why
```

## What good output looks like

- Types match the samples exactly
- Optional / nullable handling is consistent with the language conventions
- Nested types are reused, not duplicated
- Uncertainty section flags real ambiguities

## Common failure modes

- **Inferred types from too few samples.** "id: number" but later there's a sample where it's "id: string". Always provide multiple samples covering edge cases.
- **Made-up Pydantic / TypeScript syntax.** Verify the syntax compiles before using.
- **Lost precision.** "any" or "object" everywhere instead of attempting a real type. Push back.
- **Inferred enums from incidental values.** Three samples happen to have status="active" - agent generates `type Status = "active"`. Flag for review.

## Variations

**For OpenAPI / JSON Schema -> types:**
Different recipe: parse the schema and translate field-by-field rather than inferring from samples. Schemas are authoritative; samples are evidence.

**For incremental migration:**
Add: "If types differ from existing types in the codebase, point out the diff. Don't silently generate competing definitions."
