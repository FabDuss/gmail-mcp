---
name: mutation
description: How the hardener proves by mutation that the tests of a work-package hold its behaviour: the list of mutants, the brief of each mutant player, the fates, the report and its status, and the replay of the survivors. Preloaded by the hardener.
---

# Mutation

A mutant is a plausible change of behaviour, played then removed. When a test that passed without the mutant fails with it, the mutant is killed, and the tests hold that behaviour. When no test fails, the mutant survives, and the behaviour can change without any test seeing it. You design the mutants from what the work-package must do, you play a short list yourself or one mutant player plays each of them, and you gather the fates into one report. The planner draws the fix work-package from it.

A pass has four phases, in order: frame the list, fan out the plays, aggregate the fates, report. It is done when every mutant of the list has a fate, and it returns one report. A replay runs the same phases on the survivors alone.

## Frame

### The list

Design the mutants from the DoD lines and the NEED scenarios of the brief in force, and from the guards you see in the diff. Keep them few and plausible, on the lines the work-package's diff writes outside its test files. Each mutant says three things.

- **Where.** The `path:line` at your head.
- **What.** One change at that place, written so that a player applies it exactly: the text the line holds, and the text that replaces it. A plausible change is a mistake a coder could make there and that the build would accept, such as `>` written for `>=`, or a guard removed.
- **What a test should observe**, in the terms of test-discipline's section "The hardener". When neither the DoD nor a NEED scenario decides the behaviour the mutant changes, the expected value is what the code does now.

Number the mutants from 1. The report and the replay name them by these numbers. Before you land the list, check at your head that each line holds the text the mutant quotes, since the list is never corrected once landed. The list is an entry that keeps the status `validated`, landed before the first player starts. A list fixed before any fate is known keeps you from choosing your mutants after the fact, so no fate you learn changes it.

```
Mutants designed from the brief in force, before any player.

1. `<path:line>`: replace `<text the line holds>` with `<text that replaces it>`.
   A test should observe: <a behaviour at an interface, its literal expected value, its level>.
2. ...
```

### The test command

One command serves the whole pass, for you and for every player. It runs the tests that the repository's tooling relates to the code files of the work-package's diff, through the commands the repository declares. A repository without test selection runs its whole unit suite instead, as at a landing. When your worktree needed an install before it, as the commit skill says for a folder with a lockfile of its own, that install goes with the command into each brief. When its runner splits the tests across parallel workers, the command caps them at two, such as jest's `--maxWorkers=2`, since players run it side by side. Run the command once at your head before the first player, and note how long it ran, as the runner prints it or as you measured it. Read `baseline.md` in the epic folder first: a test it lists is not the work-package's, and you do not replay it. Replay each other test that fails there once at your head: when it passes, it is flaky. When it fails again, replay it on the commit before the work-package's first commit, the first that adds its file at `done`: red there too, it is a known red; green there, the work-package broke it. Name each failure in your report, and mutate all the same, since the barrier's gates will catch it. Each player receives these failures.

## Fan out

When the list holds at most three mutants and the test command ran in at most 60 seconds, play them yourself, one after the other, in your worktree, by the steps of the brief below, and remove each one with `git restore` before the next. Otherwise, launch one mutant player per mutant of the list, in batches sized as your definition says. A player loads no skill, so its brief stands alone. It names the commit the player's worktree starts from and the command, so that the verdict records both, and it carries the steps and the verdict to return. Fill in this brief for each mutant:

```
You play mutant <n> of WP-012 for the hardener, in your own worktree.

Your worktree starts at <head commit>.
Mutant: in `<path>`, at line <line>, replace `<text the line holds>` with `<text that replaces it>`.
Install first: none | `<install command>`, in `<folder>`
Test command: `<command>`
Failing without the mutant: none | `<test name>`, `<test name>`

1. Apply the mutant exactly as written, and change nothing else.
2. Run the install your brief gives, if any, then the test command.
3. Run again, as it is and with the mutant still in place, each failing test that is not in the list above. A test that fails again kills the mutant. A test that passes is flaky and kills nothing.
4. When no test kills the mutant, it is a survivor, unless you can show that the change has no effect. It is then equivalent, and you give the fact that shows it: a `path:line`, or a command and its output, that anyone can check.
5. Remove the mutant with `git restore <path>`. Your worktree ends as it started, and `git status` shows no change.
6. Return this verdict, and nothing else:

   Mutant <n> at <the output of git rev-parse HEAD>, with `<command>`.
   Fate: killed | survivor | equivalent
   Killed by: none | each test that failed again, with its failure line
   Flaky: none | `<test name>`
   Equivalent because: none | <the fact>

Apply the mutant with your Edit tool. Write each command literally, with no shell variable, no `$(…)`, no loop and no heredoc, run each failing test again in its own call, and run every command through the commands the repository declares, never through `npx` or `npm exec`, `--no-install` included. To run one test file, pass it to the test command as its runner takes it, such as after `--` for an npm script, and check that the output names that file's tests.
```

