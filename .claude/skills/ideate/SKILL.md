---
name: ideate
description: Ideation for an epic, with the pilot. Use when opening an epic, resuming a draft, or reopening an intent.
---

Interview the pilot until the need behind an epic is shared, and write it down. The tree remembers everything explored, dismissed and decided; the intent keeps only what converged. Only the session runs this skill, since only the session talks to the pilot.

Work a **design tree** in **rounds**. The **frontier** is every question whose prerequisites are settled: the questions you can ask now without guessing at answers you have not heard yet. Ask the whole frontier in one round, numbered, then wait for the pilot's answers. A question that depends on another question still open in this round belongs to a later round. Each round reshapes the tree: recompute the frontier and ask again.

## Two regimes

- **Opening rounds** explore the need and the forms an answer could take, as the pilot sees them. They list several options and carry no recommendation.
- **Converging rounds** carry your recommended answer, backed by FACTs.

Format a round like so. In an opening round, leave out the ➡️ line.

    ❓ **Q-012** - **<title>**: <question, with its options>

    ➡️ <recommended answer, citing its FACTs>

    ---

## Steps

1. **Name the destination.** Q-001 is the destination, settled before anything else, except for a reproduced defect below: for whom, which problem, which result. It scopes the epic and is the yardstick for every triage.
2. **Open.** Explore the need and the possible forms of the answer. What you sense but cannot phrase yet goes to the fog.
3. **Scout, in parallel.** Facts are your job, never the pilot's. Use scout to learn the current behaviour, how the domain is modelled, the parent and neighbouring concepts, and outside sources when needed. Each finding becomes a FACT in the tree. A running scout blocks only the questions that depend on it; ask the rest of the frontier now.
4. **Match words to concepts.** Hold the pilot's words against the code's concepts: aligned, a gap to settle, or a new concept. Record each in the concept table of the intent; changes to `CONTEXT.md` follow domain-modeling.
5. **Triage the fog**, every round. Classify each area against the destination: core or not. Dig into the core. For the rest, offer the pilot three outcomes: treat it now, report it as a draft epic, or drop it. A draft goes first to the open draft that carries the subject, as an addition, as INTENT-FORMAT says. The size of an area never makes a draft by itself.
6. **Converge.** Recommend, backed by FACTs. When a question is about look or behaviour, build a rough prototype for the pilot to react to, with prototype.
7. **Land each round.** At the end of every round, land the tree, and the intent if it changed, in one commit with `[skip ci]`, as the commit skill says for the session. An interruption then loses nothing, and the cloud's Stop hook finds no untracked file.
8. **Finish** when no question is open, the fog is empty and the pilot confirms. Land the intent and the tree a last time, with an entry in the epic file: the epic goes to `confirmed`.

Park every "how" question that surfaces in the intent's questions for the architect; never put one to the pilot. Never answer for the pilot, and never change the intent without the pilot. When the pilot cannot answer live, send the open questions with to-questionnaire.

## A reproduced defect

When the pilot reports a defect, a behaviour that departs from one already written down, establish two FACTs before the first round, by scout and by running the code:

- **The reference**: where the expected behaviour is written, such as a test, a doc, a rule of `CONTEXT.md` or an ADR, a NEED of a closed epic on main, or a commit where it held. Its FACT gives the source and its date, the date of the commit that wrote it, so that the pilot sees how old it is and whether something has replaced it since.
- **The reproduction**: the defect shown at the head of `origin/main`, with the certainty ran or observed, by a throwaway script under `/tmp` that calls the real code, or by driving the app as the session's observation does. Its FACT quotes the whole script, or the steps of the drive, so that the coder and the observer can replay it. Remove the script and stop what you started before your turn ends.

Then put one round to the pilot: Q-001 with your recommendation to restore the reference, and the option to change the reference instead; NEED-001, whose scenario is the reproduction with the reference's result; the fog, triaged; and the intent as it will stand, citing both FACTs with their dates. The pilot confirms that intent in their answer, or amends it. Never propose the fix: how goes to the questions for the architect.

Go back to the rounds above when a FACT is missing, as for a crash that no document decides, when the reproduction cannot run, as when it needs production data, or when the pilot chooses to change the reference or amends the destination.

## Files

- The tree: [TREE-FORMAT.md](TREE-FORMAT.md).
- The intent and the draft epic: [INTENT-FORMAT.md](INTENT-FORMAT.md).

## Re-invocation

A re-invocation reopens a confirmed intent. It starts from:

- the pilot, with a new need or a change;
- a wait on the intent, from the architect;
- an observation the pilot rejects at the barrier, or a change asked in review, when the gap is about intent.

Read the tree's index, then reopen only the touched branch: the reopened question and the questions that depend on it. List the NEEDs it touches; they point to the TASKs, work-packages and tests affected, and the rest goes on. When the pilot confirms, land the new invocation with an entry in the epic file: the epic goes back to `confirmed`, and the role that waited goes on.

## Done when

- The frontier is empty, the fog is empty, and the pilot has confirmed.
- Every check in INTENT-FORMAT.md holds.
