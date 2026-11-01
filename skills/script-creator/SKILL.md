---
name: script-creator
description: This skill should be used when the user asks to "create a script", "write a TypeScript script", "make a bun script", or wants to automate a task with a quick TypeScript utility. It guides script creation using Bun runtime with a solve-first-then-generalize philosophy.
version: 1.0.0
license: MIT
---

## Purpose

Create TypeScript scripts that solve user goals quickly and simply using the Bun runtime.

## Philosophy

1. **Solve the goal first** - Create a minimal, working solution
2. **Then generalise** - Only after it works, offer CLI enhancements and improvements

## Conventions

| Aspect | Convention |
|--------|------------|
| Runtime | bun |
| Language | TypeScript (unless user specifies otherwise) |
| Output | `./scripts/` directory in current working directory |
| Single module | `./scripts/<semantic-name>.ts` |
| Multi-module | `./scripts/<semantic-folder>/main.ts` with supporting modules |
| Naming | kebab-case for files and folders |
| Entry point | Include shebang `#!/usr/bin/env bun` |
| Dependencies | `zod` installed locally in project `package.json` |
| Global dependencies | `commander` installed globally via `bun add -g commander` |
| Style | Prefer composition, simple but effective artefacts |

## Preferred Dependencies

- `commander` - CLI argument parsing (install globally)
- `zod` - Input validation and schema definition (install locally)

## File Structure Examples

### Single Module Script

```
./scripts/fetch-users.ts
```

### Multi-Module Script

```
./scripts/data-processor/
├── main.ts          # Entry point with shebang
├── parser.ts        # Parsing logic
├── transformer.ts   # Data transformation
└── types.ts         # Shared types
```

## Implementation Process

### Step 1: Understand the Goal

- Ask clarifying questions if the user's goal is ambiguous
- Identify inputs, outputs, and core transformation

### Step 2: Ensure Project Setup

- Check if `package.json` exists at project root
- If not, create minimal `package.json` with `type: "module"`
- Install local dependencies: `bun add zod`
- Ensure global dependency exists: `bun add -g commander`

### Step 3: Create Scripts Directory

- Create `./scripts/` if it doesn't exist

### Step 4: Implement Minimal Solution

- Create the simplest script that solves the user's goal
- Use a semantically meaningful kebab-case name
- Include shebang: `#!/usr/bin/env bun`
- Focus on working code, not perfect code

### Step 5: Explain Usage

- Show the user how to run the script
- Example: `bun run ./scripts/<name>.ts`
- Or directly: `./scripts/<name>.ts` (if executable)
- Ask if testing is required

### Step 6: Offer Generalisations

Only after the solution works, offer to enhance with:

- CLI arguments (using commander) for flexibility
- Input validation (using zod) for robustness
- Configuration options
- Better error messages
- Splitting into modules if complexity warrants

## Script Template

```typescript
#!/usr/bin/env bun

// Minimal solution first
// [implementation]
```

## CLI Enhancement Template (for generalisation phase)

```typescript
#!/usr/bin/env bun

import { program } from "commander"
import { z } from "zod"

// Define input schema
const InputSchema = z.object({
  // fields
})

program
  .name("script-name")
  .description("What this script does")
  .argument("<required-arg>", "description")
  .option("-o, --option <value>", "description", "default")
  .action(async (arg, options) => {
    // Validate inputs
    const validated = InputSchema.parse({ /* ... */ })

    // Execute core logic
  })

program.parse()
```

## Anti-Patterns to Avoid

- Over-engineering before the solution works
- Creating abstractions prematurely
- Using classes when functions suffice
- Adding features the user didn't ask for (until generalisation phase)
- Complex inheritance hierarchies

## Composition Patterns to Prefer

- Small, focused functions that do one thing
- Pipe data through transformations
- Return new data rather than mutating
- Use plain objects and arrays over class instances
- Compose functions to build complex behaviour
