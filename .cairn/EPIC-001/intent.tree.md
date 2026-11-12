# EPIC-001 tree

## Index

- Q-001 Destination: decided, option A, the three subjects of the draft.
- Q-002 Who uses the deployment: decided, the pilot alone.
- Q-003 Whether sending is enabled: decided, sending off.
- Q-004 Annotation of each tool: decided, option A, read-only on reads, non-destructive on undoable writes, destructive on the rest.
- Q-005 The revocation text: decided, option A, remove the claim from the three READMEs.
- Q-006 The advertised tool count: decided, option A, keep 27, say four need ALLOW_SEND, document ALLOW_SEND.
- Q-007 Sending refused while off: decided, option A, sending tools absent and every send refused while off.

## Fog

Empty. The stale test count (FACT-004, FACT-009) was not core to Q-001, and the pilot dropped it in round 2 with the answer "tout selon tes recos".

## Questions

### Q-001 Destination

- Depends on: FACT-001, FACT-002, FACT-003, FACT-004, FACT-005, FACT-006.
- Regime: opening.
- Nature: destination.
- Resolution: conversation.
- Question: for whom, which problem, which result. The draft's intention covers three subjects: tool annotations, the README revocation claim, and a send gate that fails closed. Options put to the pilot:
  - A. The three subjects, as the draft states them.
  - B. Only the send gate and the README, without annotations.
  - C. The three subjects, plus a server-side check on sending, such as a recipient allowlist or a confirmation asked by the server.
  - D. Only the README.
- Pilot's answer: "A"
- State: decided. B was rejected because it drops the annotations, D because it leaves the gate failing open, C because the pilot did not take the server-side check.
- Follows: the destination is the draft's intention as written in `intent.md`. Q-004, Q-005, Q-006 and Q-007.
- Invocation: 0.

### Q-002 Who uses the deployment

- Depends on: nothing.
- Regime: opening.
- Nature: need.
- Resolution: conversation.
- Question: does the pilot use this deployment alone, or do several Google accounts sign in to it? This decides whether per-account revocation matters to the README text and to the destination.
- Pilot's answer: "seul"
- State: decided.
- Follows: per-account revocation is not needed. Q-005.
- Invocation: 0.

### Q-003 Whether sending is enabled

- Depends on: nothing.
- Regime: opening.
- Nature: need.
- Resolution: conversation.
- Question: does the pilot's deployment set `ALLOW_SEND=true`, or does it run with sending off? With sending off, the four sending tools are not registered (FACT-002) and the risk the annotations address is small.
- Pilot's answer: "envoi coupé"
- State: decided.
- Follows: the default with sending off is the case the docs describe first. Q-006, Q-007.
- Invocation: 0.

### Q-004 Annotation of each tool

- Depends on: Q-001, FACT-001, FACT-007.
- Regime: converging.
- Nature: need.
- Resolution: conversation.
- Question: which annotation does each of the 27 tools carry? Options:
  - A. Read-only on the seven tools that only read (`whoami`, `search_messages`, `get_message`, `get_thread`, `list_labels`, `list_drafts`, `get_attachment`). Non-destructive on the writes that can be undone (`modify_labels`, `modify_thread_labels`, `batch_modify_messages`, `create_draft`, `update_draft`, `untrash_message`, `untrash_thread`, `create_label`, `update_label`, the three `stage_attachment_*`). Destructive, the protocol's default, on the rest (`send_message`, `send_draft`, `reply_all`, `forward_message`, `trash_message`, `trash_thread`, `delete_draft`, `delete_label`).
  - B. Read-only on the seven reads, and nothing else. Every other tool keeps the default, destructive.
  - C. Destructive marked explicitly on the sending tools only.
