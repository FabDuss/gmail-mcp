# EPIC-001 tree

## Index

- Q-001 Destination: decided, option A, the three subjects of the draft.
- Q-002 Who uses the deployment: decided, the pilot alone.
- Q-003 Whether sending is enabled: decided, sending off.
- Q-004 Annotation of each tool: decided, option A, read-only on reads, non-destructive on undoable writes, destructive on the rest.
- Q-005 The revocation text: decided, option A, remove the claim from the three READMEs.
- Q-006 The advertised tool count: decided, option A, keep 27, say four need ALLOW_SEND, document ALLOW_SEND.
- Q-007 Sending refused while off: decided, option A, sending tools absent and every send refused while off.
- Q-008 The advertised test count: decided at the closing review, fix it in this epic.

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

### Q-008 The advertised test count

- Depends on: FACT-004, FACT-009, FACT-023.
- Regime: converging.
- Nature: need.
- Resolution: conversation.
- Question: the fog item dropped in round 2 came back at the closing review of PLAN-001. The three READMEs claim 253 tests while the suite runs 275 at 739a49d, and the CI step "Tool and test counts match the source" fails on those claims, so the epic's PR would show it red. Options: A. fix the claims in this epic; B. leave them, as round 2 decided.
- Recommendation: A. It is one run of `scripts/check-counts.py --fix` on files the epic already changes, and it keeps the PR's CI green.
- Pilot's answer: "J accepte tes recommandations"
- State: decided. B was rejected because the PR would carry a red CI step.
- Follows: NEED-009.
- Invocation: 1.

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

### FACT-010 registerTool takes the annotations and the list hands them to the client unchanged

- Observed: `McpServer.registerTool(name, config, cb)` reads `title`, `description`, `inputSchema`, `outputSchema`, `annotations` and `_meta` from `config`. It builds the tool through the same internal function as the deprecated `tool()` overloads, so the call path, the duplicate-name error and the callback arguments are the same. `inputSchema` accepts a raw shape of Zod 4 schemas, and an empty shape gives the callback `(args, extra)`, as `tool()` does today. The `tools/list` handler returns each tool's `annotations` as given, without filling defaults. A throwaway script that registered three tools with `readOnlyHint` and `destructiveHint`, connected an SDK `Client` through the SDK's `InMemoryTransport`, and called `listTools()` printed each tool's annotations exactly as registered.
- Source: `node_modules/@modelcontextprotocol/sdk/dist/esm/server/mcp.js:56-99,605-651,699-705`, `mcp.d.ts:150-157`, SDK 1.29.0; the script `/tmp/EPIC-001-architect-proof/annotations.ts`, run with bun 1.4.4.
- Commit: dd97a37.
- Certainty: read in the SDK, and ran.
- Used by: DEC-002, TASK-001, Impact.

### FACT-011 A tool handler that throws reaches the client as an error result

- Observed: the SDK's `tools/call` handler catches any error from the handler and returns `{ content: [{ type: "text", text: <message> }], isError: true }`, not a JSON-RPC error. The same script's call to a handler that throws returned that result.
- Source: `node_modules/@modelcontextprotocol/sdk/dist/esm/server/mcp.js:100-149`, SDK 1.29.0; `/tmp/EPIC-001-architect-proof/annotations.ts`.
- Commit: dd97a37.
- Certainty: read in the SDK, and ran.
- Used by: TASK-002.

### FACT-012 The agent framework leaves tool registration to the SDK

- Observed: `McpAgent` in `agents` 0.17.4 awaits the subclass's `init()`, then connects `this.server` to its transport. It never calls, wraps or replaces `server.tool` or `server.registerTool`, and it installs no `tools/list` or `tools/call` handler.
- Source: `node_modules/agents/dist/mcp/index.js`, `onStart` near lines 1500-1510; greps for `registerTool`, `.tool(`, `_registeredTools` and `setRequestHandler` in `agents/dist`.
- Commit: dd97a37.
- Certainty: read in the code, from targeted searches.
- Used by: TASK-001, Impact.

### FACT-013 Every Gmail request goes through GmailMCP.api

