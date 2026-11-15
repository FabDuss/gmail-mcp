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

## 2026-11-15T12:34Z · planner · planned

PLAN-001 Send safeguards and revocation docs, the epic's only plan, with its sketch landed. `plan.md` carries the order: WP-001 opens on the core, TASK-001 with TASK-003; WP-002, TASK-002, follows its validation; WP-003, TASK-004 with TASK-005, trivial, starts alongside WP-001. The plan closes NEED-001 to NEED-008 and leaves no TASK for a next plan.

The sketch adds `ToolKind`, `GmailMCP.sendingIsOn` and `GmailMCP.registerByKind` in the session agent, and `isSendingPath` in the Gmail module, all with empty bodies and wired to nothing. It pulls in only two types of the SDK already installed, as the Impact announced. FACT-020 shows the registration method keeps each handler's argument types, the design's most uncertain point; FACT-021 explains why the two methods are `protected`.

Gates on the sketch: `bun run check` passes, with the six complexity warnings that predate the epic; `bun test` ran 255 tests, 255 pass; `scripts/check-counts.py` prints no failing line about tools, and fails its test count lines, 253 claimed against 255, the known red of FACT-004.

## 2026-11-15T13:07Z · session · planned

Architecture critique of the epic's diff at 739a49d, closing review of PLAN-001-send-safeguards. Its proposals, without its Rejected section:

### Make the session agent's handler tests go through the in-process MCP client instead of the hand-written fake of `registerTool`

- Modules: the session agent `GmailMCP` and its handler test harness `makeAgent` / `tool(handlers, …)` in `test/handlers.test.ts`; the seam between `registerByKind` and the SDK's `McpServer`.
- Friction: the harness replaces `McpServer` with a fake that keeps only the handler (`test/handlers.test.ts:60-63` drops `_config`), so the 43 handler tests never touch a tool's input schema, its defaults, its description or its annotations. WP-001's mutation pass (78f2e6e) killed no test when the schema of all 27 tools was replaced with `{}`. `test/handlers.test.ts:215` passes by hand the `maxResults: 10` default that the schema sets at `src/index.ts:867`. The fake also copies the SDK's call form, so the diff rewrote it when production moved to `registerTool`. A real `McpServer` over `InMemoryTransport` already runs in the suite (`connectClient`, `test/handlers.test.ts:1282-1289`).
- Deepening: the handler tests call tools as a client does, through the in-process client against a real `McpServer`, keeping the existing stand-ins for storage, props, env and Gmail's `fetch`. The fake and the direct handler lookup go.
- Gives: each tool's schema, defaults, description, annotations and handler are checked by the test of its behaviour, and the tests stop moving with the SDK's registration call.

Beside it, for the closing review:

- The architect's rearrangement in its `designed` entry: move the 27 tool handlers out of `GmailMCP.init` into modules by kind.
- The test-count claims of the three READMEs, out of scope since round 2 of ideation and known red in `baseline.md` (253 claimed, 275 run at 739a49d). The CI step "Tool and test counts match the source" fails on them.

## 2026-11-15T14:13Z · session · closing

Barrier of PLAN-001-send-safeguards, the last barrier: `plan.md` leaves no TASK. Every work-package is hardened. Every NEED of the intent is closed by a validated work-package. Main was already in the epic branch. Gates at 739a49d, the full suite: `bun run check` exit 0, `bun test` 275 pass 0 fail, `scripts/check-counts.py` failing only on the test-count claims known red in `baseline.md`.

Observed at 739a49d.

- NEED-001 Read tools are marked read-only: not observed; it needs a Google sign-in this environment cannot complete (FACT-023). The pilot accepted it on the gates without reporting an observation. FACT-023
- NEED-002 Undoable writes are marked non-destructive: not observed, accepted by the pilot on the gates. FACT-023
- NEED-003 Destructive tools stay destructive: not observed, accepted by the pilot on the gates. FACT-023
- NEED-004 Sending tools are absent while sending is off: not observed, accepted by the pilot on the gates. FACT-023
- NEED-005 Sending is refused while sending is off: observable by nobody, judged on the gates; with sending off no user path reaches a sending request. FACT-023
- NEED-006 Sending works when enabled: observable by nobody here, judged on the gates; the pilot keeps sending off. FACT-023
- NEED-007 The README gives only revocation paths that work: holds, observed by the session. FACT-025
- NEED-008 The docs state the tool count and the setting that gates sending: holds, observed by the session. FACT-024, FACT-026

The pilot accepted the observation and the session's closing recommendations with "J accepte tes recommandations". Closing decisions:

- Handler tests through the in-process MCP client (the critique's proposal): rejected.
- Moving the 27 tool handlers out of `GmailMCP.init` by kind (the architect's rearrangement): rejected.
- The test-count claims of the three READMEs: fixed in this epic. The session sends it to the architect by `confirmed`, after a re-invocation of the intent, since the intent had it out of scope.

## 2026-11-15T14:15Z · session · confirmed

Invocation 1 of ideation, from the pilot's closing decision to fix the test-count claims in this epic, as a change asked on the PR. Q-008 decided, NEED-009 created, and the out-of-scope line on the test count removed. The known red of `baseline.md` on those claims is now the epic's work. The architect serves NEED-009.
