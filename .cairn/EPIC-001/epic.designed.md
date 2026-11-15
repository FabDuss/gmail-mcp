# EPIC-001 Send safeguards and revocation docs

## 2026-11-12T10:21Z · session · draft

Draft opened at the pilot's request after the whole-repository security audit and architecture critique at main 84e427c. The pilot kept three subjects, tool annotations, the README revocation claim and the send gate that fails open, and rejected the other proposals. `intent.md` carries the three subjects. No open draft carried them.

## 2026-11-12T11:07Z · session · ideation

The pilot resumed this draft. Main was already in the epic branch at 84e427c. No other open draft existed to fold in. Invocation 0 of ideation starts, with Q-001, Q-002 and Q-003 in `intent.tree.md`.

## 2026-11-12T11:14Z · session · confirmed

The pilot confirmed the intent at invocation 0, with NEED-001 to NEED-008 and four questions for the architect. Frontier and fog are empty.

## 2026-11-15T12:27Z · session · confirmed

Run lost: architect

The architect run launched on 2026-11-12 stopped on an API usage limit before it landed anything. Its worktree keeps `design.md`, `CONTEXT.md` and tree changes in progress. The session resumes it once.

## 2026-11-15T12:28Z · architect · designed

First design after ideation. `design.md` holds five TASKs: TASK-001 and TASK-002 carry the core, the tool list by kind and the refusal of sending on the Gmail request path, and TASK-003 to TASK-005 are mechanical. The design is short because the epic changes two places in one module, plus documentation. `CONTEXT.md` is new and carries the concepts of the intent. No ADR: no DEC is hard to reverse. FACT-010 to FACT-019 are in the tree.

A rearrangement seen that spares no TASK: the session agent's start holds all 27 tool handlers inline in one long method. Moving them into modules by kind would make each tool easier to find, but it removes no TASK of this epic, so it is for the closing review.
