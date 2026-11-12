---
name: codebase-design
description: Deep-module vocabulary, principles and the architecture rules of the rulebook. Use when designing an interface, placing a seam, writing a TASK or a DoD, setting a sketch, or writing or judging code.
---

# Codebase Design

Design **deep modules**: a lot of behaviour behind a small interface, placed at a clean seam, testable through that interface. Use this language and these principles wherever code is being designed or restructured. The aim is leverage for callers, locality for maintainers, and testability for everyone.

## Glossary

Use these terms exactly: don't substitute "component," "service," "API," or "boundary" for them. Consistent language is the whole point.

**Module**: anything with an interface and an implementation. Deliberately scale-agnostic: a function, class, package, or tier-spanning slice. _Avoid_: unit, component, service.

**Interface**: everything a caller must know to use the module correctly: the type signature, but also invariants, ordering constraints, error modes, required configuration, and performance characteristics. _Avoid_: API, signature (too narrow, they refer only to the type-level surface).

**Implementation**: what's inside a module, its body of code. Distinct from **Adapter**: a thing can be a small adapter with a large implementation (a Postgres repo) or a large adapter with a small implementation (an in-memory fake). Reach for "adapter" when the seam is the topic; "implementation" otherwise.

**Depth**: leverage at the interface. The amount of behaviour a caller (or test) can exercise per unit of interface they have to learn. A module is **deep** when a large amount of behaviour sits behind a small interface, **shallow** when the interface is nearly as complex as the implementation.

**Seam**: the junction between two modules where a contract passes, the *location* at which a module's interface lives (after Michael Feathers). Where to put the seam is its own design decision, distinct from what goes behind it. In Cairn, the sketch sets the seams of a plan. *Avoid*: boundary (overloaded with DDD's bounded context). The language skills' boundary, where data from outside the system enters it, is another notion and keeps its name.

