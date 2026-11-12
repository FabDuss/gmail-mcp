---
name: test-discipline
description: How a test proves a behaviour, from the DoD line to the assertion, and the shape rule for code. Preloaded by the architect, the coder, the judge and the hardener.
---

# Test discipline

A test proves a behaviour. It calls the code the way its callers do, and compares what they observe with a value that comes from outside the code. These rules say what such a test is, where it goes, what runs for real in it, how the coder writes it and how the judge checks it. They end with the shape rule, which every line of code follows.

They are part of the rulebook that the coder and the judge load identically. The coder writes to them, and the judge cites them by number. The architect reads them to write how each DoD line is verified, in the section "A DoD line". The hardener reads them to say what a test should observe, in the section "The hardener". The words module, interface, seam and adapter mean what codebase-design says.

## A good test

1. **Behaviour through the interface.** A test calls the module the way its callers do and checks what they observe, such as the value it returns or a state read back through the interface. It survives a rewrite of the implementation, and it reads like the scenario it proves.
2. **An expected value from outside the code.** The expected value is a literal taken from the scenario, a worked example or the DoD line. The test never computes it the way the code does.
3. **One behaviour per test**, checked by one logical assertion.
4. **Named by its behaviour.** A test's name says what it proves, in the words of its scenario, which are the terms of the intent's concept table and of `CONTEXT.md`. Its name, its file and its comments carry no Cairn identifier, such as a NEED, TASK, WP, EPIC, DEC, FACT or Q identifier. The coder note links each DoD line to its test.

Three anti-patterns, which the judge cites by number:

5. **Implementation-coupled.** The test mocks an internal collaborator, tests a private function, asserts call counts or call order, or verifies through a side channel, such as a database query in place of the interface. It breaks when the implementation is rearranged and the behaviour has not moved.
6. **Tautological.** The expected value comes from the code under test, or the test recomputes it the way the code does. The test then passes by construction and can never disagree with the code. The tautology check below finds it.
7. **Horizontal slicing.** All the tests first, then all the code. Such tests check an imagined behaviour and the shape of things, and they fix a test structure before the code has taught anything. Rule 15 replaces it.

```ts
// Implementation-coupled: bypasses the interface to verify
test("createUser saves to database", async () => {
  await createUser({ name: "Alice" });
  const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
  expect(row).toBeDefined();
});

// Through the interface
test("a created user can be retrieved", async () => {
  const user = await createUser({ name: "Alice" });
  expect((await getUser(user.id)).name).toBe("Alice");
});

// Tautological: recomputes the expected value the way the code does
test("the total sums the line items", () => {
  const items = [{ price: 10 }, { price: 5 }];
  expect(calculateTotal(items)).toBe(items.reduce((sum, i) => sum + i.price, 0));
});

// A literal expected value
test("the total sums the line items", () => {
  expect(calculateTotal([{ price: 10 }, { price: 5 }])).toBe(15);
});
```

## Where tests go

8. **At the seams.** Tests go at the seams the sketch sets and at the interfaces of the existing modules the DoD names, never against the internals of a module, except an internal seam reached only to put a local stand-in in place (rule 12). The seams are the sketch. A test that needs a seam the sketch lacks is a wait, addressed as your definition says, never a test placed lower or past the interface.
9. **Start from the existing tests.** Before writing a test, find the tests that already cover the behaviour you touch. An improved behaviour extends its tests and gets no new one. A new test file exists only for a new behaviour. An existing test file outside your footprint is a footprint in question, never a reason to open a new file.
10. **Levels, as the repository names them.** A level is a kind of test that the repository's test tooling keeps apart, by configuration, script or folder, such as unit, integration or end-to-end. Read the levels and their names in that tooling, and spell them as it does. A test lives where the tooling puts its level.
11. **Few dynamic dependencies.** The gates run the tests that the repository's tooling relates to a diff, and that selection does not see a dependency resolved at run time: injection by a container, a module chosen by configuration, a module loaded by name. A test avoids them as far as it can. A dependency passed as an argument, from a module the test imports, stays visible. Keep the dynamic ones you cannot avoid to the strict minimum.

