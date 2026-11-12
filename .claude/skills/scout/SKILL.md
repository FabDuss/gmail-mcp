---
name: scout
description: Get facts about existing code or outside sources, never opinions, from Claude Code's built-in Explore agent. Use when you need a fact you do not have.
---

Get facts: what exists, where, as of which commit or date. The scout says what is; you decide what to do with it.

## Who searches

Claude Code's built-in Explore agent (`subagent_type: Explore`), whether you are the session or the architect. It is read-only, has no Agent tool, so it cannot hand the search on, and loads no `CLAUDE.md`. One call, one answer: no branch, no state, no relay, and a context thrown away after the answer, so yours stays light.

Launch it on Sonnet (`model: "sonnet"`); without the model, Explore runs on the session's own model. A role launches it in the foreground (`run_in_background: false`), all its searches in one message: the call waits, and each answer comes back to you in a separate message before your next turn. Launched in the background, the answer would go to the session if your turn ended first. The session launches it in the background, and its completion notification wakes it.

## What you write

- **One question about what exists**, never about the change. A question that names future behaviour leaks the ticket.
- **A scope**: a subsystem, an entry point or a starting symbol.
- **A budget**, in tool calls or time. Returning what was found is normal; "not found" is a fact.
- **The template**, chosen by what you are about to decide:
  - [`HOW.md`](HOW.md), by default: how the code behaves, traced from an entry point to its effect.
  - [`WHY.md`](WHY.md), before a lever or a change to existing code: why the code has this shape.
  - [`WEB.md`](WEB.md), for a library or an exposed surface: documentation, versions, known pitfalls, security.

Explore receives the question, the scope, the budget and the template, filled in, and nothing else: no intent, no `design.md`, no brief, none of your conversation. It cannot form an opinion on a change it does not know about.

## Size it

- A narrow question gets one Explore and a small budget. If one call of your own answers it, spawn nothing.
- A broad subsystem gets two to four Explore on disjoint angles (data, execution path, configuration), launched in one message, then one synthesis of their facts.
- Go broad first, precise second. The known failures: dozens of searches for a simple question, and endless searches for sources that do not exist.

## What comes back

A list of facts, and nothing else. Each fact gives the finding, where it was found, the state of the code it was read at (the commit) or the date of its source, its certainty, and what was not found. At most a few thousand tokens. Send back any answer that recommends, judges or proposes.

Facts come back unnumbered. You write the ones you keep into the tree yourself, each with its FACT identifier: one writer per phase. A fact whose code has moved since is scouted again, never corrected from memory.

## What you keep

- The decision. The scout reports; you decide.
- Any judgement on the design, which belongs to the architect.
- The registry: never scout a fact already in the tree unless its code has moved.

## Done when

Every fact you keep is in the tree, with its source, its code state and its certainty.
