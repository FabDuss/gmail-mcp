---
name: commit
description: How every Cairn run lands: the entry header and its status, one commit, and the landing path that fits how the run ended. Use at the end of every run, and whenever the session writes in an epic file or a work-package file.
---

# Commit

You end a run by landing it yourself. This skill covers the commit and nothing else: the entry header and its status, the commit message, the landing paths, and what a role clears before it lands. What your entry says is set by your own definition or skill.

Run each git command in its own Bash call, here and everywhere in your run, so that each one's output and exit code stand on their own.

In a worktree, Claude Code refuses a Bash call when it cannot check that the call stays inside the worktree, and the refusal costs you a turn. In a call that runs git or writes a file, write every path, commit and pattern literally, with no shell variable, no `$(…)` and no loop. Write no heredoc. Change the repository's files with your Write or Edit tool, never with `sed -i` or a script of your own: only git and the commands the repository declares write them otherwise, and you append an entry as Where your entry goes says. Run a script of your own only from a file you wrote, by its path.

Cairn's settings refuse a command that starts with `npx`, `npm exec` or `npm x`. A tool the repository installs runs through its npm scripts, or by its path in the `node_modules/.bin` that holds it, even where the repository writes `npx <tool>`. Node and npm find an installed package by walking up the folders from where they run: from a worktree, that walk reaches the `node_modules` at the clone's root, never one of a folder below it. So the root's dependencies come from the clone, and those of a folder with a lockfile of its own, such as one app of the repository, come from your worktree: install them there, with the install command the repository declares for that folder, before you run anything in it. Name the folder with `--prefix <folder>`, as in `npm ci --prefix <folder>` and `npm run <script> --prefix <folder>`, so that no command needs a `cd`. In a worktree, `git rev-parse --git-common-dir` prints `<clone>/.git`. Never link or copy `node_modules` into a worktree: git ignores the folder, not a link to it. A tool the repository does not install, which no gate needs, runs from a scratch folder under `/tmp` where you installed it with `npm install --prefix`.

A landing only adds to the epic branch. Push without force, keep every branch, and reach the `.git` folder only through git commands: never pass `--force`, `-f`, `--force-with-lease` or a `+` refspec to `git push`, never run git with `-C`, never delete a branch, locally or on the remote, and never run `git reset --hard`, `git clean` or `git update-ref`. To work in another worktree, which only the session does, `cd` into it and come back; to remove an untracked file, `rm` it.

## Where your entry goes

- A work-package: `.cairn/<epic>/plans/PLAN-xxx-<slug>/packages/WP-xxx.<status>.md`.
- The epic: `.cairn/<epic>/epic.<status>.md`.

Append at the end of the file, starting with a newline when the file does not end with one, and ending your entry with a newline. Write the entry with your Write tool to a file under `/tmp` named after your epic, your unit and your role, such as `/tmp/EPIC-003-WP-012-hardener.md`, append it with `cat <that file> >> <your unit's file>` alone in its Bash call, then remove the temporary file. Leave earlier entries as they are. Then rename the file to the status your entry sets: `git mv WP-007.ready.md WP-007.done.md`. A new file is born with the status of its first entry.

## The entry header

```
## <time> · <author> · <status>
```

- **`<time>`**: UTC. Read it from `date -u +%Y-%m-%dT%H:%MZ` just before the `git commit` that first carries the entry, even when you wrote the entry earlier, and never guess it. An amend or a rebase keeps it.
- **`<author>`**: your role, as your definition names it (`architect`, `planner`, `coder`, `judge`, `hardener`), or `session`.
- **`<status>`**: one word from your row below. It tells the session what comes next.

The content under the header follows the format your own definition or skill sets. Copy each count, commit or result your entry cites from the output or the report that gave it, and run the command again when that output is gone.

## Your statuses

| Author | Work-package file | Epic file |
|---|---|---|
| planner | `ready` when you open a work-package or recompose its brief; `dropped` when a revision leaves it no TASK and it has landed nothing; `done` when you have resolved a landing that could not go through; `blocked` addressed to the architect when you pass on a wait whose answer would change the design | `planned`, which lets the current plan's work-packages start, once the sketch has landed, gates passed, at composition; or `gaps`. Every planner run launched on `designed` ends on `planned` or `gaps` |
| architect | `ready` when your answer to a wait leaves the brief in force as it stands; `blocked` addressed to the planner, carrying your answer, when the brief must be recomposed | `designed`; `blocked` when the pilot must answer |
| coder | `done`; `blocked` | |
| judge | `validated`; `challenged`; `blocked`, addressed to the architect or the planner | |
| hardener | `hardened`; `holes`; `bug` | |
| session | `blocked` when a work-package's `challenged` goes beyond the bound, a landing could not go through, or a coder ran out of turns; `hardened` on a WP-xxxH, on a trivial work-package the judge did not note "non trivial", on the parent of a WP-xxxH whose verdict shows every survivor killed, or when the pilot accepts the survivors a work-package keeps at the bound of hardener reports, with an entry saying why | the pilot's answer to a wait; `draft`, `ideation`, `confirmed`, `designed`, `closing`, `delivered`, `abandoned`; `confirmed` or `designed` also when it sends a gap back to the architect or the planner, with the gap in the entry |

