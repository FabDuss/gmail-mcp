# PLAN-002 The READMEs state the test count the suite runs

The epic's second plan, composed after the barrier of PLAN-001 from TASK-006, which the design added at the closing review. One work-package, since the plan rewrites nine numbers in three READMEs. The sketch is empty: TASK-006 names no interface, type or seam in the code.

## Work-packages

| Work-package | Depends on | Starts |
|---|---|---|
| WP-004 The CI count check passes on the three READMEs | none: every work-package of PLAN-001 that adds a test is `hardened` | opening work-package, first and alone |

No other work-package of the epic is in flight, so none can add a test after WP-004 rewrites the claims.

## NEEDs closed

| NEED | Closed by |
|---|---|
| NEED-009 The docs state the test count the suite runs | WP-004 |

## Barrier

The barrier observes NEED-009: the count check, run on the epic head that the PR carries, exits 0. It is the last barrier, so it also observes again what changed in the three READMEs: the nine test-count claims, and nothing else.

## TASKs left for the next plan

None.
