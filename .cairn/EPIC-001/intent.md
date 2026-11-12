# EPIC-001 Send safeguards and revocation docs

- State: draft
- Last invocation: 0
- Confirmed: no
- Origin: the security audit and the architecture critique of the whole repository that the pilot asked for on 2026-11-12, outside any epic, at main 84e427c. No originating epic.

## Intention

A person who connects an MCP client to this server gets a client that knows which Gmail tools change or send mail, and a deployment that cannot send mail unless the operator enabled sending, so that an instruction hidden in a received message cannot quietly send their mail away, and the README states only revocation paths that work. (Q-001)

## Concepts

| Pilot's word | Concept in the code | Status | Behind it |
|---|---|---|---|
| annotations | MCP `ToolAnnotations`, `readOnlyHint` and `destructiveHint`, passed at tool registration | aligned | FACT-001, FACT-007 |
| read tool | a tool that only reads Gmail: `whoami`, `search_messages`, `get_message`, `get_thread`, `list_labels`, `list_drafts`, `get_attachment` | aligned | Q-004 |
| undoable write | a tool whose change another tool can reverse: `modify_labels`, `modify_thread_labels`, `batch_modify_messages`, `create_draft`, `update_draft`, `untrash_message`, `untrash_thread`, `create_label`, `update_label`, `stage_attachment_begin`, `stage_attachment_append`, `stage_attachment_finish` | new | Q-004 |
| destructive tool | `send_message`, `send_draft`, `reply_all`, `forward_message`, `trash_message`, `trash_thread`, `delete_draft`, `delete_label` | new | Q-004 |
| sending tool | the four tools in `SEND_TOOLS` | aligned | FACT-002 |
| sending | a request to Gmail that puts mail on the wire. The code knows it only as the names in `SEND_TOOLS` | gap to settle | FACT-002, Q-007 |
| sending off | `ALLOW_SEND` set to anything but `"true"`, or unset | aligned | FACT-002, FACT-003 |
| revocation path | the ways the README gives to withdraw access, under "Access can be withdrawn" | aligned | FACT-005 |

## Behavioural NEEDs

### NEED-001 Read tools are marked read-only

- Given a client connected to the server, when it lists the tools, then each read tool carries `readOnlyHint: true`.
- Questions: Q-004.

### NEED-002 Undoable writes are marked non-destructive

- Given a client connected to the server, when it lists the tools, then each undoable write carries `readOnlyHint: false` and `destructiveHint: false`.
- Questions: Q-004.

### NEED-003 Destructive tools stay destructive

- Given a client connected to the server, when it lists the tools, then no destructive tool carries `readOnlyHint: true` or `destructiveHint: false`.
- Questions: Q-004.

### NEED-004 Sending tools are absent while sending is off

- Given a deployment with sending off, when a client lists the tools, then no sending tool is in the list.
- Questions: Q-007.

### NEED-005 Sending is refused while sending is off

- Given a deployment with sending off, when any tool call would lead to sending, then the server refuses it and Gmail receives no request to send.
- Questions: Q-007.

### NEED-006 Sending works when enabled

- Given a deployment with `ALLOW_SEND=true`, when a client lists the tools, then the four sending tools are in the list, and a call to one of them sends.
- Questions: Q-007.

### NEED-007 The README gives only revocation paths that work

- Given `README.md`, `README.ja.md` and `README.zh.md`, when a reader looks for how to withdraw access, then none of them says that rotating the Google client secret invalidates grants, and each still gives narrowing `ALLOWED_EMAILS` and revoking at myaccount.google.com/connections.
- Questions: Q-005, Q-002.

### NEED-008 The docs state the tool count and the setting that gates sending

- Given the README, its two translations and `docs/index.html`, when a reader looks at the tool count, then it says 27 and that the four sending tools exist only with `ALLOW_SEND=true`. The README documents `ALLOW_SEND`: off unless set to exactly `true`.
- Questions: Q-006, Q-003.

## Constraint NEEDs

None.

## Out of scope

- A server-side check on sending, such as a recipient allowlist or a confirmation asked by the server. Q-001.
- A way to cut every grant at once. Q-005, with Q-002.
- The test count advertised in the README. Dropped from the fog in round 2.
- The other proposals of the audit and the critique, rejected by the pilot before this draft opened.

## Questions for the architect

- Where the refusal of sending lives, so that it holds whatever tool asks (NEED-005), and how tool registration follows from the same decision (NEED-004).
- Whether registration moves from the deprecated `tool()` overloads to `registerTool` (FACT-002), and how the annotations are passed.
- Whether `ALLOW_SEND` is declared in `Env` and in `wrangler.jsonc` (FACT-003).
- How `scripts/check-counts.py` and its CI step keep checking the tool count once the docs state the gated four (FACT-009).

## Revisions

- Invocation 0: NEED-001 to NEED-008 created.