## What runs for real

12. **Simulate only what you cannot run.** The category of each dependency, from codebase-design's [`DEPENDENCIES.md`](../codebase-design/DEPENDENCIES.md), says what a test runs.

    | Dependency | In a test |
    | --- | --- |
    | In-process | the real code, through the interface |
    | Local-substitutable | the local stand-in, running in the suite, such as a test database or an in-memory filesystem |
    | Remote but owned | the in-memory adapter of its port, the only place where a test simulates a service of your own |
    | True external | a mock adapter at its port |

    Your own modules and the internal collaborators of a module run for real. The clock and randomness reach the module through its interface, and the test passes fixed values. A port to a true external service offers one function per operation, never one generic fetcher, so that each mock returns one shape.
13. **Real tests.** Prefer the test primitives of the framework the code runs on, with their checks for leaked resources, over a double of that framework. Check a user interface on a running build.

## The loop

For the coder, on each DoD line that the brief proves by a test.

14. **Red first.** Write the test before the code, against the sketch. It would fail on a plausible but wrong implementation, and it fails first on the empty body, whose error names it. Watch it fail for the reason its DoD line names. A red from a compile error, an import or a misspelled name proves nothing, so fix the test until it fails on its assertion or on the empty body. An assertion you add to an existing test fails on the code as it stands. A test that passes before your code observes nothing your code adds. Rewrite it, or say in your note that the behaviour already held.
15. **One cycle at a time.** One seam, one test, then the minimal code that turns it green, then the next test. What each cycle teaches shapes the next test.
16. **Minimal green.** Write only the code the red test asks for, and nothing a later test or a later work-package might want.
17. **Refactoring stays out of the loop.** A rearrangement beyond the lines the DoD needs is a lever, which the architect designs, or a proposal of the closing review, which the pilot makes a draft epic or a closing fix. Leave it, and name it in your note.
18. **Nothing to test, nothing to write.** A guard, a security default or a cosmetic change gets no test before the code. The hardener mutates the guard and says whether a test is missing. Name the line in your note.
19. **Slice and acceptance.** A work-package that adds a behaviour writes its slice test, which enters where the brief's slice enters and checks what one sees at its end. The work-package that closes a NEED also writes the NEED's acceptance test, which plays its scenario, or checks its threshold for a constraint NEED, and passes before the run ends. For a lever, a trivial work-package or a wide change, the existing suite, which stays green, and the judge's reading stand in for these tests. The acceptance test stays in the suite. Acceptance itself comes from the barrier's observation, which never reuses it.

## Guardrails

20. **The assertion holds.** A test written from a DoD line keeps its assertion as written, whatever the code does. When the test seems wrong, the DoD line is in question. Stop on that line, and put it in question in a wait on the design. An existing assertion changes only when a TASK of the brief changes the behaviour it expects, and never to match an implementation.
21. **No test rather than a bad test.** A bad test checks mostly mocks, mirrors the implementation, depends on timing or on unrelated global state, or would be deleted once it had served. A DoD line that only a bad test could prove is in question, as in rule 20. Beyond the DoD, write no test rather than a bad one.
22. **Evidence, not outcomes.** For each test written red first, the coder note names the failure it showed before the code and the run where it passed. When the red could not be shown, the note says why.

## The tautology check

The judge runs it on every test of the diff, before reading the code. The coder runs it on every test before keeping it.