- An entry may keep the file's current status: it records a fact and changes nothing that comes next.
- `blocked` addresses the footprint and the sketch to the planner, and the design to the architect. Only the architect addresses the pilot, and only in the epic file.

## Answering a wait

Answer in the file that asks, with an entry whose status restarts the work. If your answer changes something you do not own, end with `blocked` addressed to its owner instead. A wait on the intent is answered by the session's re-invocation.

## The commit

One commit carries the run: `git commit -m "<subject>"`. A hardener's pass is the one exception: the hardener lands its list of mutants first, as an entry that keeps the file's status, by the Finished path, before it launches a single player; then it lands its report.

- **Subject**: `<type>(<unit>): <summary>`. The unit is the work-package or the epic of your entry, as its file names it (`WP-012`, `WP-012H`, `WP-012B`, `EPIC-003`), or their closest common parent, plan or epic, when your entries span several files. Add ` [skip ci]` when the commit touches only `.cairn/`.
- **Type**: `feat`, `fix`, `refactor`, `perf`, `test`, `docs` or `chore`. A commit that holds only Cairn files is `chore`.
- **Summary**: short and imperative, with no trailing period. Name a real symbol when one carries the change: `feat(WP-012): parse ISO dates in ImportRow`, never `feat(WP-012): import improvements`.
- **No body of your own**: your entry carries the detail, and the summary does not restate your status. If your environment asks for attribution trailers, give them in a second `-m`, so that a blank line separates them from the subject.

## Choose your path

Exactly one of these three fits how your run ended. Any of them sends you to Cannot land when it says so.

| How the run ended | Path |
|---|---|
| Your work is done, or you answered a wait | Finished |
| You stopped on a wait of your own: `blocked`, or the planner's `gaps` | Stopped |
| You merged main into the epic: the session at a barrier, or a resolver | Merge |

The epic branch is `epic/<id>/main`, where `<id>` is the folder under `.cairn/`: `.cairn/EPIC-012/` lands on `epic/EPIC-012/main`.

Before it pushes, or before its last message on Cannot land, a role removes each scratch file it created outside its worktree, by its path, and stops each process it started, by its PID. It never removes a cache or a shared folder, and never stops a process by its name, since runs in parallel use them.

### Finished

You work from the base your worktree started on: bring nothing from origin into your worktree before your commit, with no merge, pull, rebase or reset. What others landed meanwhile reaches you only through the rebase of step 3, where a conflict shows it.

1. If the run touches code, read `baseline.md` in the epic folder, then run the gates, through the commands the repository declares, which carry its own flags and the versions it pins. The gates are build and type checking, and lint where the repository enforces it: its CI or a commit hook runs it, or its `CLAUDE.md` says a change must pass it; the tests related to your diff, selected by the repository's tooling, never by you, or the whole unit suite when the repository has no test selection; and your own tests. To run one test file, pass it to the repository's test command as its runner takes it, such as after `--` for an npm script, and check that the output names that file's tests. A test listed in `baseline.md` does not block you. Replay any other failure on the same commit, then on your base: if it passes on the same commit it is flaky, if it also fails on your base it is a known red; either way it is not yours, so name it in your entry and go on. A failure that passes on your base is yours: fix it.
2. Write your entry and rename the file. Restore a tracked file that a command changed without your run asking for it, and remove an untracked file it generated. Then `git add -A` and commit. Your code and your entry go in this one commit; push nothing before it.
3. `git fetch origin epic/<id>/main`, then `git rebase origin/epic/<id>/main`. Resolve a conflict only by keeping both sides as they were written, as when both added lines at the same place: `git add` the file, then `GIT_EDITOR=true git rebase --continue`. If resolving would remove or change anything the other side wrote, you do not settle it: `git rebase --abort`, then Cannot land.
4. If the rebase brought in code, check that your run still holds on it before you push. A run that touches code runs the gates again. A run that writes a design or a plan reads what that code changes in the code and the facts its artifact names. Fix your run where it no longer holds, then `git add -A` and `git commit --amend --no-edit`, so that your run stays one commit. A gate failure that also fails on the epic head you rebased onto is not yours: name it in your entry, as in step 1, and go on.
5. `git push origin HEAD:epic/<id>/main`. Never force. If the push is refused, the head has moved: go back to step 3.

