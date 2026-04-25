---
name: typescript-guidelines
description: Mandatory TypeScript strict mode and type-safety guidelines. Use whenever writing new TypeScript, improving existing TypeScript, reviewing TypeScript, refactoring TypeScript, deciding between schemas and plain TypeScript definitions, choosing type vs interface, modeling domain values, configuring tsconfig, or checking TypeScript code for unsafe patterns.
license: MIT
metadata:
  author: itzcull
  version: "0.1.0"
---

# TypeScript Guidelines

## Purpose

Use TypeScript to make illegal states hard to represent, parse unknown input at system boundaries, and keep internal code simple, explicit, and refactorable.

This skill turns the project TypeScript guidance into an operational checklist for code generation, review, and convention decisions.

## When to Use

Use this skill whenever TypeScript is being written, changed, reviewed, or configured.

This includes:

- Writing new TypeScript application code, tests, utilities, or shared contracts.
- Improving old TypeScript code, even when the requested change is small.
- Refactoring TypeScript without changing behavior.
- Reviewing TypeScript for correctness, maintainability, type safety, boundary parsing, unsafe assertions, or schema duplication.
- Deciding whether a value needs a runtime schema or only a compile-time TypeScript definition.
- Choosing between `type` and `interface`, especially for data shapes, component props, argument objects, and configuration options.
- Setting up or auditing `tsconfig` strictness.
- Creating test factories or shared test data for TypeScript code.

Do not treat this as optional guidance. If a task involves TypeScript, apply these rules unless the user explicitly asks to ignore them.

## Core Rules

- Run TypeScript in strict mode.
- Do not use `any`; use `unknown` when a value is genuinely unknown.
- Do not use type assertions unless the justification is explicit and local.
- Do not use `@ts-ignore` or `@ts-expect-error` without an explanation of why the type system cannot express the case.
- Apply the same type-safety rules to tests as production code.
- Prefer readonly data by default.
- Prefer domain-specific types over generic primitives when values are easy to mix up.
- Define schemas first at trust boundaries and derive types from schemas.
- Parse once at the boundary, then trust parsed types internally.
- Use real shared schemas and types in tests; never redefine production contracts in test files.

## Strict Mode Baseline

Prefer this compiler baseline unless the existing project has a stricter standard:

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true
  }
}
```

## Type Modeling

Use types that encode meaning, not just structure.

```typescript
type UserId = string & { readonly brand: unique symbol };
type PaymentAmount = number & { readonly brand: unique symbol };

type User = {
  readonly id: UserId;
  readonly email: string;
  readonly role: UserRole;
};
```

Guidelines:

- Use explicit annotations when they improve clarity.
- Rely on inference when the inferred type is already precise.
- Use `Pick`, `Omit`, `Partial`, `Required`, and mapped types when they communicate intent better than duplicating a shape.
- Use discriminated unions for state and workflow variants.
- Introduce branded types when plain primitives would allow mixing distinct domain values.

## Type vs Interface

Use `type` for domain data. Use `interface` for call signatures and behavior contracts.

Use `type` for:

- Object data shapes.
- Unions and discriminated unions.
- Intersections.
- Mapped types.
- Domain data structures.
- Internal state shapes.

Use `interface` for:

- Component props.
- Argument objects.
- Configuration options.
- Ports.
- Adapters.
- Dependency injection contracts.
- Behavior-oriented abstractions.

Example:

```typescript
type PaymentRequest = {
  readonly amount: number;
  readonly currency: string;
};

interface PaymentFormProps {
  readonly payment: PaymentRequest;
  readonly onSubmit: (payment: PaymentRequest) => void;
}

interface ChargePaymentOptions {
  readonly request: PaymentRequest;
  readonly idempotencyKey: string;
}

interface Logger {
  log(message: string): void;
  error(message: string, error: Error): void;
}
```

## Schema-First Boundaries

Parse data at trust boundaries. Do not trust external input because it has a TypeScript type.

Use a Standard Schema-compatible library. `Zod` is the default example.

```typescript
import { z } from "zod";

const AddressDetailsSchema = z.object({
  houseNumber: z.string(),
  addressLine1: z.string().min(1),
  city: z.string().min(1),
  postcode: z.string().min(1),
});

