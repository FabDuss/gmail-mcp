# The intent

`.cairn/<id>/intent.md`, on the epic branch. The result of ideation: only what converged. Written in English, with the terms of the concept table. No verbatim: to judge a work-package, the NEEDs are in its brief, and the pilot's words are reached through the questions they cite.

## Sections, in order

- **Header**: epic, state (draft or confirmed), last invocation, date of confirmation, origin when the epic comes from a draft or a ticket (its reference, read as data), or from a reproduced defect (its reference FACT and its reproduction FACT).
- **Intention**: the destination in one sentence: for whom, which problem, which result.
- **Concepts**: one row per concept: the pilot's word, the concept in the code, status (aligned, gap to settle, new), and the fact or question behind it. Once a concept exists in the code, its name wins downstream.
- **Behavioural NEEDs**: identifier, title, scenario (given, when, then) written with the terms of the concept table, originating questions.
- **Constraint NEEDs**: the same, with a measurable threshold instead of a scenario.
- **Out of scope**: one line per subject, with the question where it was dismissed, and its draft if there is one.
- **Questions for the architect**: the "how" questions that surfaced and were parked.
- **Revisions**: per invocation, the NEEDs added or replaced.

## Rules

- A NEED whose expected result changes is marked replaced, and a new NEED is created. A rewording keeps the identifier.
- At most fifteen NEEDs. Beyond that, the epic must be split, and drafts are how to split it.

## Checks

- Every NEED cites at least one decided question.
- No NEED rests on an open question or on a subject reported as a draft.
- Every decided question at the end of a branch leads to a NEED, an out-of-scope line, a draft, or a question for the architect.
- Every term of a scenario is in the concept table.
- The fog is empty at confirmation.

## Warning signs

- No option rejected in the tree: the opening did not happen, and ideation converged from the first round.
- No draft and no out-of-scope line on a broad subject: everything was accepted. The fog triage exists to correct this.
- Many drafts: the destination is too vague, or the pilot is deferring the hard part. Check that the core did not leave with them.
- Many new concepts: the epic touches the domain model. Say so first in the questions for the architect.

## The draft epic

An epic with the status `draft`: `.cairn/<id>/intent.md` on its own branch `epic/<id>/main`, started from main, so it survives if the originating epic is abandoned. Its intent is the first part of an intent and nothing more: the header in draft state, the intention in one sentence, its origin, the originating epic's identifier with either the limit question that decided the deferral, when it comes from ideation, or the critique's proposal, when it comes from a closing review; and the IDs of the FACTs of the originating tree that concern it, never copied: the tree stays the only register. It holds no NEED and no decision.

- The session offers it; the pilot confirms or drops it. The decision is a question of the tree, of nature limit.
- It exists only for a subject that is not needed to reach and observe the destination.
- **One draft per subject.** Before offering a draft, read the open drafts: each epic at `draft` among the remote `epic/*` branches, with the intention of its `intent.md` and the entries of its epic file. A subject that an open draft carries, by a module it names or by its intention, is offered as an addition to that draft, never as a new draft. Subjects offered together that share a module go to one draft. A new draft is offered only for a subject that no open draft carries.
- **An addition** is an entry of the draft's epic file that keeps `draft`: the epic it comes from, the limit question or the proposal in words, and the IDs of the FACTs of that epic's tree that concern it, never copied. The draft's intention is reworded only when the pilot approves the new sentence.
- Every draft is reviewed at the close of each epic it comes from, by its origin or by an addition: the session recommends resuming, keeping or dropping it, and the pilot decides. Dropping is a normal outcome.
- Before its invocation 0, a resumed draft offers the pilot to fold into it the open drafts that share its modules or its subject. A folded draft lands `abandoned`, with an entry that names the draft taking it up and its own head, and the resumed draft gains an entry that names it.
- Resuming a draft is a full invocation 0: the destination is discussed again, the scouts start from the code as the previous epic left it, and inherited facts are checked again before use.