23. **The source of the expected value.** A literal, a worked example or the scenario holds. A value taken from the code under test, or recomputed the way the code computes it, makes the test tautological.
24. **The discrimination check.** Name a plausible wrong behaviour for the DoD line and the assertion that would fail for it. Returning `undefined` or doing nothing is one probe, not a universal verdict: `toBeDefined` and `toBeTruthy` fail on `undefined`, while an absence may be the expected result. Failing this probe does not establish the whole DoD line either. These five shapes call for inspection, not rejection by matcher name:
    - **Weak or no assertion.** No assertion, or only a broad property such as `toBeDefined`, `toBeTruthy`, `not.toThrow`, `toBeInstanceOf` or `toBeGreaterThan(0)`. Check whether the DoD asks for that property or for a more specific result that could still be wrong.
    - **Mock or absence only.** A call count says nothing about its payload. An empty result or a missing call may be required; check that an unwanted result or effect would fail the test, and that the suite also detects an implementation that always returns empty or does nothing where the contract requires otherwise.
    - **Self-referential.** An expected value obtained from the same computation cannot independently establish its correctness, as in `expect(f(a)).toBe(f(a))`. Trace helpers used for the expected value by rule 23.
    - **Constant pin.** An exact constant can be a requirement. Comparing it with an independently specified value checks that value, not whether the mechanism uses it. If the DoD claims the mechanism's behaviour, exercise the mechanism.
    - **Fixture asserts fixture.** The assertion only confirms data prepared by the test, without any result or effect of the subject. A subject called during setup can still produce a result worth asserting; its call location alone decides nothing.
25. **The fix.** Exercise the subject through its interface with a concrete input, and assert the expected result or observable effect from the scenario, as in `expect(slugify("Hello, World!")).toBe("hello-world")`. Strengthen only what the DoD requires. For an absence, cover the contrasting input where the contract requires presence, in this test or another. For a constant used by a mechanism, exercise that mechanism. For an external mock, check the required payload or observable effect, not just the call count. If the line cannot be proved this way, put it in question by rule 20 rather than inventing an expected value.
26. **What passes anyway.** A test of a relation across a table's rows, such as a key present in two tables or a parent that exists, and a compile-time check in a type-test file such as `*.test-d.ts`.

A test holds a DoD line only for the behaviour its assertions distinguish from a relevant defect. A weak assertion is not necessarily tautological, and it does not establish a stronger requirement.

## The shape rule

The readability rule for code, tests included, that the coder and the judge share. It holds on the lines a diff writes.

27. **One pass.** The control flow reads in one pass, from top to bottom.
28. **No extraction for one caller.** A function with a single caller stays inline in it, unless its name states a domain rule or a rule of the language skill or of the architecture rules sets it apart, such as lang-typescript's pure core. A wrapper with one caller folds into that caller. Count the callers across the repository.
29. **No split for a metric.** Split nothing to lower a metric.
30. **Count the layers.** Between a call and the code that answers it, a reader crosses no layer that only forwards the call.
31. **The code justifies itself.** No Cairn identifier, such as a NEED, TASK, WP, EPIC, DEC, FACT or Q identifier, appears in the code, its names or its comments. The link runs from the governance to the code: the coder note says which test proves which DoD line.

## A DoD line

For the architect. Each line of a DoD says how it is verified.

- **By a test.** The line names the level, as the repository names it (rule 10). It says whether the test extends the existing tests of the behaviour or is new because the behaviour is new (rule 9). It says what runs for real and what is simulated, by the category of each dependency (rule 12). It names behaviours and modules, never a test file.
- **By reading**, when there is nothing to test (rule 18).

## The hardener

You never read the tests. For each mutant of your list, write what a test should observe in the terms of rules 1 and 2: a behaviour at an interface, with its literal expected value, at a level the repository names.

## Done when

- **Architect.** Every DoD line says how it is verified: by a test, with its level, extended or new, and what is real or simulated, or by reading.
- **Coder.** Every DoD line that a test proves has its test, which failed first for the reason its line names and passes now. Every test you wrote passes the tautology check and is named by its behaviour, and the shape rule holds on every line of your diff.
- **Judge.** Every test of the diff has been through the tautology check, and every finding on a test or on the shape of the code cites its rule number.
- **Hardener.** Every mutant of your list says what a test should observe, in the terms above.