**Adapter**: a concrete thing that satisfies an interface at a seam. Describes *role* (what slot it fills), not substance (what's inside).

**Leverage**: what callers get from depth. More capability per unit of interface they learn. One implementation pays back across N call sites and M tests. *Not to be confused with* a Cairn lever, a rearrangement that removes TASKs (VOCABULARY.md).

**Locality**: what maintainers get from depth. Change, bugs, knowledge, and verification concentrate in one place rather than spreading across callers. Fix once, fixed everywhere.

## Deep vs shallow

**Deep module** = small interface + lots of implementation:

```
┌─────────────────────┐
│   Small Interface   │  ← Few methods, simple params
├─────────────────────┤
│                     │
│  Deep Implementation│  ← Complex logic hidden
│                     │
└─────────────────────┘
```

**Shallow module** = large interface + little implementation (avoid):

```
┌─────────────────────────────────┐
│       Large Interface           │  ← Many methods, complex params
├─────────────────────────────────┤
│  Thin Implementation            │  ← Just passes through
└─────────────────────────────────┘
```

When designing an interface, ask:

- Can I reduce the number of methods?
- Can I simplify the parameters?
- Can I hide more complexity inside?

## Principles

- **Depth is a property of the interface, not the implementation.** A deep module can be internally composed of small parts; they just aren't part of the interface. A module can have **internal seams** (private to its implementation) as well as the **external seam** at its interface. Its tests cross the external seam: a test reaches an internal seam only to put a local stand-in in place, as [DEPENDENCIES.md](DEPENDENCIES.md) says, never to mock a collaborator.
- **The deletion test.** Imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across N callers, it was earning its keep.
- **The interface is the test surface.** Callers and tests cross the same seam. If you want to test *past* the interface, the module is probably the wrong shape.
- **One adapter means a hypothetical seam. Two adapters means a real one.** Don't introduce a seam unless something actually varies across it.

## Designing for testability

Good interfaces make testing natural:

1. **Accept dependencies, don't create them.**

   ```typescript
   // Testable
   function processOrder({ order, paymentGateway }) {}

   // Hard to test
   function processOrder({ order }) {
     const gateway = new StripeGateway();
   }
   ```

2. **Return results, don't produce side effects.**

   ```typescript
   // Testable
   function calculateDiscount(cart): Discount {}

   // Hard to test
   function applyDiscount(cart): void {
     cart.total -= discount;
   }
   ```

3. **Small surface area.** Fewer methods = fewer tests needed. Fewer params = simpler test setup.

## Relationships

- A **Module** has exactly one **Interface** (the surface it presents to callers and tests).
- **Depth** is a property of a **Module**, measured against its **Interface**.
- A **Seam** is where a **Module**'s **Interface** lives.
- An **Adapter** sits at a **Seam** and satisfies the **Interface**.
- **Depth** produces **Leverage** for callers and **Locality** for maintainers.

## Rejected framings

- **Depth as ratio of implementation-lines to interface-lines** (Ousterhout): rewards padding the implementation. We use depth-as-leverage instead.
- **"Interface" as the TypeScript `interface` keyword or a class's public methods**: too narrow: interface here includes every fact a caller must know.
- **"Boundary" for a seam**: overloaded with DDD's bounded context. Say **seam** or **interface**.

## The architecture rules

These rules are part of the rulebook that the coder and the judge load identically: the coder writes to them, and the judge cites them by number. Like test-discipline's shape rule, they hold on the lines a diff writes. The sketch in force is the planner's, and the code a diff leaves untouched stays as it is. The architect and the planner design to them, and the architecture critique looks across work-packages for what they let through. Rules 1 and 3 hold in full on the modules the epic adds, those whose files are absent from `origin/main`. In a module that exists on `origin/main`, the lines a diff writes follow the module's current form and the exemplar module, even where that form breaks rule 1 or 3: bringing such a module to these rules is a lever, which the architect designs as a TASK, never a finding against the coder. Rules 2, 4 and 5 judge only what the diff adds, and hold everywhere.

1. **Dependencies point inward.** A module far from input and output never imports one near it: a module that holds a concept of `CONTEXT.md` imports no adapter, framework, transport or storage module. A library of pure types and functions, such as a schema library, is none of these. A port belongs to the module that uses it, and the adapter conforms to it.
2. **No import cycle.** An import the diff adds closes no cycle between modules.
3. **A rule lives with its concept.** A business rule on a concept of `CONTEXT.md` is decided in the module that holds that concept, never in an adapter, a handler, a view or a query.
4. **One concept, one name.** The diff adds no module, type or name for a concept that `CONTEXT.md` or the code already carries, and names no concept by a word that an `_Avoid_` line of `CONTEXT.md` lists. A name that comes from a third-party interface is not the diff's choice.
5. **No hypothetical seam.** The diff adds no port with a single adapter, counting the adapters its tests use, and no module that the deletion test shows to be a pass-through.

## Writing a TASK

Phrase a TASK in this vocabulary: the module it changes, the interface it adds or alters, the seam it uses, the adapter it needs. Never name a file path.

## Dependencies and seams

How a dependency decides what runs for real in a test, and when a seam is real: [DEPENDENCIES.md](DEPENDENCIES.md). A DoD uses these categories to say what is real and what is simulated in its test.

## Two candidates

When the architect sees a lever, it designs the two forms, integrated and with the lever, alone and in one pass. Compare them on facts: the TASKs spared, the impact, the size of the interfaces, the time before the first observable plan, and reversibility. Record the choice as a DEC, with the reason the other form was rejected.

Deepening is not an activity of its own: it exists only as a bounded lever inside an epic, as a closing fix the pilot asks for at the epic's closing review, or as a draft that the architecture critique proposes. Unless the pilot fixes it in the epic at the closing review, a rearrangement that spares no TASK of the epic stays out of `design.md`: the architect names it in its `designed` entry, and the session takes it to the closing review beside the architecture critique's proposals.
