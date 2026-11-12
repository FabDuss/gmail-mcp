# Rust patterns

Original examples for the numbered rules in `SKILL.md`. They use the standard library. Follow the repository's error and runtime conventions when applying them.

## A validated type, not just a validating constructor

Rules 1, 2, 5 and 16. A percentage here is an integer from zero through one hundred, inclusive. The field stays private in its module, and every value exposed to callers passes the constructor.

```rust
mod percentage {
    use std::convert::TryFrom;
    use std::fmt;

    #[derive(Debug, Clone, Copy, PartialEq, Eq)]
    pub struct Percentage(u8);

    #[derive(Debug, Clone, Copy, PartialEq, Eq)]
    pub struct InvalidPercentage {
        value: u8,
    }

    impl fmt::Display for InvalidPercentage {
        fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
            write!(f, "percentage {} exceeds 100", self.value)
        }
    }

    impl std::error::Error for InvalidPercentage {}

    impl TryFrom<u8> for Percentage {
        type Error = InvalidPercentage;

        fn try_from(value: u8) -> Result<Self, Self::Error> {
            if value > 100 {
                return Err(InvalidPercentage { value });
            }
            Ok(Self(value))
        }
    }

    impl Percentage {
        pub fn get(self) -> u8 {
            self.0
        }
    }
}

use percentage::Percentage;
use std::convert::TryFrom;

assert_eq!(Percentage::try_from(0).unwrap().get(), 0);
assert_eq!(Percentage::try_from(100).unwrap().get(), 100);
assert!(Percentage::try_from(101).is_err());
assert!(Percentage::try_from(u8::MAX).is_err());
```

The `unwrap` calls assert success for known valid test inputs. They are not a parsing policy for external input. Privacy protects callers outside the defining module; code inside that module still has to preserve the invariant.

A derived deserializer that writes the private field directly could bypass the check. Route deserialization through the checked conversion, using the repo's serialization facilities. A setter or a mutable dereference to the inner `u8` would need the same scrutiny.

## Fallible conversion preserves an error

Rules 3 and 4. The input may not fit the target integer type, and that is an error rather than permission to truncate.

```rust
use std::convert::TryFrom;

fn narrow_count(input: u32) -> Result<u16, std::num::TryFromIntError> {
    u16::try_from(input)
}

assert_eq!(narrow_count(65_535).unwrap(), 65_535);
assert!(narrow_count(65_536).is_err());
```

This proves representability, not whether the count is valid for the business operation. That separate bound belongs in its domain constructor. Similarly, `Duration` represents a non-negative duration, but it does not prove that adding it to a timestamp will succeed.

## Move what the receiver retains

Rules 7 and 8. The receiver stores the label, so it accepts ownership. The caller chooses whether to transfer a string or create a copy.

```rust
#[derive(Debug)]
struct Job {
    label: String,
}

impl Job {
    fn new(label: String) -> Self {
        Self { label }
    }

    fn label(&self) -> &str {
        &self.label
    }
}

let label = String::from("rebuild");
let job = Job::new(label);
assert_eq!(job.label(), "rebuild");

let retained = String::from("retry");
let second = Job::new(retained.clone());
assert_eq!(retained, second.label());
```

The first call needs no copy. The second deliberately retains an independent string. Neither taking every input by reference nor cloning every input expresses both cases well.

## Dynamic dispatch does not imply a box

Rule 9. These signatures make different dispatch choices without requiring ownership of the policy.

```rust
trait Policy {
    fn allows(&self, size: usize) -> bool;
}

struct Limit(usize);

impl Policy for Limit {
    fn allows(&self, size: usize) -> bool {
        size <= self.0
    }
}

fn static_check(policy: &impl Policy, size: usize) -> bool {
    policy.allows(size)
}

fn dynamic_check(policy: &dyn Policy, size: usize) -> bool {
    policy.allows(size)
}

let limit = Limit(8);
assert!(static_check(&limit, 8));
assert!(!dynamic_check(&limit, 9));
```

Use a trait only if the caller needs substitutable policies. The wrappers above contrast signatures; they are not extra layers to add to production code. A caller that only uses `Limit` can call it directly.

## Locks and cancellation

Rules 13 through 15. In async code, put a short synchronous lock scope before the await and pass an owned result out of that scope. This can make a clone intentional. If the operation must hold the lock while awaiting, use a suitable async primitive and examine contention and shutdown instead of mechanically replacing every mutex.

For Tokio, cancelling `read_exact` or `write_all` can leave partial progress. Recreating the operation is not automatically a safe retry. The `select!` documentation distinguishes these from operations with cancellation guarantees. Consult the version used by the repository, not an assumption about all futures.

Dropping a Tokio `JoinHandle` detaches its task. If a parent must stop its children, give those tasks an explicit cancellation and completion policy. Synchronous cleanup in `Drop` is not a substitute for an async shutdown protocol.

## Verify the supported configurations

Rules 11, 17 and 18. Read the existing CI commands before choosing a check. A crate may support the minimum compiler with default features, a newer toolchain for development, and a separate default-disabled build.

- `rust-version` declares a minimum compiler; an actual build on that compiler tests the claim.
- A default build, a default-disabled build and an all-features build exercise different code.
- Doctests check public examples; a targeted unit test run may not run them.
- A compile-fail test is useful for a promised type restriction, but a compilation failure alone does not prove that the intended restriction caused it.
- Existing Miri or property tests can add evidence for unsafe invariants and trait contracts. Passing them does not establish soundness for every caller.

Use these distinctions to select the repository's existing commands, not to invent a universal Cargo command or introduce new tools.
