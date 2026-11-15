# Gmail MCP

A Gmail server that one operator deploys on their own Cloudflare account, and that MCP clients connect to on behalf of the Google accounts allowed to sign in.

## Tools

**Read tool**:
A tool that only reads the mailbox and changes nothing in it.
_Avoid_: query tool, safe tool
_Pilot_: read tool

**Undoable write**:
A tool that writes labels, drafts or staged attachments, or takes mail out of the trash, in a way another tool can reverse.
_Avoid_: safe write, non-destructive tool
_Pilot_: undoable write

**Destructive tool**:
A tool that sends, trashes or deletes. Trashing counts as destructive even though untrashing reverses it.
_Avoid_: dangerous tool, write tool
_Pilot_: destructive tool

**Sending tool**:
A destructive tool that sends.
_Avoid_: send tool, mail tool
_Pilot_: sending tool

## Sending

**Sending**:
A request to Gmail that puts mail on the wire, delivering a message or a draft to its recipients.
_Avoid_: delivery, dispatch
_Pilot_: sending

**Sending off**:
The state of a deployment whose `ALLOW_SEND` setting is anything but exactly `true`, or unset. Sending off is the default.
_Avoid_: read-only mode, safe mode
_Pilot_: sending off

## Access

**Revocation path**:
A way the operator or an account holder withdraws the server's access to a mailbox.
_Avoid_: kill switch
_Pilot_: revocation path