- Recommendation: A. By FACT-007 an unannotated tool already counts as destructive, so the annotations change what a client shows only by marking reads and undoable writes. A gives the client the most to go on. B is the smaller change if the pilot prefers it. C changes nothing a client sees, by FACT-007.
- Pilot's answer: "tout selon tes recos"
- State: decided. The rejected options lost for the reasons the recommendation gives.
- Follows: NEED-001, NEED-002, NEED-003.
- Invocation: 0.

### Q-005 The revocation text

- Depends on: Q-001, Q-002, FACT-005, FACT-006, FACT-008.
- Regime: converging.
- Nature: need.
- Resolution: conversation.
- Question: what replaces the claim that rotating the Google client secret invalidates every grant? Options:
  - A. Remove that claim from `README.md`, `README.ja.md` and `README.zh.md`, and keep the two paths that work.
  - B. Replace it with a way that does cut every grant at once.
- Recommendation: A. FACT-006 shows the claim is wrong, and with one user (Q-002) revoking at myaccount.google.com/connections cuts all access. B would need a new mechanism that the destination does not ask for.
- Pilot's answer: "tout selon tes recos"
- State: decided. The rejected options lost for the reasons the recommendation gives.
- Follows: NEED-007.
- Invocation: 0.

### Q-006 The advertised tool count

- Depends on: Q-001, Q-003, FACT-003, FACT-004, FACT-009.
- Regime: converging.
- Nature: need.
- Resolution: conversation.
- Question: what do the README badges, the translations and `docs/index.html` say about the number of tools? Options:
  - A. Keep 27, and state that four of them, the sending tools, exist only when `ALLOW_SEND=true`. Document `ALLOW_SEND` in the README.
  - B. Advertise 23, the default, and mention four more with `ALLOW_SEND=true`.
- Recommendation: A. The CI check counts the tools in the source and finds 27 (FACT-009), so A keeps that check meaningful, and the README gains the setting it lacks (FACT-003).
- Pilot's answer: "tout selon tes recos"
- State: decided. The rejected options lost for the reasons the recommendation gives.
- Follows: NEED-008.
- Invocation: 0.

### Q-007 Sending refused while off

- Depends on: Q-001, Q-003, FACT-002.
- Regime: converging.
- Nature: need.
- Resolution: conversation.
- Question: with sending off, what does the client see and what does the server do? Options:
  - A. As today, the four sending tools are absent from the client's list. In addition, the server refuses any call that would put mail on the wire, whatever tool makes it, unless `ALLOW_SEND` is `true`.
  - B. The four tools appear in the list and answer with a refusal.
- Recommendation: A. The client never offers a tool that cannot work, and a sending tool added later cannot slip past the gate (FACT-002).
- Pilot's answer: "tout selon tes recos"
- State: decided. The rejected options lost for the reasons the recommendation gives.
- Follows: NEED-004, NEED-005, NEED-006.
- Invocation: 0.

## Facts

### FACT-001 No tool carries MCP annotations

- Observed: `GmailMCP.init` registers 27 tools through `this.server.tool(`. None passes a `ToolAnnotations` argument, and no `readOnlyHint` or `destructiveHint` appears in `src/`. The SDK's `tool()` accepts annotations in two of its overloads.
- Source: `src/index.ts:816-1715`; `node_modules/@modelcontextprotocol/sdk/dist/esm/server/mcp.d.ts:141,146`, SDK 1.29.0.
- Commit: 84e427c.
- Certainty: read in the code.
- Used by: Q-001.

### FACT-002 The send gate wraps a deprecated registration method and matches names

- Observed: `SEND_TOOLS` holds `send_message`, `send_draft`, `reply_all` and `forward_message`. `init` replaces `this.server.tool` with a wrapper that drops those names unless `ALLOW_SEND` is exactly `"true"`. A sending tool absent from the set, or registered through `registerTool`, is not gated. The SDK marks overloads of `tool()` deprecated in favour of `registerTool`.
- Source: `src/index.ts:151`, `src/index.ts:806-814`; `mcp.d.ts:110`.
- Commit: 84e427c.
- Certainty: read in the code.
- Used by: Q-001, Q-003.

