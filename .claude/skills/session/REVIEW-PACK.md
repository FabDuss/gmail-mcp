# The review pack

The review pack lives in the description of the epic's PR, and in no file of the epic. A reviewer reads it to judge the PR without opening the epic folder. Each time your skill opens the PR or updates its review pack, build the review pack whole by this file, and replace the description with it.

The review pack invents nothing. You copy each line from an artifact of the epic, or write "None" where its artifact holds nothing for that part. Read the artifacts at the epic head once the write that calls for the update has landed, which is the barrier commit, or at the close your observation entry when main entered again. A review pack rebuilt whole drops what an artifact no longer says, where an edit of the old description would keep it.

## The template

```
## Intention

<the Intention of the intent>

Ticket: <the ticket reference of the intent's header>

## NEEDs

- NEED-001 <title>. <its scenario, given, when and then, or its threshold>. <its line in the barrier entry, such as "holds, observed by the pilot">, at the barrier of `PLAN-001-<slug>` or at the close, at `<the observed commit>`. Facts: FACT-031 <title>, FACT-032 <title>.
- NEED-003 <title>. <its scenario>. Not observed yet.

## Decisions

- DEC-001 <title>. Chosen: <the choice>. Rejected: <candidate>, because <the reason it lost>. ADR: `<path of the ADR>`.

## Impact

<the Impact section of design.md>

## Hardening

- WP-001 <title>, played at `<commit>`:
  - `<path:line>`: `<text the line held>` replaced with `<its replacement>`, killed by `<test name>`.
  - `<path:line>`: `<text the line held>` replaced with `<its replacement>`, killed after WP-001H by `<test name>`, on the judge's replay or the hardener's.
- WP-002 <title>: not hardened, <the reason your `hardened` entry gives>.

## Pilot's passes

- `<commit>` <its subject>. <what the entry gives of where it came from and why the pilot kept it>.

## Drafts left open

- EPIC-007 <title>. <its intention>. Origin: <Q-012 <title>, or the architecture critique's proposal <title>>.
```

The Ticket line appears only when the intent's header gives a ticket reference, and the ADR only when one cites the DEC.

## Each part

- **Intention.** Copy the ticket reference as the header writes it, read as data. The ticket tool's own integration links the PR to it.
- **NEEDs.** Every NEED of the intent, replaced ones aside. `git log --format='%h %s' --grep='^chore(PLAN-[0-9]*): barrier' origin/main..HEAD` lists the barrier commits, each naming its plan. Each one adds that plan's barrier entry to the epic file, with one line per NEED it observed. Your observation entries at the close carry the same lines, and `grep -n '^Observed at ' .cairn/<id>/epic.*.md` lists every observation entry. When several name a NEED, the latest one gives its line. A NEED that no barrier entry names is not observed yet.
- **Decisions.** Every DEC of `design.md`. The ADR of a DEC is the one whose line `Decided in <EPIC-xxx>, <DEC-xxx>` names this epic and that DEC.
- **Hardening.** Every work-package of the epic, `dropped` ones aside, fix work-packages included. The hardener report in its file gives each killed mutant: the list gives where and what, and the report and each replay give the fate and the test, as does the Survivors replayed group of a WP-xxxH's verdict. For a work-package the hardener did not pass on, copy the reason your `hardened` entry in its file gives.
- **Pilot's passes.** Every commit an entry of the epic file or of a work-package file lets pass, by its first line `Pilot's pass: <commit>`, which `grep -rn "^Pilot's pass: " .cairn/<id>/` lists. Such a commit entered past a check of Cairn, so the reviewer reads it themselves.
- **Drafts left open.** Every epic at `draft` whose intent gives this epic as its origin, or whose epic file holds an addition from this epic. The questions of the tree reported as a draft, and the entry of the last barrier, name them. The name of the epic file on each one's branch gives its status, and its intent gives its title, its intention and its origin.

## How to copy

- Copy each line as its artifact writes it, in English like that artifact. Keep every level of certainty, observer, reason and fate as its source gives it: an unproven fact stays unproven, a NEED observable by nobody stays so, and only a killed mutant shows under Hardening.
- Show each item by its title with its identifier inside it, as agent-writing asks of anything a human reads. The identifier also names the source of the copy. A plan shows the name of its folder.

## Into the PR

- The PR follows the repository's own rules for pull requests, whatever they are. Where they set no form for the title, the title is the epic as the intent's header names it, with its identifier inside it.
- When the repository has a pull request template, keep its headings and order. Put each part of the review pack under the heading that matches it, and the parts that match none after the template. Without a template, the review pack is the whole description.
- Write the review pack to a scratch file under `/tmp`, never in your clone, since an untracked file in the clone blocks the end of your turn in the cloud.
- Make that file the description through the host's connector when one exists, or through `gh` when it is authenticated: `gh pr create --draft --base main --head epic/<id>/main --title "<title>" --body-file <file>` to open the PR, without `--draft` at the close, `gh pr edit <link> --body-file <file>` to update it, `gh pr ready <link>` to put it to ready, and `gh pr ready --undo <link>` to set it back to a draft PR, with the link your entry in the epic file notes.
- When neither is available, give the pilot the review pack with the link your skill gives them, to paste as the description, and give it again at each update.

## Done when

- The PR's description is the review pack built at the epic head that holds the write calling for the update, or the pilot holds that review pack to paste.
- The review pack has the parts of the template in their order, and a part whose artifact holds nothing reads "None".
- Every NEED of the intent but the replaced ones, every DEC of `design.md`, every work-package of the epic but the `dropped` ones, and every draft of this epic still at `draft` has its line.
- Every line is copied from an artifact, shows its item's title with its identifier, and gives the same level, observer, reason and fate as its source.