- Observed: `gmailFetch` is the only code that fetches Gmail, and it prefixes every path with `https://gmail.googleapis.com/gmail/v1/users/me`. Its three calls are all inside `GmailMCP.api`, which runs `assertSessionOwner`, then `assertWithinRate`, then `token()`, then `gmailFetch`. A 401 retries after a forced token refresh. Other Google calls in `src/utils.ts` go to the OAuth token and userinfo endpoints only.
- Source: `src/gmail.ts:6,66-82`; `src/index.ts:332-355`; `src/utils.ts:99,140,174`.
- Commit: dd97a37.
- Certainty: read in the code.
- Used by: DEC-001, TASK-002, Impact.

### FACT-014 The sending tools reach Gmail through two paths

- Observed: `send_draft` posts to `/drafts/send`, and `send_message`, `reply_all` and `forward_message` post to `/messages/send`. No other path the code builds names a send method. The four tools are registered at `src/index.ts:1136,1309,1367,1404`.
- Source: `src/index.ts:1140,1358,1395,1491`.
- Commit: dd97a37.
- Certainty: read in the code.
- Used by: DEC-001, TASK-002.

### FACT-015 Gmail's methods that deliver mail

- Observed: Gmail API v1 describes two methods as sending a message to its recipients: `users.messages.send` ("Sends the specified message to the recipients in the To, Cc, and Bcc headers") and `users.drafts.send`, both `POST` on `.../messages/send` and `.../drafts/send`, with media upload variants under `/upload/` and `/resumable/upload/` at the host root. `messages.insert` and `messages.import` put a message in the user's own mailbox only, and `drafts.create` and `drafts.update` write drafts. Three settings methods mail a verification message, `forwardingAddresses.create`, `sendAs.create` and `sendAs.verify`, and all three are restricted to service accounts with domain-wide delegation.
- Source: Gmail's discovery document, https://gmail.googleapis.com/$discovery/rest?version=v1. developers.google.com was unreachable from the search.
- Date: read 2026-11-12.
- Certainty: stated by the primary source for the two send methods; some other descriptions came back summarised rather than verbatim.
- Used by: DEC-001, CONTEXT.md, Impact.

### FACT-016 Dot segments resolve before a request leaves, percent escapes do not

- Observed: with Gmail's base URL, the path `/drafts/abc/../send` resolves to `/gmail/v1/users/me/drafts/send`, and a local listener received a `fetch` to such a URL on that resolved path. The query string is not part of the resolved path. `/messages/%73end` keeps its escape after resolution. An id passed through `encodeURIComponent` cannot carry a `/`, but the id `..` passes through unchanged.
- Source: `/tmp/EPIC-001-architect-proof/paths.ts`, run with bun 1.4.4, following the WHATWG URL parser that the Workers runtime also follows.
- Date: run 2026-11-12.
- Certainty: ran under bun; the Workers runtime is inferred to resolve the same way.
- Used by: DEC-001, TASK-002.

### FACT-017 How a setting reaches Env and survives a deploy

- Observed: `wrangler types` (wrangler 4.114.0) builds `Env` from the config's `vars`, each typed as its literal and required, and from the keys of `.dev.vars` or `.env`, each typed `string` and required. It never reads `.dev.vars.example`. The committed `worker-configuration.d.ts` types the four secrets of `.dev.vars.example` as `string`. On `wrangler deploy`, a var in the config whose name matches a secret on the Worker triggers a warning and a confirmation, then the var replaces the secret. Without `keep_vars`, a deploy deletes the vars set outside the config, and a deploy never deletes a secret. `bun run setup` copies `wrangler.jsonc` once into `wrangler.local.jsonc`, which every later deploy reads through `.wrangler/deploy/config.json`, and puts `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `ALLOWED_EMAILS` and `COOKIE_ENCRYPTION_KEY` as secrets.
- Source: `node_modules/wrangler/wrangler-dist/cli.js:210885-211268,143444-143551,144344-144354,221765,134869`; `scripts/setup.ts:89-127`; `worker-configuration.d.ts:1-27`; `.dev.vars.example`.
- Commit: dd97a37.
- Certainty: read in the code.
- Used by: DEC-003, TASK-003.

### FACT-018 The handler tests stub the server's tool method

