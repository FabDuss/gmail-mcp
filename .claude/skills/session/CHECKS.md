# Start check, landing checks and bounds

Instructions alone do not hold the rules below. These checks and counts do. Except the start check, which reads your shell, they read only git, so that a compaction loses nothing. Run them from your clone's root.

## The start check

Claude Code applies the repository's `.claude/settings.json` in a session opened on this repository alone, whose working directory is the repository. In a session opened on several repositories, whose working directory is above them, Claude Code still loads the agents and the skills, but not that file: worktrees start from the remote default branch, no role can launch a helper, and no deny rule holds.

Cairn's `settings.json` sets `CAIRN_SETTINGS` to `1` under `env` for this check alone, and nothing else sets it. `printenv CAIRN_SETTINGS`, alone in its Bash call, prints `1` when the file is in force. When it prints anything else, or nothing, launch no run and land nothing: reread and answer the pilot only, and tell them to open the session on this repository alone.

## The landing checks

Check every commit a run landed, as soon as you read its landing. After a compaction or a restart, before your next launch or landing, check again every run commit since the last barrier commit, whatever a summary says of the checks already run. The run's role is the one its launch named, as in `WP-012 coder`. After a compaction, when the launch is lost, it is the author its entries name: all the entries of one commit share that author, who is the role the chain sends the file to from its status before the commit. Read the epic's own folder, `.cairn/<id>/`, and no other: a merge of main brings the folders of other epics, which are not the run's.

1. **Cairn's files.** `git show --name-only --format= <commit> -- .claude .mcp.json` prints nothing. A merge commit is checked against its parent that comes from main, the one for which `git merge-base --is-ancestor <parent> origin/main` succeeds: `git diff --name-only <parent> <commit> -- .claude .mcp.json` prints nothing, since a merge keeps them as main has them. When neither parent comes from main, as when a resolver merges the epic head into its merge, that diff prints nothing against one of the two. This check is what keeps the agents from changing themselves.
2. **Entries only add.** `git diff -M --numstat <commit>^ <commit> -- '.cairn/<id>/epic.*.md' '.cairn/<id>/*/WP-*.md'` shows 0 in its second column, the removed lines, on every line. Two cases show removed lines without a rewrite: a rename git does not pair, shown as a whole file removed and another added, and a last line that lacked its newline, shown as removed then added back. In both, the old file must be the start of the new one, byte for byte: save both versions under `/tmp` with `git show <commit>^:<old path>` and `git show <commit>:<new path>`, then `cmp -n <size> <old> <new>`, the size given by `git cat-file -s <commit>^:<old path>`, exits with 0. When the old file does not end with a newline, the byte that follows it in the new one is a newline, so that no header is glued to the line before.
3. **Statuses.** Each entry the commit adds has the run's role as author, and as status a word from that role's row of the commit skill, or the status the file already had, for an entry that keeps it. Each file the commit renames takes the status of its last entry.
4. **Files.** A coder's commit touches its work-package file and files of the footprint of the brief in force, and nothing else. A judge's or a hardener's touches its work-package file alone. The architect and the planner write across the epic, so their commits get checks 1 to 3 only.

When a check fails, stop and tell the pilot, and launch nothing on that unit until they decide. When they let the commit pass, land an entry that keeps the status, whose first line reads `Pilot's pass: <commit>`, and a later check skips that commit.

## The bounds

Every loop that no human request starts has a bound. The bound of `challenged` is in the chain, and opens a wait on the design. These are the others, each counted in git at the moment the table gives.

| Bound | Counted | When |
| --- | --- | --- |
| Six verdicts on one work-package | the judge entries in its file | after a judge lands `challenged`, before you launch the coder |
| Three hardener reports at `holes` or `bug` on one work-package, the pass and two replays | the hardener entries at those statuses in its file | after a hardener lands one, before you launch the planner on it |
| Four waits on one work-package | the entries at `blocked` in its file, a `Pilot's go` aside | before you launch the recipient of a fifth |
| Three planner gap reports in one plan | the planner's entries at `gaps` in the epic file since the last barrier commit | before you launch the architect on a fourth |
| Three gaps you raise in one plan | your entries since the last barrier commit, or since your last `delivered` entry when it is later, whose first line reads `Gap sent back: gate`, `Gap sent back: conflict` or `Gap sent back: open NEED` | before you send a fourth |
| The pilot's go at each multiple of 120 role entries in the epic | the entries of the five roles across the epic folder, by the command below | before any launch or resume, once the count reaches or passes the next multiple above the one your last go names |

The role entries are the sum of the counts that `grep -rhcE '^## .* · (architect|planner|coder|judge|hardener) · ' .cairn/<id>/` prints, one per file.

A `PreToolUse` hook of `.claude/settings.json` holds this last bound. Before each launch of one of the five roles, and each resume of one by SendMessage, it makes the same count in your clone and refuses once the count reaches the next multiple above the one your last go names. It finds the epic by the `.cairn/<id>/` or the `EPIC-xxx` that the prompt or the message gives, and refuses one that gives neither. A launch or a resume it refuses has reached that bound.

A rejected observation, a change asked on the PR and a resume of the epic start from a human request, and count toward no bound.

At a bound, launch nothing more on that unit, or on the epic for the last three rows. Tell the pilot what was spent and what still fails, and land their answer in an entry of yours in the file the bound counts in, the epic file for the role entries:

- **Go on.** The entry keeps the status. Its first line reads `Pilot's go: <the bound>, at <the count>`, or `Pilot's go: 120 role entries, at <the multiple>` for the role entries, the multiple the pilot lets the epic pass, which the hook reads and which can be ahead of the count. Its body names the recipient the status sends to and the header of the entry it follows, so that the next run still finds its wait or its report. The bound counts again from this entry, or from the multiple it names for the role entries.
- **Accept the survivors**, at the bound of hardener reports. The entry lands `hardened`, with the pilot's decision as its reason.