const PaymentRequestSchema = z.object({
  cardAccountId: z.string().length(16),
  amount: z.number().positive(),
  source: z.enum(["Web", "Mobile", "API"]),
  addressDetails: AddressDetailsSchema,
});

type AddressDetails = z.infer<typeof AddressDetailsSchema>;
type PaymentRequest = z.infer<typeof PaymentRequestSchema>;

export const parsePaymentRequest = (input: unknown): PaymentRequest => {
  return PaymentRequestSchema.parse(input);
};
```

Rules:

- Define schemas first when data crosses a runtime boundary.
- Derive types from schemas instead of duplicating shape definitions.
- Keep parsing and validation close to the boundary.
- Avoid revalidating parsed internal values repeatedly.

## When Schemas Are Required

Use a schema when any of these are true:

- Data crosses a trust boundary.
- The type encodes runtime validation rules.
- The shape is a shared contract between systems.
- Test factories need runtime validation against a real production contract.

Typical schema-required cases:

- API requests and responses.
- Database records at adapter boundaries.
- Queue messages and domain events.
- URL params, form data, and environment variables.
- Reusable test data validated against production contracts.

## When Plain TypeScript Definitions Are Enough

Plain TypeScript definitions are enough when the value is purely internal and has no runtime validation need.

Typical plain-type cases:

- Internal state.
- Utility types.
- Discriminated unions for control flow.
- Branded primitives.
- Behavior contracts.
- Component props, argument objects, and configuration options that do not cross a trust boundary.

Example:

```typescript
type Point = {
  readonly x: number;
  readonly y: number;
};

type Result<T, E = Error> =
  | { readonly success: true; readonly data: T }
  | { readonly success: false; readonly error: E };

type LoadingState =
  | { readonly status: "idle" }
  | { readonly status: "loading" }
  | { readonly status: "success"; readonly data: unknown }
  | { readonly status: "error"; readonly error: Error };
```

## Decision Framework

Ask these questions in order:

1. Does this value enter from outside the system?
2. Does it have runtime validation rules?
3. Is it a shared contract?
4. Will tests rely on the real contract?

If the answer to any question is yes, use a schema and derive the type from it.

If the value is purely internal and compile-time-only, use a plain TypeScript definition.

## Test Data Rules

- Test files must import real schemas and types from shared locations.
- Test factories should return complete valid objects with sensible defaults.
- Test factories may accept `Partial<T>` overrides for focused variation.
- When a production schema exists, validate factory output with that schema.
- Never redefine API, database, event, or shared domain contracts inside tests.

Example:

```typescript
const buildUser = (overrides: Partial<User> = {}): User => {
  return UserSchema.parse({
    id: crypto.randomUUID(),
    email: "person@example.com",
    role: "user",
    ...overrides,
  });
};
```

## File Naming

Use `kebab-case` file names.

Use dedicated suffixes when a module exports only schemas or only types:

- `*.schemas.ts` for runtime schemas.
- `*.types.ts` for compile-time-only shared types.

Examples:

```text
user.schemas.ts
user.types.ts
payment.schemas.ts
payment.types.ts
```

Keep local-only types near the implementation. Split schemas or types into dedicated modules when they are shared or when separation materially improves clarity.

## Review Checklist

- Does the code avoid `any` and unjustified assertions?
- Are unknown inputs typed as `unknown` until parsed?
- Are trust boundaries parsed once with real schemas?
- Are schema-derived types used instead of duplicated contract shapes?
- Are internal-only values modeled with simple plain types?
- Are data structures `readonly` where practical?
- Are `type` and `interface` used according to domain data vs call signature and behavior semantics?
- Do tests import real schemas and shared types?
- Do file names follow `kebab-case`, `*.schemas.ts`, and `*.types.ts` conventions?

## Output

When applying this skill, produce one of these outputs depending on the task:

- For implementation: TypeScript code that passes strict type checking and uses schemas at boundaries.
- For review: Findings ordered by risk, with file references and concrete fixes.
- For convention decisions: A direct recommendation with the schema/plain-type rationale.
- For setup: A strict `tsconfig` baseline and migration notes for any existing incompatibilities.

## References

- Source guidance: `~/.claude/docs/typescript.md`
- Related testing guidance: `~/.claude/docs/testing.md`
- Related workflow guidance: `~/.claude/docs/workflow.md`
