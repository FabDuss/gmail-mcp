---
name: blast-radius
description: Find what a change breaks beyond its diff, and prove the fact it is safe because of. Use when writing the Impact section of design.md.
---

# Blast radius

Find what an epic's change breaks somewhere else, before any work-package is written. The architect uses this for the Impact section of `design.md`.

Listing the callers is not the job: a search finds those in a second. The job is the breakage a search will not show.

## Do not trust your own writeup

A blast-radius writeup that sounds right is worthless: it reads as convincing whether or not it is true. Find the one or two facts the whole change depends on, and prove them by running code.

### How sure are you

For each fact the change's safety depends on, take it as far down this scale as is cheap, and say where it stopped.

1. **Said.** You said so. Worthless on its own.
2. **Pointed.** You pointed at the line: a real `file:line`, or the library's own source.
3. **Shown.** You showed the bad case cannot happen, walking the failure step by step until it does not reach.
4. **Ran.** A script or test calls the real code and fails loudly if you are wrong.
5. **Reproduced.** You reproduced it in the running app.

Level 4 is usually one small throwaway script that imports the same library the app ships and calls the exact function in doubt. Write it outside your worktree, in `/tmp`, so that it is never committed.

## Steps

1. **Read the change.** The TASKs, the symbols they add, change and delete, and what the code will do differently, including the part the TASKs do not spell out. Pull the history through scout (why).
2. **Find the one fact it is safe because of.** Most changes that look risky are safe because of a single fact, such as "this call only drops cache entries that are already dead, and nothing else". Find that fact. If it holds, most risky cases clear at once. Spend your time here, not on a long list of maybes.
3. **Look where a search stops.** Read the source of the library you call, and check its pinned version and any local patch. Work out when things run: microtasks, teardown, framework lifecycles. Follow what a symbol search misses: the JSON an API returns, a database column, a wire format, another language reading the same bytes, a feature flag, code three hops downstream. Check the files many slices touch: route registries, migrations, dependency-injection modules, stored data.
4. **Be honest about each risk.** Give it a real chance of happening and a real cost if it does. Keep the risks you confirmed; list the ones you checked and cleared separately. Cite a real `file:line` in the FACT you write to the tree; a search that finds nothing is still an answer; never invent a caller or an API.
5. **Prove the one fact.** Write the throwaway script, run it, and paste what happened. If you cannot prove it, mark it **unproven**.

When the number of callers exceeds what one work-package can absorb, sequence the change with expand-migrate-contract.

## The Impact section

Write it into `design.md`, through agent-writing's prose rules:

- **What changes**, including the part that is not obvious.
- **The one fact it is safe because of**: its FACT, with the level it reached and its proof, or marked unproven. Name the FACT, never a file: the `file:line` lives in the FACT.
- **Risks**, each with how it breaks, where, how likely, how costly, and how to check it.
- **Cleared**: what you checked and why it is fine.
- **The cheapest check before integration**. It becomes a DoD line, never a separate artifact.

The judge judges the DoD, not the blast radius: this skill never reviews a diff after the fact, and never runs as a work-package.
