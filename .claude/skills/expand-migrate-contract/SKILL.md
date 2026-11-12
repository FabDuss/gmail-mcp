---
name: expand-migrate-contract
description: Sequence a wide change as expand, migrate, contract. Use when one change breaks more callers than a work-package can absorb.
---

A **wide change** is one uniform change (renaming a column, retyping a shared symbol) whose blast radius fans across the codebase, so that no vertical slice can land green. It is an exception to vertical slicing, as a lever is: sequence it in three stages instead. The decision to migrate is a DEC; this skill says how, once it is taken.

The architect writes the stages as TASKs, with the number of callers written down. The planner orders them. Work-packages stay work-packages, and the epic branch is the integration branch.

## The three stages

1. **Expand.** Add the new form beside the old, so nothing breaks.
   DoD: the old form stays green; the new form is reachable.
2. **Migrate.** Move the callers over in work-packages sized by blast radius (per package, per directory), each after the expand. The old form still exists, so each work-package passes the gates on its own; size them so it can.
   DoD: the work-package's callers are on the new form; the old form is intact.
3. **Contract.** Delete the old form once no caller remains, after every migrate work-package.
   DoD: the old form is gone, and no caller is left.

## Rules

- No compatibility layer survives the contract.
- Tests assert the new contract. Tests that only protected the old internals are deleted in the contract.
- A temporary adapter is a TASK of nature lever, with its own removal TASK. It is never architecture by default.
- An epic that is only a wide change has no core work-package: the expand TASK opens the plan.
- These work-packages are not framed as tracer bullets.
