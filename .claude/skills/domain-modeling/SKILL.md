---
name: domain-modeling
description: Build and sharpen the domain model. Use when resolving a domain term, writing or editing CONTEXT.md, or writing an ADR.
---

# Domain Modeling

Actively build and sharpen the project's domain model as you design: challenge terms, invent edge-case scenarios, and write the glossary and decisions down the moment they crystallise. Reading `CONTEXT.md` for vocabulary is not this skill; every role does that. This skill is for changing the model.

Only the architect writes `CONTEXT.md` and the ADRs, each ADR from a DEC; design runs go one at a time, so there is never a second writer. During ideation, a term settled with the pilot becomes a question of nature concept, in the state decided, whose verbatim gives the `_Pilot_` line; at its next run, the architect carries the decided concept questions into `CONTEXT.md`. Every other role reads them.

## File structure

Most repos have a single context:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── EPIC-001-01-event-sourced-orders.md
│       └── EPIC-001-02-postgres-for-write-model.md
└── src/
```

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts. The map points to where each one lives:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                          ← system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/                 ← context-specific decisions
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Create files lazily: only when you have something to write. If no `CONTEXT.md` exists, the architect creates one when it carries the first resolved term. If no `docs/adr/` exists, create it when the first ADR is needed.

## Where it lands

| What you learned | Where it goes |
|---|---|
| The meaning of a domain term | `CONTEXT.md`, in the format of [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md) |
| The pilot's own word for that term | the `_Pilot_` line of the term, in the pilot's language |
| The pilot's exact answer | the tree, as verbatim |
| A decision for this epic | a DEC in `design.md` |
| A decision that is hard to reverse, surprising, and a real trade-off | an ADR that cites its DEC and its epic |
| How something is implemented | the code and `design.md`, never `CONTEXT.md` |
| A question still open | a Q in the tree |

## During ideation, with the pilot

### Challenge against the glossary

When the pilot uses a term that conflicts with the existing language in `CONTEXT.md`, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y. Which is it?"

### Sharpen fuzzy language

When the pilot uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account': do you mean the Customer or the User? Those are different things."

### Discuss concrete scenarios

When domain relationships are being discussed, stress-test them with two or three specific scenarios per definition. Invent scenarios that probe edge cases and force the pilot to be precise about the boundaries between concepts.

### Cross-reference with code

When the pilot states how something works, send a scout to check whether the code agrees. If you find a contradiction, surface it: "Your code cancels entire Orders, but you just said partial cancellation is possible. Which is right?"

### Look for a parent before creating

Before adding a term, look in `CONTEXT.md` for a parent or neighbouring concept. Refine or extend it rather than create a near-duplicate.

### Record each term as it is resolved

When a term is resolved, record it right there. Don't batch these up: capture them as they happen. During ideation, the session writes it as a question of nature concept, decided, with the pilot's words. As architect, you write it in `CONTEXT.md`, in the format of [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

`CONTEXT.md` should be totally devoid of implementation details. Do not treat `CONTEXT.md` as a spec, a scratch pad, or a repository for implementation decisions. It is a glossary and nothing else.

## As architect

You run unattended and cannot ask the pilot. When the domain moves, stop before writing any TASK: put the change, with two or three concrete scenarios for each definition, in your `blocked` entry in the epic file, addressed to the pilot, since a stopped run lands only its entry. The session takes the change to the pilot; once it is settled, you write the Domain section of `design.md` and update `CONTEXT.md`. Check claims about the code through a scout, as in ideation.

## ADRs

Write an ADR, do not offer one, when a DEC meets all three criteria:

1. **Hard to reverse**: the cost of changing your mind later is meaningful
2. **Surprising without context**: a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off**: there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, the DEC in `design.md` is enough. The ADR cites its DEC and its epic. Use the format in [ADR-FORMAT.md](./ADR-FORMAT.md).
