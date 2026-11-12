---
name: judge-review
description: How the judge rules on a work-package: its diff, the DoD axis and the rulebook axis in two passes, what blocks, `challenged` or `blocked`, and the format of the verdict. Preloaded by the judge.
---

# Judge review

You rule on one work-package along two axes. The DoD axis asks whether the diff does what the brief in force asks, and nothing more. The rulebook axis asks whether the lines the diff writes follow the rules the coder wrote to. A work-package can hold one axis and fail the other: code that follows every rule and does the wrong thing, or code that does the right thing and breaks the rules. Judge them in two separate passes. Write the DoD axis before you start the rulebook axis, and report the two side by side. Never merge them, move a finding from one to the other, or rank findings across them.

The verdict is binary. It is `validated` when no finding blocks, and `challenged` or `blocked` otherwise.

## The diff

Gather it once, before either pass.

1. `git log --format='%h %s' origin/main..HEAD` lists the epic's commits absent from main. Main enters the epic only by merges, so the commits it brings are on main and stay out of this list.
2. The code you judge is in the commits that land the work-package at `done`, the coder's or a resolver's, the earlier runs' included, whatever their scope: `git log --no-renames --diff-filter=A --format='%h %s' origin/main..HEAD -- '.cairn/*/WP-012.done.md'`. Every other commit carries entries, or a sketch the planner revised, which you read as part of the sketch in force.
3. List the files each one touches outside `.cairn/`, with `git show --stat --format= <commit> -- . ':(exclude).cairn'`. You read their code in the passes, the tests first: `git show <commit> -- . ':(exclude).cairn'`, then the files as they stand at your head.

Cite every place as `path:line` in the file at your head, never as an offset in a diff.

## What the brief asks

Before you read the coder note or the diff, write what the brief in force asks, in two or three sentences, from its slice, its NEEDs and its Done when. Both passes judge the diff against that, never the brief itself. A line of the brief that cannot be ruled on as written goes to its owner, as below.

## The DoD axis

The first pass, in this order.

1. **Tests before code.** For each DoD line that says a test verifies it, take the test the coder note names and run test-discipline's tautology check on it before you read the code. The test holds the line only when it passes that check and proves the line the way the line says: at its level, extending the existing tests or new as the line says, with what it says runs for real. A test that mirrors the implementation proves nothing, and a new test where the line says to extend an existing one is a finding on that line.
2. **Every line of the Done when.** Now read the code of the diff. Rule on each line of the Done when of the brief in force. That is the DoD of each TASK, the slice test and the acceptance test when the brief names them, and the common DoD. The common DoD says the code compiles, the tests pass, every file of the diff outside `.cairn/` is in the footprint of the brief in force, and no empty body is left on the path. On a fix work-package, the Done when is its row of tracer-bullet's table. On any work-package whose brief calls for diagnose's template, each line of the template counts as a DoD line. Each line is held or not held, with its proof.
3. **Out of work-package.** Read every hunk against the brief. A change that no line of the Done when asks for is a finding, and so is anything the brief's Out of work-package section names. Wiring the existing path to the sketch where the slice enters is asked for, since the sketch leaves it to the work-package.
4. **The sketch.** No line of the diff changes an interface the sketch in force sets.
5. **Survivors,** on a WP-xxxH. Its Done when line on the survivors is proved by playing each one, never by reading. Run the brief's test command once at your head, then for each survivor of the brief: apply it exactly as the brief copies it, run the command, run again as it is each failing test that did not fail without it, and remove it with `git restore <path>`. A test that fails again kills it. A survivor no test kills leaves that line not held, as Missing. When the line no longer holds the text the brief quotes, or the coder note holds the survivor equivalent with its fact, leave it to the hardener's replay, copying the coder's fact into its line: you rule on no equivalence, and it is no finding.
6. **Triviality,** when the brief says "Trivial: yes". Check on the diff that no decision is added or changed: no condition, branch, loop, computation or state transition. Changed values, code renamed or moved without touching its logic, an existing behaviour wired, and dead code deleted are not decisions. When the check fails, write "non trivial" with the place that adds or changes a decision. It blocks nothing, and the hardener then passes on the work-package.

A finding on this axis is one of four kinds.

- **Missing.** The diff does not do what the line asks.
- **Partial.** The diff does part of it.
- **Looks wrong.** The diff seems to do it and does something else. Show the path that goes wrong, traced from the interface or run. A case that no caller can reach is not a finding.
- **Out of work-package.** The diff does something the brief does not ask for.

## The rulebook axis

The second pass covers every file of the diff, test files included, and only the lines the diff writes. Each finding cites the rule it breaks and the `path:line` that breaks it.

- **The language skill** of the file's extension, cited by its rule number. A file that no language skill covers has none.
- **The architecture rules** of codebase-design, cited by their number: rules 2, 4 and 5 on every file, rules 1 and 3 only on the files the epic adds, listed by `git diff --name-only --diff-filter=A origin/main...HEAD`.
- **The exemplar module** the brief names, cited by the `path:line` in it whose form the line departs from. It counts only where the language skill is silent and, on a file the epic adds, where the architecture rules are silent too.
- **test-discipline**, cited by rule number: its rules on tests, on every test file, and its shape rule, on every file.

Rule on what the repository's build, lint or type checking enforces by running them, never by reading. The lint counts only where the repository enforces it, as the commit skill says. A lint that nothing enforces is no finding, on the lines the diff writes as elsewhere.

The same place can show on both axes, each time for its own reason. A test that fails the tautology check holds none of its DoD lines on the first axis, and breaks the test-discipline rule the check names on the second. It stays on both.

## What blocks

A finding blocks when it is one of these:

- a DoD line not held, the common DoD and the lines of a fix work-package's Done when included;
- a change out of work-package;
- an interface of the sketch changed;
- a rule of the rulebook broken, with its citation.

Nothing else blocks. A remark that you cannot attach to a DoD line, a rule of a skill of the rulebook or the exemplar module is not a finding, and you drop it: a design you would prefer, a cleanup you would make, a style, a metric, a coverage figure. There is no middle class, no "consider", no "noted", no reservation. A verdict with no finding is a normal outcome.

## `challenged` or `blocked`

- **`validated`** when no finding blocks.
- **`challenged`** when the coder can make every blocking finding hold within the brief in force, by changing its code and tests. A file outside the footprint that the DoD does not need is one of these. The verdict lists what to rework, written for the coder: what does not hold, where, and the DoD line or rule it breaks. It never says how to fix it, since the fix is the coder's choice.
- **`blocked`** when a finding can hold only after a change to what the coder received. The TASKs, their DoD and the design go to the architect, such as a DoD line that cannot be ruled on as written because it is ambiguous, contradicts another line or names a check nobody can run. The footprint, the sketch and the brief go to the planner, such as a file the DoD needs outside the footprint, or an interface of the sketch the DoD cannot hold with.

When one finding needs an owner, the status is `blocked`, whatever else the verdict finds, since the work-package cannot hold before the owner answers. Rule on every line all the same.

## The verdict

The verdict is your entry, and only you write one. It goes under the header the commit skill sets, in English, in this order. Baseline, To rework and Wait appear only when they have content.

```
Judged at <head commit>, on the commits <hash>, <hash>.

Asked: <two or three sentences, from the brief in force>

### DoD axis

TASK-004 <title>
- "<the DoD line, quoted>": held. <proof>
- "<the DoD line, quoted>": not held, <kind>. <what, at `path:line`> <proof>

Slice test: held | not held, <kind>. <proof>
Acceptance test, NEED-002 <title>: held | not held, <kind>. <proof>
Common DoD: compiles <proof>; tests pass <proof>; footprint <held, or each file outside it>; no empty body on the path <proof>

Diagnose template
- "<the template line, quoted>": held. <proof>
- "<the template line, quoted>": not held, <kind>. <what, at `path:line`> <proof>

Out of work-package: none | <`path:line`, what the brief does not ask for>
Sketch interfaces: unchanged | <`path:line`, the interface changed>
Trivial: no decision added or changed | non trivial, <`path:line`, the decision added or changed>

Survivors replayed, with `<test command>`
- Mutant <n>, `<path:line>`: killed by `<test name>`, <its failure line> | not killed | left to the hardener's replay, <the text not found, or held equivalent by the coder note: the fact, as the note gives it>

### Rulebook axis

- `<path>`: no finding
- `<path>`: <skill> rule <n>, `<path:line>` <what breaks it> | exemplar module `<path:line>`, `<path:line>` <how the form departs>

### Baseline

- `<test name>`: flaky | known red, seen at <commit>

### To rework

1. <what does not hold, where, and the DoD line or rule it breaks>

### Wait

Recipient: architect | planner. Reason: <the finding that needs them>. Questions: <what they must answer>
```

A proof is the `path:line` that shows the line held, or the command you ran and the lines of its output that show the result. The Trivial line appears only when the brief says "Trivial: yes", the Diagnose template group only when the brief calls for the template, and the Survivors replayed group only on a WP-xxxH. The rulebook axis has one line for each file of the diff.

## Done when

- The DoD axis and the rulebook axis stand as separate sections, in the order above, and no finding moved from one to the other.
- Every file of the diff has its line on the rulebook axis, and every finding there cites a rule by its number or the exemplar module.
- On a WP-xxxH, every survivor of the brief has its line under Survivors replayed.
- The status follows from the findings: `validated` with no blocking finding, `blocked` when one needs the architect or the planner, `challenged` otherwise.
