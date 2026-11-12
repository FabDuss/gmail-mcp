# The tree

`.cairn/<id>/intent.tree.md`, on the epic branch. The memory of ideation: everything explored, dismissed and decided, and why. Written in English, except the pilot's verbatim answers, which stay in the pilot's language.

## Sections, in order

- **Index**, at the top, kept by the session, in the same write as the question: one line per question, with its title, state and a summary of the decision. A re-invocation reads the index, then opens only the entries it needs.
- **Fog**: the areas sensed but not yet phrased, written as freely as the view allows, coarser than a question. An area becomes one, several or no question, goes to a draft, or is dropped; it then leaves the section.
- **Questions**, one entry each.
- **Facts**, one entry each.
- **Invocations**, one header each.

## A question entry

- Identifier and short title.
- Depends on: the questions and facts it needs. This decides what can be asked now; a fact not found yet blocks only what depends on it.
- Regime: opening or converging.
- Nature: destination, need, possibility, concept, limit.
- Resolution: conversation, scout, prototype.
- The question and the options considered, with the reason each rejected option was rejected.
- Your recommendation, in converging only.
- The pilot's answer, verbatim. The verbatim lives here and nowhere else.
- State: open, decided, out of scope, reported as a draft, reopened.
- What follows from it: the NEEDs, or the draft created or added to.
- The invocation number.

## A fact entry

A fact entry follows the facts skill: identifier, title, what was observed, its source, the commit or date, the certainty, and what uses it. During design and planning, the architect and then the planner write their own FACTs here: one writer per phase.

## An invocation header

What triggered it, who, which questions it created or reopened, which NEEDs it touched, which drafts it created or added to.

## Rules

- A question is never deleted. No identifier is renumbered or reused.
- Links: a NEED points to its questions; a reopened question points to its NEEDs; a question depends on questions; a question points to its FACTs; an out-of-scope line or a draft points to the question that settled it.
- One file per epic means one invocation at a time.
