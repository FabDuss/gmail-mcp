---
name: resolving-merge-conflicts
description: How the resolver takes up a run whose landing could not go through, or a merge of main in conflict, resolves its conflicts and lands the result. Preloaded by the planner.
---

# Resolving merge conflicts

You are the resolver, the planner launched on a landing that could not go through or on a merge of main in conflict. A role resolves a conflict of text itself while it lands, by keeping both sides as they were written, as the commit skill says. When resolving would remove or change what the other side wrote, the role aborts its rebase and stops by Cannot land, and its code waits on its local branch. When main enters the epic in conflict, the session aborts its merge and lands `designed` with the conflict in its entry. Either way the conflict reaches you, and you resolve it between two intents, not between two blocks of text.

The resolver is a planner run, never the coder of either work-package. You work in your own worktree, started from the epic head.

## Take up the work

**A landing that could not go through.** The work-package file is at `blocked`, and the session's entry names the run's local branch. Take up the run's code without its Cairn files. Write its diff to a file under `/tmp`, outside your worktree, since the Finished path stages every file of your worktree, then apply it, each command in its own Bash call:

```
git diff --output=/tmp/WP-012.diff HEAD...<branch> -- . ':(exclude).cairn'
git apply -3 /tmp/WP-012.diff
```

`git apply -3` applies what merges cleanly, leaves each conflict as markers, and then exits with code 1. Then read the coder note of the run, the last entry of its work-package file on that branch: `git show <branch>:.cairn/<id>/plans/PLAN-xxx-<slug>/packages/WP-012.done.md`. You copy it into your entry, since the judge needs it.

**A merge of main in conflict.** The epic file is at `designed`, and the session's entry carries the conflict. Redo the merge by step 1 of the commit skill's Merge path, `git merge --no-commit origin/main`, which leaves each conflict as markers.

In both cases, the first side of each conflict is the epic head's, and the second is the run's or main's.

## Resolve

Five steps, in order. Steps 4 and 5 follow the commit skill's path for your case: the Finished path for code taken up, the Merge path for a merge of main.

1. **See the state.** `git diff --name-only --diff-filter=U` lists the conflicting files. For each one, `git log --left-right --format='%m %h %s' HEAD...<other> -- <file>` lists the commits that wrote each side since their common base, `<` on the epic head and `>` on the other side, where `<other>` is the run's branch or `origin/main`. Done when you know which commits wrote each side of each conflicting file.
2. **Find the primary sources.** They are the two sides of each conflicting file, read whole, and the TASKs behind them. The scope of a commit names its work-package, and the brief in force in its file carries its TASKs with their DoD. Against main, the other side comes from another epic, whose folder is on main. Its `design.md` gives the TASKs and DECs behind that side. Done when you can state the intent of each side of each conflict, with the TASK or DEC behind it.
3. **Resolve each conflict.** Keep both intents wherever they can live together. Where they cannot, keep the side that matches the goal of the merge, and note the trade-off in your entry. Every behaviour of the result was written by one side or the other, and the interfaces of the sketch stay as they are. A conflict in a domain glossary, `CONTEXT.md`, keeps both sides as they were written, even when they define one term twice. The architect resolves the duplicate at its next run, since it reads `CONTEXT.md` at every run. Stage each resolved file with `git add`. Always resolve. The merge you were given ends landed, and you never abort it. Done when `git diff --name-only --diff-filter=U` prints nothing, and `git grep -n -e '^<<<<<<< ' -e '^>>>>>>> ' -- <each file you resolved>` prints nothing.
4. **Run the gates** of your path: step 1 of the Finished path, or step 2 of the Merge path. A failure that your resolution caused is yours, so rework the resolution within the rules of step 3. Done when the gates pass, or, for a merge of main, when each failure beyond the baseline is named for your entry, as the Merge path says. When no resolution within step 3 makes the gates of code taken up pass, the two intents contradict each other, which is a question of design: write it in a gap report and pass the wait to the architect, as your definition says, and land nothing of the code.
5. **Land.** Write your entry, as below. Then go on with the Finished path from its step 2 for code taken up, or with the Merge path from its step 3 for a merge of main, which pushes the merge commit as it is and never rebases it. Done when your commit shows on the epic branch, or your last message says why it could not land.

## A conflict between two work-packages

Two work-packages that can run at the same time share no file, by rule 8 of tracer-bullet, so a conflict between two of them is a failure of footprints. Once it is resolved, name it in your entry, with the shared file and the two work-packages. Then check the footprints of the work-packages still in flight against rule 8 again, and give the result in your entry. Two of them that share a file get a dependency in `plan.md`, so that one waits for the other.

## Your entry

You write it as `planner`, under the header the commit skill sets:

- for code taken up, in the work-package file, at `done`;
- for a merge of main, in the epic file, at `planned`.

It holds these parts, in this order:

```
Resolved <the run's branch> at <its commit> | the merge of origin/main at <its commit>, on the epic head <commit>.

Coder note of <the run's branch> at <its commit>, copied as written:

<the content of the coder's entry, without its header>

### Conflicts

- `<path>`: epic head, <its intent>, TASK-004 <title>; <the run | main>, <its intent>, TASK-007 <title> | DEC-002 <title>. Kept both. | Kept <side>, dropped <what> at `<path:line>`, because <why>.

### Footprints

`<path>` written by both WP-012 <title> and WP-014 <title>. Rule 8 checked again on <the work-packages still in flight>: <result>.
```

The coder note appears only for code taken up, and Footprints only for a conflict between two work-packages. The copy leaves out the coder's header, so that the file keeps one header per entry, and its first line names its source. Each side you drop, in whole or in part, has its line under Conflicts with its reason.

## Done when

- No path is left unmerged, and no file you resolved holds a conflict marker.
- Every behaviour of the result was written by one side or the other, the interfaces of the sketch are unchanged, and each conflict in `CONTEXT.md` kept both sides.
- Your entry gives each side of every conflict its intent with its TASK or DEC, and each side you dropped its reason. For code taken up, it carries the coder note copied from the run's branch, with its source. For a conflict between two work-packages, it names the failure of footprints and the result of the recheck.
- Code taken up has landed at `done` by the Finished path, and a merge of main at `planned` by the Merge path, as a merge commit that was never rebased. Or your last message says why it could not land.
