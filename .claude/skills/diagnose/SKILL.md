---
name: diagnose
description: How the coder repairs a bug, a failing gate or a defect reproduced at ideation, from a loop that goes red to the confirmed cause, and the DoD template that the judge cites. Preloaded by the coder and the judge.
---

# Diagnose

A fix work-package, or the work-package that closes a reproduced defect, repairs a behaviour that an existing requirement already sets. The coder works by this skill when the brief in force calls for the diagnose template, on a bug, a failing gate or a reproduced defect. The judge reads it for the template alone, whose lines count as DoD lines of that brief. A WP-xxxH never calls for it, since it repairs nothing and only adds the tests that kill survivors.

The work goes through six phases, in order, and everything rests on the first. Phase 1 builds a tight loop that goes red on this bug, before any hypothesis. With that loop, bisection, hypotheses and instrumentation only consume its signal, and the cause turns up. Without it, no amount of reading the code finds the cause. Size each later phase to the bug, and give a short one its line in your coder note, as your definition asks of short work.

## The origin

The header of the brief in force names the origin of the work-package. The symptom you reproduce is the one that origin names, never a different failure nearby, since the wrong bug gets the wrong fix.

- **A bug the hardener found.** A survivor of the hardener report on the parent work-package, with the case its change touches and the DoD line it breaks.
- **A gate the session sent back to the planner.** A failing gate, with its failing test and its failure line.
- **A defect reproduced at ideation.** The intent's reproduction FACT, with its script or its drive, against its reference FACT.

The brief carries what you need from the origin. Trace back to the origin's entry, by the identifiers of the brief, only when the symptom is in doubt.

## An existing requirement

A fix takes up a requirement that already exists, and never writes a new one. That requirement is the DoD line the bug breaks, the failing test of a gate, or the reference a reproduced defect's intent names, and the brief names it. Your regression test asserts it, with the literal expected value that requirement gives, never the value the code returns now. When no DoD line, no NEED scenario and no existing test decides the right behaviour, there is no requirement to take up. End on a wait on the design, addressed to the architect.

## Redact

Your coder note and every output you paste into it are committed and pushed, and they reach main with the epic folder. Redact every secret first, with `<REDACTED>` in its place. Build loops against environment variables, so that a credential stays in the environment and out of what you write. A captured artifact carries auth headers, so quote only the lines that carry the signal.

## 1. Build a feedback loop

This phase is the skill, and the rest is mechanical. A tight pass or fail signal that goes red on this bug lets you find the cause. Spend most of your effort here, and keep at it until you have one.

In your worktree, the commit skill's rule on Bash calls holds here: a shell loop, a stress run or a harness goes in a script file you write and run by its path.

Ways to build one, roughly in this order:

1. A failing test at whatever seam reaches the bug, at a level the repository names.
2. An HTTP script against a running dev server.
3. A CLI invocation with a fixture input, its output diffed against a known-good snapshot.
4. A headless browser script that drives the UI and asserts on the DOM, the console or the network.
5. A captured trace, such as a real request, payload or event log, saved to disk and replayed through the code path in isolation.
6. A throwaway harness that starts a minimal part of the system, one service with its dependencies mocked, and runs the bug's code path in one function call.
7. A property or fuzz loop, when the output is wrong only sometimes, run on a thousand random inputs to find the failure mode.
8. A bisection harness, when the bug appeared between two known states, such as two commits, datasets or versions. Automate "start at state X, then check", and run it with `git bisect run`.
9. A differential loop, which runs one input through the old and the new version, or through two configurations, and diffs the outputs.

Then tighten the loop, as you would a product:

- Make it faster. Cache the setup, skip unrelated initialisation, narrow the test scope.
- Make the signal sharper. Assert the exact symptom of the origin, never "it did not crash".
- Make it deterministic. Pin the time, seed randomness, isolate the filesystem, freeze the network.

A flaky loop of thirty seconds is barely better than none. A deterministic loop of two seconds is tight.

**A non-deterministic bug.** Aim for a higher reproduction rate, not a clean repro. Loop the trigger a hundred times, run it in parallel, add stress, narrow the timing windows, inject delays into the code under test. A bug that shows half the time can be debugged, and one that shows once in a hundred cannot, so raise the rate until it can.

**No loop.** When you cannot build one, form no hypothesis. End on a wait on the design, addressed to the architect, since the DoD says how each line is verified and that verification is now in question. Give the numbered list of what you tried and what would let you build the loop.

Phase 1 is done when you can name one command, such as a test invocation, a script path or an HTTP call, that you have already run at least once, with its invocation and output kept for your note, and that is:

- red-capable, since it drives the bug's code path and asserts the exact symptom of the origin, so it goes red on this bug and green once the bug is fixed;
- deterministic, with the same verdict on every run, or a pinned, high reproduction rate for a non-deterministic bug;
- fast, in seconds rather than minutes;
- runnable unattended, by you alone.

If you catch yourself reading code to build a theory before this command exists, stop. A jump to a hypothesis is the exact failure this skill prevents.

## 2. Reproduce and minimise

Run the loop and watch it go red. Before you go on, confirm that:

- the loop produces the failure the origin names, not a different failure nearby;
- the failure reproduces across several runs, or at a rate high enough to debug against;
- you have captured the exact symptom, such as the error message, the wrong output or the measured time, so that phase 5 can check the fix removes it.

Then shrink the repro to the smallest scenario that still goes red. Cut inputs, callers, configuration, data and steps one at a time, and run the loop after each cut. A minimal repro leaves fewer parts to suspect in phase 3, and it becomes the regression test of phase 5. The phase is done when every remaining element is load-bearing, so that removing any one of them turns the loop green.

## 3. Hypothesise

Write three to five ranked hypotheses before you test any of them. A single hypothesis anchors you on the first plausible idea. Each one is falsifiable and states its prediction, in this form:

> If <X> is the cause, then <changing Y> makes the bug disappear, or <changing Z> makes it worse.

A hypothesis whose prediction you cannot state is a hunch, so sharpen it or drop it. Write the ranked list into your coder note before the first probe, and go on without waiting for anyone to rank it again.

## 4. Instrument

Each probe tests one prediction of phase 3 and changes one variable at a time. In order of preference:

1. A debugger or a REPL, when you can drive it from a command. One breakpoint beats ten logs.
2. Targeted logs at the boundaries that tell two hypotheses apart, never every line logged and grepped afterwards.

Tag every debug log, and every other line you add only to probe, with one prefix for the whole run, made of `[DEBUG-`, four random hexadecimal characters and `]`, as in `[DEBUG-a4f2]`. Removing them is then one search, and a line without the tag escapes that search. The tag holds no Cairn identifier, since none appears in the code.

**Performance.** When the NEED behind the requirement is about performance, logs are usually the wrong tool. Take a reference measurement first, with a timing harness, a profiler or a query plan, then bisect. Measure first, fix second.

## 5. Fix and regression test

Write the regression test before the fix, at a correct seam. A correct seam lets the test run the real bug pattern as it occurs at the call site. A seam too shallow gives false confidence, such as a test of a single caller when the bug needs several, or a unit test that cannot replay the chain that triggered the bug. When no correct seam exists, that is the finding, since the architecture keeps the bug from being locked down. End on a wait on the design, addressed to the architect, with where the bug occurs and why each seam you found is too shallow.

At a correct seam:

1. Turn the minimised repro into a failing test at that seam, placed and named as test-discipline says.
2. Watch it fail on the symptom of the origin.
3. Fix the cause that the confirmed hypothesis names, where it lives.
4. Watch the test pass.
5. Run the phase 1 loop again, on the original scenario before it was minimised.

A guard that silences a crash, such as a null check before the failing access, hides the symptom and leaves the cause in place, so it is no fix. When the cause lives in a file outside your footprint, that is a wait for the planner, never a guard inside the footprint.

## 6. Remove the instrumentation

Before you land, check each of these:

- the phase 1 loop, run again, no longer reproduces the bug;
- the regression test passes;
- `git grep -n` on your tag prints nothing;
- every throwaway harness and script is deleted, since the landing stages every file of your worktree;
- the hypothesis that turned out right is in your coder note.

## The template

When the brief in force calls for the diagnose template, these five lines join its Done when. The coder holds them, and the judge rules on each one as a DoD line, quoted as written, with its proof. The rest of this skill is the coder's method, and the judge cites none of it.

1. A regression test goes red on the symptom of the origin before the fix, and green after it.
   Verified by running it at the head, where it passes, then with the code files of the fix restored to the commit before the work-package's first commit, the first that adds its file at `done`, and the tests left as they are, where it fails with that symptom. Restore those files afterwards.
2. No behaviour changes but the one the fix restores.
   Verified by reading the diff, and by the gates, where every test that passed before the work-package still passes.
3. The regression test stays in the suite.
   Verified by reading the diff, which holds the test in a file the test runner collects, with nothing that skips it.
4. No instrumentation remains.
   Verified by running `git grep -n` at the head on the tag the coder note names, which prints nothing.
5. The coder note states the confirmed hypothesis.
   Verified by reading the note, which gives the cause, the prediction that held and the probe that showed it.

## Your note

When the brief calls for the template, your coder note also holds:

- the command of the loop and the output that showed it red, redacted;
- the ranked hypotheses, as you wrote them before the first probe, each with its prediction;
- the confirmed hypothesis, with the cause, the prediction that held and the probe that showed it;
- the tag of your instrumentation;
- a neighbouring bug you met, among what you had to leave.

## The bounds of a fix

- The footprint stays as the brief sets it. A neighbouring bug, even one of the same pattern, belongs to another work-package. Name it in your note, and leave its code as it is.
- The lines around the fix stay as you found them, even those you would clean.
- Performance work belongs to a fix only when the NEED behind the requirement it takes up is about performance.

## Done when

- **Coder.** The loop went red on the symptom of the origin before your first hypothesis, the ranked hypotheses are in your coder note, and every line of the template holds. Or your run ended on a wait that names the bar it could not pass: no loop, no correct seam, a cause outside the footprint, or no requirement to take up.
- **Judge.** Each line of the template has its ruling, held or not held, with its proof.