## Aggregate

### The fates

Every mutant of the list ends with one fate.

- **Killed.** With the mutant in place, a test that passed without it fails, and fails again when run as it is.
- **Survivor.** No test that passed without the mutant fails, flaky tests aside.
- **Equivalent.** A survivor whose change has no effect, shown by a fact a third party can check. Without that fact, it stays a survivor.

### Reading the verdicts

A verdict counts when it names your head and your command, and gives a fate by these rules, with its evidence. A player that returned nothing, or a verdict that misses one of these, leaves its mutant with no verdict. Play that mutant yourself, by the steps of the brief, in your worktree. Check the fact each equivalent names. When it does not show that the change has no effect, the mutant is a survivor. Every fate comes from a player's verdict or from your own play.

### Hole or bug

Read each survivor against the DoD of the brief in force, on the case its change touches.

- **Hole.** The code without the mutant does what the DoD says on that case, and no test observes it. A case that neither the DoD nor a NEED scenario decides is a hole too, which the fix work-package pins by a test.
- **Bug.** The code without the mutant breaks a DoD line on that case. Name the line, quoted with its TASK, the case, and the `path:line` where the code does otherwise.

### Your status

The first that applies, in this order:

1. `bug` when a survivor is a bug, even beside holes.
2. `holes` when every survivor is a hole.
3. `hardened` when every mutant is killed or equivalent.

## Report

Your report is the entry that closes the pass, at your status. It gives every mutant of the list its fate with its evidence, one line each, and pastes no player's output. With the list, it gives the planner what a fix work-package needs: where each survivor is, what it changes, what a test should observe, whether it is a hole or a bug, and for a bug the DoD line it breaks.

```
Played at <head commit>, with `<test command>`, which ran in <seconds> s.

Failing without any mutant: none | `<test name>`, `<test name>`

| Mutant | Fate | Evidence |
| --- | --- | --- |
| 1 | killed | `<test name>`: <its failure line> |
| 2 | survivor, hole | no test failed |
| 3 | survivor, bug | breaks "<the DoD line, quoted>" of TASK-004 <title>: <the case>, at `<path:line>` |
| 4 | equivalent | <the fact> |

Played by the hardener: none | all, a short list | <mutant numbers>, <why: no verdict, or one that misses your head, your command or a fate by the rules>

### Baseline

- `<test name>`: flaky | known red, seen at <commit>
```

The Baseline section appears only when the first run or a play found a flaky test or a known red, so that the session writes it into `baseline.md`.

## The replay

After a fix work-package, the replay plays the survivors your last entry leaves, and them alone, at your head, which holds the fix. It is not a new pass.

- **Frame.** The list stays as it landed. Read in the verdict of the fix work-package only the lines that leave a survivor to your replay: a fact the coder gave for an equivalent is checked as a player's equivalent is, and makes it equivalent only when it shows the change has no effect. Locate each survivor at your head, where the fix may have moved its line. When the fix rewrote that line, apply the same change of behaviour where the fix put it, and quote the transposed text in your entry: it is the same mutant, not a new one. Run the test command once at your head, built as for the pass.
- **Fan out.** As in a pass: yourself for at most three survivors and a command of at most 60 seconds, one player per survivor otherwise, with the same brief.
- **Aggregate.** As in a pass.
- **Report.** One entry in the format of the report, whose first line reads `Replayed at <head commit>, after <the fix work-package>, with <test command>.`, and one row per replayed survivor. Its status is `hardened` when every replayed survivor is killed or equivalent, and otherwise `bug` or `holes`, by the order above.

## Done when

- Each mutant of the list gives its `path:line`, the text it replaces and its replacement, and what a test should observe, and the list landed before the first player started.
- Each mutant of the list, or each survivor on a replay, has one fate, with its evidence, from a player's verdict or from your own play.
- Each equivalent names a fact that shows its change has no effect, and each survivor is a hole or a bug, each bug with the DoD line it breaks.
- Your status is the first that applies in the order `bug`, `holes`, `hardened`, and a replay lands `hardened` only when every replayed survivor is killed or equivalent.
