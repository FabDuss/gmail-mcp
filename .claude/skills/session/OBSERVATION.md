# The observation

At step 5 of the barrier, and at the close when main entered, you observe the NEEDs that your skill names: those that `plan.md` says this plan closes and, at the last barrier and at the close, those whose latest observation saw other code than your head. You play each one on the running app, at the head that main entered and your gates passed on, and each proof becomes a FACT in the tree. What the environment keeps you from observing goes to the pilot. The pilot then accepts or rejects the observation, and the barrier goes on from step 7, or at the close, the close goes on as CLOSE.md says. At the last barrier the architecture critique reads your clone while you observe: keep every scratch file under `/tmp`, and write nothing in the clone but the tree.

A test run belongs to the gates, never to the observation. Your drive runs no test and reuses none, the acceptance tests of the epic included.

## What you observe

- Run `git rev-parse HEAD` in your clone. Every FACT of this observation gives that commit, and the pilot observes that same commit.
- Take from `plan.md` the NEEDs this plan closes. At the last barrier, and at the close when main entered, add each NEED of the intent, replaced ones aside, whose latest observation entry, of this plan or an earlier one, gives a commit for which `git diff --quiet <commit> HEAD -- ':/' ':(top,exclude).cairn'` fails. Take from the intent the scenario of each one, given, when and then, or the threshold of a constraint NEED.
- Read the slice in the brief of the work-package that closes each NEED. It says where the behaviour enters and what one sees at its end.
- When the intent names a reproduced defect, replay its drive, or, for a reproduction by script, its scenario along the user's path: the same steps now show the reference holding.
- FACTs of this barrier already in the tree, as after a restart, stand when `git diff --quiet <their commit> HEAD -- ':/' ':(top,exclude).cairn'` succeeds: observe again only the NEEDs they leave.
- The tree may hold FACTs from an earlier barrier, or from the reproduction, that show how the app was started and driven. Rely on them as facts says, and search one again when its code has moved since its commit.

## Ask the repository

Answer these from the repository, through its scripts, its task runner, its README, its configuration and its existing harnesses. Ask the pilot only what the repository cannot tell.

- **Surface.** Where a user meets the behaviour the scenario names, such as a screen, a command, an exposed API or a scheduled job.
- **Start.** The command the repository declares to start the app, with its ports, its environment variables, its seed data and its authentication.
- **Drive.** The tools the repository already has to drive that surface, such as a browser driver and its helpers, a PTY helper or a debug port. Without one, drive a web app with a headless browser, a CLI through a PTY and a service over plain HTTP. Run a tool, one the repository lacks included, as the commit skill says.
- **Evidence.** What a user sees and what the app leaves behind, such as a screen, a terminal transcript, a response body, a log line, an exit code or a stored row.
- **Isolation.** Whether your instance can run beside another one, on its own port and with its own data.

The environment holds only the secrets that every role may see. A NEED that needs another secret goes to the pilot, and the secret itself stays out of the conversation.

## Launch and check

1. **Start your own instance** from your clone, in the background, with the command the repository declares. Give it its own port, and its own data in a scratch folder under `/tmp`, when the repository lets you choose them. Drive no instance you did not start. Keep your drive scripts, and everything they write, under `/tmp`. Run a repository tool from the clone only when it needs that working directory, with its output sent under `/tmp`, and check `git status` after each drive.
2. **Wait for the ready signal**, such as a log line, a port that answers or a prompt, by checking that signal itself. Claude Code blocks a `sleep` that only waits. A short-lived CLI has no server to wait for. Build it once, then start each drive on its own.
3. **Check that it runs**, read-only, before the first drive and again whenever anything looks off. The process you started is up, it answers where you expect, it serves the build of your head, and its authentication works.

You observe the head as it is, and you change no code. A head that does not build or start is an observation like any other, which you prove and bring to the pilot. When a missing file that no scenario reaches blocks the start, such as a sample configuration, create it as scratch and remove it at cleanup.

## Drive the scenario

Play each scenario as the intent writes it, along the path a user takes.

- **Given.** Set up the state through the app's own paths, or with the seed data the repository declares.
- **When.** Act at the entry the scenario names, as a user does, through the screen, the command or the exposed API. Reach each element by a stable handle, such as an accessible role and name, a data attribute, a prompt string or a route path, rather than by coordinates or position.
- **Then.** Read what a user sees, and the effects the app leaves behind.