- Observed: `makeAgent` builds a `GmailMCP` through `Object.create(GmailMCP.prototype)` and sets `server` to an object whose only member is `tool(name, description, shape, cb)`, which stores the handler in a map. Its `env` sets `ALLOW_SEND: "true"`. The tests call the stored handlers directly, so a handler error arrives as a rejected promise. The describe block "sending is off unless the deployment enables it" counts 23 handlers without `ALLOW_SEND` and 27 with it. The SDK ships `InMemoryTransport` in `dist/esm/inMemory.js`.
- Source: `test/handlers.test.ts:45-80,1264-1285`; `node_modules/@modelcontextprotocol/sdk/dist/esm/inMemory.js`.
- Commit: dd97a37.
- Certainty: read in the code.
- Used by: TASK-001, TASK-002, Impact.

### FACT-019 Where the docs list the tools and the settings

- Observed: the "What it can do" table of `README.md:124-150` lists the four sending tools in its Write column with no condition, as do the same tables of `README.ja.md` and `README.zh.md` near line 128. `docs/index.html:117` says "27 tools" and is read by no script. Settings are documented in "Who can sign in" (`README.md:241-254`, `ALLOWED_EMAILS`) and in the "Limits" table (`README.md:256-275`, with `MAX_ACCOUNTS` as a `vars` entry of `wrangler.jsonc`). `lhm.plugin.json` lists the 27 tools, and nothing in CI reads it. `scripts/check-counts.py` matches only `badge/tools-(\d+)-` in the three READMEs for tools.
- Source: those lines; `scripts/check-counts.py:28-34`.
- Commit: dd97a37.
- Certainty: read in the files.
- Used by: TASK-004, TASK-005, Impact.

### FACT-020 One generic registration method keeps each handler's argument types

- Observed: a method of `GmailMCP` taking one object `{ kind, name, description, inputSchema: Shape, handler: ToolCallback<Shape> }`, with `Shape extends ZodRawShapeCompat`, infers `Shape` from `inputSchema` and types the handler's arguments from it. A throwaway subclass called it with an empty shape and a handler of no argument, and with a shape of `z.string()`, `z.number().int().default(10)` and `z.string().optional()`, whose handler read its arguments as `string`, `number` and `string | undefined`. Both type-checked. A handler declaring `{ query: number }` for a `z.string()` field failed the type check.
- Source: `GmailMCP.registerByKind` in `src/index.ts` as the sketch of PLAN-001 declares it; `ToolCallback` at `node_modules/@modelcontextprotocol/sdk/dist/esm/server/mcp.d.ts:261` and `ZodRawShapeCompat` at `zod-compat.d.ts:5`, SDK 1.29.0; the probe was checked with `tsc --noEmit`, then removed.
- Commit: 60bfc3d, with the sketch applied.
- Certainty: ran.
- Used by: the sketch of PLAN-001, WP-001, the risk "Handler arguments lose their types" of `design.md`.

### FACT-021 The type check rejects unused private members and unread parameters

- Observed: `tsconfig.json` sets `noUnusedLocals` and `noUnusedParameters`, and the `check` script runs `tsc --noEmit` after Biome. A private method that no code calls, or a parameter that no body reads, fails the gate.
- Source: `tsconfig.json`; `package.json`, script `check`.
- Commit: 60bfc3d.
- Certainty: read in the files; the effect on private members is the documented behaviour of `noUnusedLocals`.
- Used by: the sketch of PLAN-001, which declares its two methods of `GmailMCP` as `protected` and prefixes its unread parameters with `_`.

### FACT-022 What the handler tests' harness can make refuse ahead of the Gmail fetch

- Observed: in `GmailMCP.api`, the refusal of sending comes first, then `assertSessionOwner`, then `assertWithinRate`. `assertSessionOwner` throws "this MCP session belongs to a different Google account" when storage's `owner` names an account other than the caller's; the harness's storage is the `Map` that `makeAgent` returns as `storage`, and the tests of "session ownership" set it. `assertWithinRate` returns at once when `env.RATE_LIMITER` is unset, and otherwise calls `limit({ key })` and throws "rate limit reached for this account; retry shortly" when it answers `success: false`. The harness's `env` sets no `RATE_LIMITER`, and no test stands one up. The helper `bootThenTurnSendingOff` replaces the agent's `env` by assignment, with no cast. Neither check fetches anything. Under bun, `new URL` keeps the path `/messages/%E0%A4%A` as written and `decodeURIComponent` of its pathname throws `URIError`; `/messages/sendfoo` resolves to `/gmail/v1/users/me/messages/sendfoo`.
- Source: `src/index.ts:226-256,327-335,377-383`; `test/handlers.test.ts:48-87,143-181,1374-1394`; a one-line `bun -e` on the two paths.
- Commit: bbaf8d7; bun 1.4.4.
- Certainty: read in the code; the two paths ran.
- Used by: WP-002H.