### Stopped

You land your entries, and the facts you wrote, and nothing else. The next run starts again from the brief and the answer.

1. Write your entry with its wait in your unit's file, and any other entry your run lands, such as a gap report in the epic file. Rename each file to its new status.
2. Stage those files under their new names with `git add <file>`, and `intent.tree.md` too if you wrote FACTs in it: a fact does not depend on the answer you wait for. `git diff --cached --stat` shows lines added in each file where you wrote an entry; a file shown renamed with nothing added has lost its entry, so stage it again. Commit with ` [skip ci]`. Leave the rest of your work uncommitted: it is not landed.
3. `git fetch origin epic/<id>/main`, then `git rebase --autostash origin/epic/<id>/main`, which sets your uncommitted work aside during the rebase. Then `git push origin HEAD:epic/<id>/main`. Never force. If the push is refused, repeat this step.

A planner that stops on `gaps` lands its gap report and no partial sketch.

### Merge

A merge commit is pushed as it is. Never rebase it: a rebase drops the merge and replays main's commits one by one.

1. `git fetch origin main epic/<id>/main`, then, from the epic head, `git merge --no-commit origin/main`. If you are the resolver, resolve the conflicts as resolving-merge-conflicts says, and never abort the merge. If you are the session and the merge conflicts, `git merge --abort` before you hand it to a resolver, so that your clone stays clean.
2. Run the gates on the merge: the tests related to the epic's diff from main, selected as in Finished step 1, and the integration tests of the touched areas when the repository can target them, or the full suite at the session's last barrier, read against `baseline.md`. Replay any other failure on the same commit, where a pass makes it flaky, then on `origin/main`, where a failure makes it a known red. A merge that fails beyond the baseline is pushed as well: no other run is going while main enters, and a fix work-package will make it green. Say so in your entry.
3. Write your entry and rename the file, then `git add -A` and commit.
4. `git push origin HEAD:epic/<id>/main`. Never force. If the push is refused, the head has moved: `git fetch origin epic/<id>/main`, `git merge origin/epic/<id>/main`, run the gates again, and push again.

### Cannot land

Push nothing. End the run with a last message that says why, the conflicting files, and names your branch (`git branch --show-current`). The session records it as `blocked` in your unit's file, addressed to the planner, or, for a design run, in an entry that keeps the epic's status. Your work waits on your branch for the planner, who resolves it.

## The session's own writes

The session lands its writes from its clone, on the epic branch, staging only the files it wrote: `git add <files>`, one commit, then `git fetch origin epic/<id>/main`. Check that the epic head you knew is an ancestor of the fetched one, with `git merge-base --is-ancestor <known head> origin/epic/<id>/main`. When it is not, someone rewrote the branch: stop and tell the pilot, as the session skill says. Otherwise `git rebase origin/epic/<id>/main`, then `git push origin HEAD:epic/<id>/main`. Never force. If the push is refused, repeat from the fetch. To merge main into the epic, it takes the Merge path.

- **During ideation**: land the tree, and the intent if it changed, at the end of every round, in one commit with ` [skip ci]`.
- **The barrier commit**: subject `chore(PLAN-xxx): barrier`, naming the plan whose barrier passed, so that what a plan delivered reads between two barrier commits.
- **Another epic's branch**, to create a draft, add an entry to one, or drop one: write through a temporary worktree under `.claude/worktrees/`, which git ignores, started from that branch, or from `origin/main` for a new epic. Your shell starts each call in the folder of the last `cd` any call made, so write each command for that worktree as `cd <worktree> && <command>`. Push, check that `git ls-remote origin <branch>` prints your commit, since git prints none for a new branch, then remove the worktree. Your clone's HEAD never leaves the epic you lead, since every worktree starts from it.

## Done when

- Your entry is the last one in your unit's file, its header in the form above, with a status from your row, and the file is named after that status.
- One commit carries your run, two for a hardener's pass (its list, then its report), each with a subject in the form above; a stopped run's commit holds only its entry's file, and the tree if it wrote FACTs.
- `git log -1 origin/epic/<id>/main` shows your commit, or your last message says why you could not land.
