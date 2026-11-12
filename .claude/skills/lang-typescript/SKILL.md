---
name: lang-typescript
description: TypeScript rules, for .ts and .tsx files only. Preloaded by the planner, the coder and the judge; apply them to those files and to no other.
---

These rules govern every `.ts` and `.tsx` file of a work-package. The coder writes to them and the judge cites them by number, from the same file. Conventions specific to the repo live in the exemplar module named in the brief. What the compiler enforces stays with it, and so does what the linter enforces where the repository enforces its lint, as the commit skill says.

## Principle

The type checker is a proof assistant. Use it to rule out impossible states, mixed-up primitives and unhandled variants at compile time: a case the types let you ignore becomes a runtime failure the compiler could have stopped. Define errors and special cases out of existence rather than multiplying handlers.

Validation lives at the boundaries of the system: CLI arguments, configuration, network, external APIs, stored data. Inside, typed data flows without re-validation, and business logic lives in pure functions that thin adapters and handlers call.

## Rules

1. **Discriminated unions.** Model variants with a `kind` literal discriminant so impossible states cannot be represented. No bag of optional fields.
2. **Branded types.** Brand semantic primitives with `& { readonly __brand: "X" }` so they cannot be mixed up. Validate once, at creation.
3. **Constructive modeling.** Build the shape so the illegal value cannot be constructed: `[T, ...T[]]` for non-empty, `[T, T][]` for even length. Numeric bounds still need boundary validation; a different field name does not constrain a `number`.
4. **Simplest total type.** Keep `T[]` while every operation on it stays total. Strengthen to `NonEmpty<T>` only where the loose type forces `!`, a cast, or a "should never happen" throw.
5. **`unknown` over `any`.** External data is `unknown` until parsed.
6. **Schemas before guards.** Before writing a property-by-property type guard, use the repo's runtime schema library and infer the type from the schema.
7. **No `as` casts.** Cast only after validation has proved the claim.
8. **Narrowing hierarchy.** Discriminant switch, then the `in` operator, then `typeof` or `instanceof`, then a user-defined guard, and `as` last.
9. **Honest type guards.** A guard verifies its claim and is named `isX` or `hasX`.
10. **Exhaustiveness.** Put `const _exhaustive: never = x;` in default arms so the compiler fails when a variant is added.
11. **`satisfies` over `as`.** It validates the value without widening literal types.
12. **Boundary validation.** Parse where data crosses in, into a named domain type. `Record<string, unknown>`, however spelled, stops at that parse. Trust the types inside and skip redundant checks deep in call chains.
13. **Schema-derived types.** Reach for `Pick`, `Omit`, `Parameters`, `ReturnType`, `Awaited` or `typeof`, or the types generated from an authoritative schema, before declaring a new interface.
14. **Object arguments.** Pass objects, not positional arguments, so argument order documents itself. Hot paths are the exception: per-frame render, tokenizers, parsers.
15. **Domain types at the surface.** The public surface of a module that holds a domain concept exposes domain concepts. Transport, storage, framework and wire types stay behind it.
16. **Pure core.** Business logic, parsing, prompt construction and scoring are pure functions with no framework dependency; adapters and handlers only call them.
17. **Structured telemetry.** Log through the structured logger, with enough context to debug from an id. No `console.log` in shipped code.

## Self-checks

- Can a comment explain when this combination of fields is valid? Then the type is too loose: split it into a union.
- Do two arguments share a primitive type but mean different things? Brand them.
- Where did this `any`, this `as`, this non-null assertion come from? Trace it to the boundary and validate there.
- If a variant is added next month, will the compiler point to every place that needs a case?
- Does this type duplicate a shape another file owns? Derive it.
- Is this data crossing a boundary right now? If not, validating it again is noise.

Examples for most rules: [`PATTERNS.md`](PATTERNS.md).