### FACT-023 The Worker at the epic head starts and refuses MCP without a grant

- Observed: `wrangler dev --port 8799 --ip 127.0.0.1 --persist-to <scratch>` with placeholder Google client values and `ALLOW_SEND` unset logged "Ready on http://127.0.0.1:8799". `GET /` answered 200. `POST /mcp` with a `tools/list` request and no bearer answered 401 `{"error":"invalid_token","error_description":"Missing or invalid access token"}`. Listing the tools or calling one needs a grant from a Google sign-in, which this environment cannot complete: it holds no Google OAuth client and no Google account. So the session did not observe NEED-001 to NEED-006.
- Source: the Worker started by `wrangler dev`, routes `/` and `/mcp`.
- Commit: 739a49d.
- Certainty: observed in the running app.
- Used by: NEED-001 to NEED-006, barrier of PLAN-001-send-safeguards.

### FACT-024 The landing page says four tools need ALLOW_SEND

- Observed: a headless Chromium opened `http://127.0.0.1:8799/` and read the pills "27 tools", "4 of them need ALLOW_SEND=true", "OAuth 2.1 + PKCE", "multi-account", "scope gmail.modify", "MIT", and the line "Four of the 27 tools, send_message, reply_all, forward_message and send_draft, exist only when ALLOW_SEND is exactly true. Set it as a Worker secret with wrangler secret put ALLOW_SEND; drafts work without it."
- Source: route `/` of the Worker started for FACT-023.
- Commit: 739a49d.
- Certainty: observed in the running app.
- Used by: NEED-008, barrier of PLAN-001-send-safeguards.

### FACT-025 The three READMEs give only the two working revocation paths

- Observed: a case-insensitive search for "client secret", "クライアントシークレット" and "客户端密钥" finds 0 lines in `README.md`, `README.ja.md` and `README.zh.md`. The withdrawal item at `README.md:300`, `README.ja.md:284` and `README.zh.md:284` names narrowing `ALLOWED_EMAILS` and revoking at myaccount.google.com/connections.
- Source: the three README files, read as a reader does.
- Commit: 739a49d.
- Certainty: read in the files.
- Used by: NEED-007, barrier of PLAN-001-send-safeguards.

### FACT-026 The three READMEs state 27 tools and document ALLOW_SEND

- Observed: the tool badge reads 27 at `README.md:15`, `README.ja.md:7` and `README.zh.md:7`. A footnote at `README.md:169`, `README.ja.md:157` and `README.zh.md:157` says the four sending tools exist only with `ALLOW_SEND=true` and, in English and Chinese, that the server otherwise offers the other 23. A section at `README.md:260`, `README.ja.md:248` and `README.zh.md:248` says sending is off unless `ALLOW_SEND` is exactly `true`, and that it is set with `wrangler secret put ALLOW_SEND`.
- Source: the three README files, read as a reader does.
- Commit: 739a49d.
- Certainty: read in the files.
- Used by: NEED-008, barrier of PLAN-001-send-safeguards.

## Invocations

### Invocation 0

- Trigger: the pilot resumed draft EPIC-001 on 2026-11-12.
- Who: session, with the pilot.
- Questions created: Q-001, Q-002, Q-003 in round 1; Q-004, Q-005, Q-006, Q-007 in round 2.
- NEEDs touched: NEED-001 to NEED-008 created.
- Drafts: none created or added to. No other open draft existed to fold in.
- Confirmation: the pilot confirmed the intent with "je confirme, go".

### Invocation 1

- Trigger: the pilot's decision at the closing review of PLAN-001 to fix the test-count claims in this epic, on 2026-11-15.
- Who: session, with the pilot.
- Questions reopened: the round 2 fog item on stale counts, as Q-008.
- NEEDs touched: NEED-009 created.
- Drafts: none created or added to.
- Confirmation: the pilot gave the decision with "J accepte tes recommandations".
