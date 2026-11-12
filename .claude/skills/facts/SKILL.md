---
name: facts
description: How to write a FACT entry in an epic's tree. Use whenever you record a fact: the session during ideation and at a barrier, the architect during design, the planner during planning.
---

# Facts

A FACT is something observed, never decided: what exists, where, as of which commit or date. Facts live in the Facts section of `.cairn/<id>/intent.tree.md`, on the epic branch. The tree is their only register. During ideation and at a barrier the session writes them; during design the architect, then during planning the planner: one writer per phase.

## A fact entry

- **Identifier and title**: `FACT-xxx`, the next number in the tree. An identifier is never renumbered or reused.
- **Observed**: what is, stated without opinion.
- **Source**: a place in the code, path and symbol, or an external reference.
- **Commit or date**: the commit the code was read at, or the date of the external source.
- **Certainty**: as the search reported it.
- **Used by**: the questions, TASKs or Impact lines that rely on it.

## Rules

- A fact whose code has moved since its commit is searched again before anyone relies on it, never corrected from memory.
- A fact is never edited to say something else. A new fact supersedes it and points to it.
