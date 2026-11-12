# EPIC-001 Send safeguards and revocation docs

- State: draft
- Last invocation: none
- Confirmed: no
- Origin: the security audit and the architecture critique of the whole repository that the pilot asked for on 2026-11-12, outside any epic, at main 84e427c. No originating epic and no tree, so no FACT to cite.

## Intention

A person who connects an MCP client to this server gets a client that knows which Gmail tools change or send mail, and a deployment that cannot send mail unless the operator enabled sending, so that an instruction hidden in a received message cannot quietly send their mail away, and the README states only revocation paths that work.

## Subjects from the audit

The pilot kept these three subjects out of the audit and rejected the rest.

1. Tool annotations. No tool registered in `src/index.ts` carries MCP annotations, so a client cannot tell read tools from tools that send, forward, trash or delete. The audit proposes `readOnlyHint` on read tools and `destructiveHint` on send, forward, trash and delete tools. The risk it addresses is a prompt injection in a received message that leads the model to call `forward_message` with attachments to a third party while `ALLOW_SEND=true`.
2. Revocation claim in the README. `README.md:284` says that rotating the Google client secret invalidates every grant at once. The audit believes this is wrong, since Google refresh tokens are bound to the client id and the MCP grants in KV do not depend on Google. Nobody has checked it against Google's documentation yet.
3. Send gate that fails open. `src/index.ts:806-814` gates sending by replacing `this.server.tool`, which the MCP SDK marks deprecated, and by matching names against `SEND_TOOLS` (`src/index.ts:151`). A fifth sending tool missing from that set, or a move to `registerTool`, would register ungated. `ALLOW_SEND` is also absent from `Env` in `worker-configuration.d.ts`, from `vars` in `wrangler.jsonc` and from the README, and the README advertises 27 tools while a default deployment serves 23.
