# PLAN-001 Send safeguards and revocation docs

The epic's only plan: every TASK can be sketched now, and no part waits on another being built.

## Work-packages

| Work-package | Depends on | Starts |
|---|---|---|
| WP-001 Tools listed by kind, sending tools only while sending is on | none | opening work-package, the core, first |
| WP-002 No sending request reaches Gmail while sending is off | WP-001 at `validated` | after WP-001 |
| WP-003 The READMEs and the landing page state working revocation paths, 27 tools and ALLOW_SEND | none | alongside WP-001 |

WP-002 depends on WP-001 for two reasons: both write the session agent and the handler tests (rule 8), and WP-002 reads the sending decision and the harness that WP-001 fills.

## NEEDs closed

| NEED | Closed by |
|---|---|
| NEED-001 Read tools are marked read-only | WP-001 |
| NEED-002 Undoable writes are marked non-destructive | WP-001 |
| NEED-003 Destructive tools stay destructive | WP-001 |
| NEED-004 Sending tools are absent while sending is off | WP-001 |
| NEED-005 Sending is refused while sending is off | WP-002 |
| NEED-006 Sending works when enabled | WP-002 |
| NEED-007 The README gives only revocation paths that work | WP-003 |
| NEED-008 The docs state the tool count and the setting that gates sending | WP-003 |

## Barrier

The barrier observes all eight NEEDs. The pilot sees the annotations and the 23 or 27 tools in a client's list, a refused send while sending is off, and the three READMEs and the landing page.

## TASKs left for the next plan

None.