### FACT-003 ALLOW_SEND is undeclared

- Observed: `ALLOW_SEND` appears in no file among `README.md`, `wrangler.jsonc`, `worker-configuration.d.ts` and `docs/`. The code reads it through a cast.
- Source: `grep ALLOW_SEND` on those files; `src/index.ts:809`.
- Commit: 84e427c.
- Certainty: searched.
- Used by: Q-001.

### FACT-004 The docs advertise 27 tools and 253 tests

- Observed: the tool badge says 27 in `README.md:15`, `README.ja.md:7` and `README.zh.md:7`, and `docs/index.html:117` says 27 tools. A deployment without `ALLOW_SEND=true` serves 23. `scripts/check-counts.py` counts `this.server.tool(` in the source, so it counts 27 whatever the setting. The test badge and `README.md:292,312` say 253, while `bun test` ran 255 tests.
- Source: those lines; `scripts/check-counts.py:33-34`; the security audit's run of `bun test` on 2026-11-12.
- Commit: 84e427c.
- Certainty: read in the files; the 255 comes from the audit's report.
- Used by: Q-001, fog.

### FACT-005 The README revocation claim

- Observed: `README.md:284` lists three ways to withdraw access: narrowing `ALLOWED_EMAILS` stops new sign-ins, revoking at myaccount.google.com/connections revokes one account, and "Rotating the Google client secret invalidates every grant at once".
- Source: `README.md:284`.
- Commit: 84e427c.
- Certainty: read in the file.
- Used by: Q-001.

### FACT-006 Rotating the Google client secret does not revoke refresh tokens

- Observed: Google's rotation adds a new secret and lets the operator disable the old one, after which OAuth flows reject the old secret. Google's docs say a refresh token stays valid until the user revokes access or the token expires, and name no other cause. A refresh request sends the client secret with the refresh token. So once the operator sets the new secret on the Worker, the grants already stored keep refreshing. No Google page states this outright.
- Source: https://support.google.com/cloud/answer/15549257 and https://developers.google.com/identity/protocols/oauth2.
- Date: read 2026-11-12.
- Certainty: inferred from the two pages.
- Used by: Q-001.

### FACT-007 MCP annotation defaults

- Observed: in the MCP SDK's tool annotations, `readOnlyHint` defaults to false and `destructiveHint` defaults to true, meaningful only when the tool is not read-only. A tool with no annotations therefore counts as one that may destroy data.
- Source: `node_modules/@modelcontextprotocol/sdk/dist/esm/types.js:1178-1199`, SDK 1.29.0.
- Commit: 84e427c.
- Certainty: read in the SDK.
- Used by: Q-004.

### FACT-008 The translations repeat the revocation claim

- Observed: `README.ja.md:268` and `README.zh.md:268` say, as `README.md:284` does, that regenerating the Google client secret invalidates every grant at once.
- Source: those lines.
- Commit: 84e427c.
- Certainty: read in the files.
- Used by: Q-005.

### FACT-009 CI checks the advertised counts against the source

- Observed: the CI step "Tool and test counts match the source" runs `scripts/check-counts.py`, which compares the tool badges to the number of `this.server.tool(` calls in `src/index.ts` and the test claims to the count `bun test` reports.
- Source: `.github/workflows/ci.yml:39-40`; `scripts/check-counts.py:25-40`.
- Commit: 84e427c.
- Certainty: read in the files.
- Used by: Q-006, fog.

## Invocations

### Invocation 0

- Trigger: the pilot resumed draft EPIC-001 on 2026-11-12.
- Who: session, with the pilot.
- Questions created: Q-001, Q-002, Q-003 in round 1; Q-004, Q-005, Q-006, Q-007 in round 2.
- NEEDs touched: NEED-001 to NEED-008 created.
- Drafts: none created or added to. No other open draft existed to fold in.
- Confirmation: the pilot confirmed the intent with "je confirme, go".
