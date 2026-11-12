# EPIC-001 tree

## Index

- Q-001 Destination: open.
- Q-002 Who uses the deployment: open.
- Q-003 Whether sending is enabled: open.

## Fog

- Which tools count as changing or destroying mail for an annotation: send, forward and reply, then trash, label changes, batch label changes, draft deletion and label deletion. Depends on Q-001.
- Other stale counts in the docs. The test badge says 253 while the suite ran 255 at 84e427c (FACT-004). Not part of the three subjects of the draft.
- The translated READMEs (`README.ja.md`, `README.zh.md`) and `docs/index.html` repeat the tool count (FACT-004). Whether they also carry the revocation paths has not been checked.

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
- State: open.
- Invocation: 0.

### Q-002 Who uses the deployment

- Depends on: nothing.
- Regime: opening.
- Nature: need.
- Resolution: conversation.
- Question: does the pilot use this deployment alone, or do several Google accounts sign in to it? This decides whether per-account revocation matters to the README text and to the destination.
- State: open.
- Invocation: 0.

### Q-003 Whether sending is enabled

- Depends on: nothing.
- Regime: opening.
- Nature: need.
- Resolution: conversation.
- Question: does the pilot's deployment set `ALLOW_SEND=true`, or does it run with sending off? With sending off, the four sending tools are not registered (FACT-002) and the risk the annotations address is small.
- State: open.
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

## Invocations

### Invocation 0

- Trigger: the pilot resumed draft EPIC-001 on 2026-11-12.
- Who: session, with the pilot.
- Questions created: Q-001, Q-002, Q-003.
- NEEDs touched: none yet.
- Drafts: none created or added to. No other open draft existed to fold in.
