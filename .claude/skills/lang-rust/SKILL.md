---
name: lang-rust
description: Rust rules, for .rs files only. Preloaded by the planner, the coder and the judge; apply them to those files and to no other.
---

These rules govern every `.rs` file of a work-package. The coder writes to them and the judge cites them by number, from the same file. Conventions specific to the repo live in the exemplar module named in the brief. What rustc, rustfmt or the configured Clippy lints already enforce stays with them.

Read the crate's `Cargo.toml`, workspace settings, toolchain file and CI commands to learn its edition, minimum supported Rust version, targets, features and runtime. They are context for these rules, not permission to change the toolchain, dependencies or gates.

## Principle

Use ownership and types to express the contract the caller needs. Safe Rust prevents classes of memory errors; it does not establish business validity, freedom from deadlock or correct recovery after cancellation.

Validate external values when they enter domain types. Keep those invariants true through every constructor and mutation. Prefer the smallest concrete API that expresses the required behaviour, without making every struct generic or every dependency a trait.

## Rules

1. **Domain states.** Use enums for mutually exclusive states and newtypes for distinctions that callers must not mix. Reuse standard types such as `Duration`, `Path` and `NonZeroUsize` when their semantics fit. State the invariant each type actually guarantees.
2. **Validated construction.** Keep invariant-bearing fields private. Use a fallible constructor or `TryFrom` for external values. Check every construction and mutation path, including builders, deserialization and `Default`; none may bypass the invariant.
3. **Conversions without hidden loss.** Use `From` for infallible, value-preserving conversions and `TryFrom` when failure matters. Reject or explicitly handle truncation, overflow and out-of-range input. A numeric `as` cast is not validation; choose checked arithmetic when overflow is a possible domain error.
4. **Absence and failure.** Use `Option` when absence alone describes the outcome, and `Result` when the caller needs failure information. Propagate errors with their context. Do not turn a failure into success with `.ok()`, an ignored result or a default unless the contract explicitly defines that recovery.
5. **Errors callers can use.** Expose structured errors where callers must distinguish outcomes. Implement `Display` and `Error`, preserving an underlying cause when one exists. Reuse the repo's error conventions; do not add a crate for convenience. Type-erased errors can fit an application boundary, but callers must not parse display strings to decide what to do.
6. **Panics need an invariant.** Invalid external input and expected operational failures return errors rather than panic. An `unwrap` or `expect` needs an invariant visible in the code or a test's failure condition. An `expect` message explains why success should hold. Do not replace an error path with a panic to satisfy a type.
7. **Ownership follows use.** Borrow for temporary access and take ownership to retain or consume a value. Prefer `&str` and `&[T]` when no owned-container operation is needed. Do not add `'static` bounds, leaks or shared ownership just to silence a borrow error; identify who owns the value and how long it is needed.
8. **Copies have a purpose.** A clone may isolate state or transfer an owned snapshot to a task. Check its semantics and cost at the call site rather than banning it. Avoid cloning an input the caller could move. Do not spread lifetimes or reference counting through an API to avoid an allocation without a demonstrated need.
9. **Abstractions serve a caller.** Use concrete types until polymorphism is needed. Choose generics for static dispatch and trait objects for the required dynamic dispatch or heterogeneity. `&dyn Trait` does not itself require a heap allocation. Do not introduce a trait, box or macro merely to follow a pattern from another language.
10. **Traits preserve meaning.** Implement only traits with a valid contract for the type. Keep equality, ordering and hashing consistent; derived implementations are appropriate only when field semantics match that contract. `Default` must produce a valid, meaningful value. Do not use `Deref` to simulate inheritance or expose mutable access that defeats validation.
11. **Public contracts stay explicit.** Keep implementation details private and expose the narrowest useful visibility. Preserve callers' supported use of public types, variants and trait bounds. Document examples, errors, panics and unsafe preconditions where they apply. Use doctests for executable examples, not `ignore` to hide a broken contract.
12. **Unsafe needs a proof.** Prefer a safe API when it meets the requirement. Keep necessary unsafe operations behind a small audited interface. Document the caller's obligations in `# Safety` and justify each unsafe operation's memory, aliasing, lifetime or synchronization assumptions locally. Safe callers must not be able to violate them. An unsafe `Send` or `Sync` implementation needs the same proof; a comment or a passing test alone is not one.
13. **Async APIs name their constraints.** Follow the repo's runtime. Decide whether a trait's callers require dynamic dispatch, a `Send` future or borrowed inputs before choosing its async signature. Native `async fn` in a trait does not by itself supply a dispatchable method on `dyn Trait` or a `Send` guarantee for its returned future. Do not add boxing or a macro without that need.
14. **Locks have a bounded scope.** Keep critical sections short and release synchronous lock guards before `.await`. A synchronous mutex can suit a short non-awaiting section in async code; an async mutex is not automatically better. Do not block an executor thread with blocking I/O or long CPU work; use the runtime's existing mechanism when needed.
15. **Cancellation preserves the contract.** At a timeout, `select!` or task cancellation, identify which effects already happened and what state remains if the future is dropped. Check the called API's cancellation guarantees. Make retry and partial progress explicit. Own spawned tasks through completion or deliberate shutdown; dropping a handle does not universally cancel the task.
16. **Test the promised invariant.** Alongside the shared test rules, cover valid boundaries, rejected values, error variants and state transitions that the changed contract distinguishes. For affected concurrency code, exercise relevant cancellation and shutdown paths. Use the repo's existing property, compile-fail or interpreter checks where they prove a promise; do not add a testing framework by default.
17. **Compatibility is part of correctness.** Use APIs available on the declared minimum supported Rust version and supported targets. Follow the repo's feature matrix, including default-disabled or `no_std` configurations when supported. Features should compose additively; testing only `--all-features` cannot prove every supported configuration. Do not silently raise the minimum version or require a new runtime.
18. **Tooling follows the repository.** Run the existing format, lint, test and doctest commands that cover the change. Keep the configured lint policy; do not enable all of Clippy's `restriction` group or suppress diagnostics broadly. A narrow allowance needs a reason tied to the code. Record a required configuration that could not be verified rather than declaring it supported.

## Self-checks

- Can any safe constructor, setter or deserializer create a value the domain forbids?
- Does each borrow, move and clone match who owns the data after the call?
- Can a caller distinguish failures without parsing an error message?
- What remains if this future is dropped after its first side effect?
- Which supported feature, target and compiler configurations did the change actually exercise?
- Does each review finding name a broken contract or numbered rule, rather than a personal preference?

Examples: [`PATTERNS.md`](PATTERNS.md). Research provenance and the limits of these recommendations: [`SOURCES.md`](SOURCES.md).
