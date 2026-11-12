# Sources and scope

The rules and examples are original Cairn synthesis, not a reproduction of these documents. Sources were checked on 2026-11-04. These are maintained documents, not a frozen edition of Rust. For an API used by a project, check its pinned version and minimum supported Rust version.

## Primary references

| Source | What it supports | Rules |
| --- | --- | --- |
| [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/checklist.html) and [flexibility](https://rust-lang.github.io/api-guidelines/flexibility.html) | Encapsulation, ownership, meaningful traits, errors and API design | 1, 2, 5, 7-11 |
| [Rust Book: to panic or not to panic](https://doc.rust-lang.org/book/ch09-03-to-panic-or-not-to-panic.html) | Recoverable failures, proven invariants and panics | 2, 6 |
| [TryFrom](https://doc.rust-lang.org/std/convert/trait.TryFrom.html), [Result](https://doc.rust-lang.org/std/result/index.html) and [Option](https://doc.rust-lang.org/std/option/index.html) | Fallible conversions, absence and failure information | 2-4 |
| [Duration](https://doc.rust-lang.org/std/time/struct.Duration.html) | A standard non-negative duration and its arithmetic limits | 1, 3 |
| [Rust Reference: dyn compatibility](https://doc.rust-lang.org/reference/items/traits.html#dyn-compatibility) | Trait objects and dispatchable methods | 9, 13 |
| [Cargo Book: Rust version](https://doc.rust-lang.org/cargo/reference/rust-version.html) | Minimum supported Rust version and its verification | 17 |
| [Cargo Book: features](https://doc.rust-lang.org/cargo/reference/features.html) | Additive features and feature combinations | 17 |
| [Clippy usage](https://doc.rust-lang.org/clippy/usage.html) | Lint configuration, allowances and the restriction group | 18 |
| [Rustonomicon: how safe and unsafe interact](https://doc.rust-lang.org/nomicon/safe-unsafe-meaning.html) | Sound safe interfaces and unsafe obligations | 12, 16 |
| [Tokio select: cancellation safety](https://docs.rs/tokio/latest/tokio/macro.select.html#cancellation-safety) | Partial progress and dropping futures | 15, 16 |
| [Tokio Mutex](https://docs.rs/tokio/latest/tokio/sync/struct.Mutex.html) and [JoinHandle](https://docs.rs/tokio/latest/tokio/task/struct.JoinHandle.html) | Lock scope and spawned-task lifetime | 14, 15 |
| [Rustdoc documentation tests](https://doc.rust-lang.org/rustdoc/write-documentation/documentation-tests.html) | Executable examples and compile-fail tests | 11, 16, 18 |

## What this synthesis does not claim

- Clean code has no universal function-length, trait-count or allocation threshold.
- `clone`, `unwrap`, `dyn` and `unsafe` are not equivalent mistakes. Review their contract and consequences.
- Tokio-specific guarantees do not describe every async runtime. This skill does not require Tokio.
- A newtype only carries an invariant if every construction and mutation path preserves it.
- A tool passing is evidence within its scope, not a proof of business correctness or soundness.
- These sources inform the rules. They do not add uncited requirements to the coder and judge's closed rulebook.