The path stays the user's from start to end, so an internal setter, an endpoint that exists for tests and a test run have no place in it. Simulate an external system only where a production boundary already isolates it. When the safe path is a dry run or a test mode, watch the files, the network and the git refs to learn what it really skips, rather than trusting its name.

For a constraint NEED, take the measurement its threshold names, on the running app and under the conditions the NEED states, and give the number.

## Prove

Capture the action and the state it leads to, never the final screen alone. Read a stored change back through a second, read-only view, such as the record reopened from its list, since a status message alone proves nothing. Check the side effects beside what is visible, such as the files written, the rows inserted and the messages sent.

Write each proof as a FACT in the tree, by facts:

- **Observed**: the action you took, the command with its exit code, and the lines of output or the state that show the result.
- **Source**: the entry you drove, such as a route, a command or a screen.
- **Commit or date**: your head.
- **Certainty**: observed in the running app.
- **Used by**: the NEED, and the barrier of `PLAN-xxx`, the plan the barrier commit names, or the close.

A FACT says what you saw, never whether the NEED holds. You read it against the scenario when you put the observation to the pilot.

Quote only the lines that carry the proof, with every secret in them replaced by `<REDACTED>`, since the tree reaches main with the epic folder.

A scenario you could not play is not observed. Its FACT gives the command you tried and the precondition that was missing. A scenario played along another path than the one it names is not observed either.

Anything else you observe that the next plan should know, such as an error in a log, becomes a FACT too. The planner composes the next plan from the facts observed at this barrier.

## Clean up

- Stop each process you started, and only those. A match on a process name can stop processes you did not start, such as the pilot's own.
- Remove each file you made, by its path: scratch data, scaffolding and throwaway scripts. Restore the seed data you changed, and any tracked file a command changed, with `git restore <path>`.
- Leave the tree as you wrote it. Your FACTs hold the proof in text, so cleanup removes no evidence.
- Check that `git status` shows no change but the tree. Then land the tree by the commit skill's session writes, before you put anything to the pilot. Your turn ends once the observation is put to them, and every write of yours is landed by then.

## Put it to the pilot

Put the observation to the pilot in one message. Take the NEEDs one by one, each shown by its title with its identifier, and give each one of these:

- **Observed by you.** How what you saw reads against its scenario, with its FACTs.
- **For the pilot to observe.** The steps of its scenario, and what they should see at its end.
- **Observable by nobody.** What was tried and what is missing. It is judged on the gates, which passed at your head, and the pilot decides.

For what they must observe, give the branch `epic/<id>/main` and your head, already merged with main, with the exact commands to check it out, install it and start it on their machine. When the repository's CI deploys the epic branch, give that deployment at your head instead.

Ask them to accept or reject the observation, and on a rejection, what they saw. At the last barrier, the same message then carries the closing review's items, as your skill says, and tells the pilot that the PR goes ready on their acceptance of the NEEDs alone, unless they fix a proposal in this epic, and that a proposal they leave undecided is dropped. When they cannot answer live, send it by to-questionnaire.

Write what the pilot reports seeing as a FACT, with the pilot as its source, the commit or deployment they observed, and "reported by the pilot" as its certainty. Their acceptance leads to the barrier commit, or at the close to your observation entry, and their rejection to the gaps, as your skill says.

## The barrier entry

Your barrier entry, under the header the commit skill sets, carries the observation in this form, with one line for each NEED you observed. At the close, the same lines go in an entry of yours that keeps the status. Each entry in this form is an observation entry:

```
Observed at <commit>.

- NEED-002 <title>: holds, observed by the session. FACT-031, FACT-032
- NEED-004 <title>: holds, observed by the pilot. FACT-035
- NEED-005 <title>: observable by nobody, judged on the gates. FACT-036
```

## Done when

- Each NEED this barrier observes is observed by you, observed by the pilot, or named as observable by nobody, with what was tried.
- Each proof is a FACT at the observed commit that gives the action, what was seen and the entry driven, and quotes no secret.
- Nothing you started still runs, `git status` shows no change of yours, and the tree is landed.
- The pilot has accepted or rejected the observation, and what they reported seeing is a FACT.
