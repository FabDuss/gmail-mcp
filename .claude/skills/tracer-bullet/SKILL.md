---
name: tracer-bullet
description: How the planner composes a plan: the sketch, the work-packages as tracer bullets, plan.md, the briefs, and the gap report when the design does not assemble. Preloaded by the planner.
---

# Tracer bullet

You compose a plan from the TASKs of `design.md`. Each work-package is a tracer bullet: a narrow but complete path through every layer the change crosses, ending in a state someone can check. A path built end to end gets feedback early; a layer built alone gets none until everything else exists.

## Read the design first, as a critic

Read `design.md` once, as the one who must assemble it. Everything that does not assemble goes into one gap report, in one go: a TASK that cannot be assembled, a DoD that cannot be verified, a NEED without a TASK, a lever without benefit, a sketch that contradicts the design. You then end on `gaps`, by the commit skill's Stopped path, and land no partial sketch.

## The rules of a work-package

1. **Whole TASKs.** A work-package is one or more whole TASKs. Never cut a TASK: one too big goes back to the architect in the gap report.
2. **One run.** A work-package fits in one run. If it cannot, its TASKs are too big: gap report.
3. **Vertical.** It enters somewhere, crosses what it must, touches its data, and ends with something observable. Never one layer alone.
4. **Verifiable on its own.** When it is validated, someone can check what it does without waiting for another work-package.
5. **Two exceptions.** A lever rearranges existing code without changing behaviour, and so does a closing fix that rearranges code. A wide change goes in three stages, by the expand-migrate-contract skill. Nothing else is horizontal.
6. **Levers first,** before what they simplify.
7. **The core first.** The opening work-package is the first tracer bullet through the core, launched first; the work-packages that depend on it wait for its validation, the others start alongside.
8. **No shared file between runs that can overlap.** Two work-packages that can run at the same time have no file in common. When two need the same file, one depends on the other in `plan.md`.
9. **Triviality follows the work-package, never the reverse.** A work-package is trivial when all its TASKs are mechanical, and a trivial one skips hardening. Never split a work-package to make part of it trivial: a mixed work-package is simply hardened, and mutation finds nothing to change in its mechanical part.
10. **An order that proves itself.** Each work-package ends in a verifiable state, and what a later one relies on is validated before it starts.

You are the only one to draw work-package numbers. A TASK whose work-package is at `validated` or beyond, `hardened`, `holes` or `bug`, is never rewritten: its revision becomes a new TASK, which cites it, and you put it in an ordinary work-package.

You open a fix work-package outside the composition of a plan, in one of three cases:

| Work-package | Opened when | Footprint | Goes through | Done when |
| --- | --- | --- | --- | --- |
| `WP-012H` | the hardener lands `holes` | the parent's | coder, then judge, who replays its survivors; `validated` closes it | each listed survivor is killed by a test named after the behaviour it proves, or held equivalent in the coder note with the fact that shows it, extending the existing tests of that behaviour; the acceptance tests stay green; no interface changes |
| `WP-012B` | the hardener lands `bug` | the parent's | the whole chain, hardener included | the violated DoD line holds, with the diagnose template, plus what `WP-012H` asks for the holes |
| no parent, numbered like any work-package | a gap the session sends back to the planner: a failing gate, a rejected observation, a change asked on the PR | what the fix needs | the whole chain | the requirement it takes up holds, with the diagnose template for a failing gate, and the common DoD |

One hardener pass opens at most one fix work-package of each kind, which handles all its survivors; the strongest survivor sets its kind. A replay that leaves survivors gets the brief of the existing fix work-package recomposed, which takes it back to `ready` whatever its status, since it carries survivors and no TASK: the WP-xxxB when there is one, since its Done when covers holes too, and otherwise the WP-xxxH. Only a replay at `bug` after a WP-xxxH alone opens the WP-xxxB.

## The sketch

You land it at the start of the plan, before any work-package leaves, by the commit skill's Finished path, so it passes the gates.

- Interfaces, types, empty bodies and shared files, and nothing `design.md` does not name.
- An empty body throws an error that names it, so the coder's first test fails first.
- The sketch wires no existing path to an empty body. The work-package does that wiring, so the suite stays green until a work-package lands.
- A work-package fills empty bodies and never changes an interface. A needed change is a wait addressed to you, since only you revise the sketch; a change that contradicts `design.md` goes to the architect. Your entry for a revised sketch lists the work-packages whose interface it touches, so that the session stops their runs.

## `plan.md`

`plans/PLAN-xxx-<slug>/plan.md`, one per plan. It serves the run of the plan and repeats nothing written elsewhere: the TASKs and the footprints are in the briefs.

- The work-packages, the core ones first, the opening work-package named.
- For each, its dependencies: what must be at least `validated` before it starts. This is the only place they live.
- For each NEED the plan closes, the work-package that closes it. That work-package depends on all those that carry the TASKs of that NEED.
- The TASKs of `design.md` left for the next plan, or "none".

When you drop a work-package, remove the dependencies on it from `plan.md`. A fix work-package goes into the plan's folder and into `plan.md`.

At each composition, check the coverage yourself: every NEED is served, and every TASK is in a work-package or left for the next plan.

## The brief

The first entry of the work-package file, as `planner`, with the status `ready`. A recomposed brief is a new entry, and the last one is in force. An answer to a wait on the footprint is a new brief too, since the judge checks the footprint against the brief in force.

On the happy path, the coder and the judge finish their work from this file alone, so the brief holds everything they need. The identifiers are there to trace back in case of doubt, and only then.

- **Header**: the identifier, and a title that says what can be run at the end; the epic and the plan; for a fix work-package, its parent if it has one, and its origin; the NEEDs it closes, if any; "Trivial: yes" when all its TASKs are mechanical.
- **Slice**: a few lines, from the user's side: where it enters, what it crosses, what data it touches, what one sees at the end. For a lever, or a closing fix that rearranges code: what is rearranged, and the guarantee that behaviour does not change. No file path here.
- **Needs**: the NEEDs it serves, with their scenarios, and what it brings to each.
- **What to know**:
  - the TASKs, with their DoD copied as they are in `design.md`;
  - the modules touched and what a caller may ask of them; the domain, if it moves;
  - the FACTs and DECs these TASKs depend on;
  - the seams of the sketch to use, and the sketch files to fill;
  - the exemplar module to follow;
  - the references to trace back: NEEDs, the tree's questions that carry the pilot's words, TASKs, FACTs, DECs;
  - for a fix work-package: the requirement it takes up, the violated DoD line, the failing gate or the revised TASK, with the diagnose template for a bug or a failing gate; for the work-package that closes the NEED of a reproduced defect: the reproduction FACT as its origin and the reference FACT as its requirement, with the diagnose template; for a WP-xxxH: each survivor, copied word for word from the hardener's list, with its number, its `path:line`, the text it replaces, its replacement and what a test should observe, and the test command of the hardener's last report.
- **Footprint**: the files reserved to the coder, as paths or patterns: code, tests, and any other file a DoD line needs, such as a doc, including the sketch's empty bodies to fill and the files the test runner writes beside those tests, such as snapshots. A work-package whose TASK adds a dependency has the manifest and the lockfile in its footprint, so two such work-packages depend on each other (rule 8). The judge checks the footprint against the diff, and the session checks it at the landing.
- **Done when**, for a fix work-package: its line of the table above.
- **Done when**, for any other work-package: the DoD of each TASK holds; if the work-package adds a behaviour, its slice test passes; if the work-package closes a NEED, its acceptance test passes, unless it is trivial, a lever or a wide change, as test-discipline says; and the common DoD: it compiles, the tests pass, the footprint is kept, no empty body is left on the path.
- **Out of work-package**: what not to do here, said plainly: the tempting next steps, the cases given to another work-package.

## The gap report

The content of your `gaps` entry, in the epic file: each gap, with the TASK, the NEED or the fact that carries it. One entry per round trip with the architect.
