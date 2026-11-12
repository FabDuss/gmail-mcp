---
name: prototype
description: Build a throwaway prototype to answer a question of ideation. Use when a question is about how the logic behaves or what something looks like.
---

# Prototype

A prototype is **throwaway code that answers a question**. The question decides the shape. In Cairn, the session builds it during ideation, for the pilot to drive. It produces a decision, never a fact: facts come from scouts.

## Pick a branch

Write the question first, as a Q in the tree, with prototype as its resolution. Then pick the branch, from the question, the surrounding code, or by asking the pilot:

- **"Does this logic / state model feel right?"** → [LOGIC.md](LOGIC.md). Build a single shareable HTML file (free-play buttons plus tabbed guided walkthroughs) that pushes the state machine through cases that are hard to reason about on paper, and that a non-developer can drive.
- **"What should this look like?"** → [UI.md](UI.md). Generate several radically different UI variations on a single route, switchable via a URL search param and a floating bottom bar.

The two branches produce very different artifacts, so getting this wrong wastes the whole prototype. If the question is genuinely ambiguous and the pilot isn't reachable, default to whichever branch better matches the surrounding code (a backend module → logic; a page or component → UI) and state the assumption at the top of the prototype.

## Rules that apply to both

1. **Throwaway from day one, in its own worktree.** Build it in its own worktree, under `.claude/worktrees/`, which git ignores, on the local branch `epic/<id>/proto/Q-xxx`, never merged and never on the epic branch. Place it next to the module or page it explores so context is obvious, and name it so a casual reader can see it's a prototype. For throwaway UI routes, obey whatever routing convention the project already uses; don't invent a new top-level structure.
2. **Trivial to run.** A UI prototype starts from one command in the project's task runner: `pnpm <name>`, `python <path>`, `bun <path>`, etc. A logic demo is a single HTML file the pilot double-clicks. Either way, no thinking required to start it. Every visible word is in the pilot's language, since the pilot drives it.
3. **No persistence by default.** State lives in memory. Persistence is the thing the prototype is _checking_, not something it should depend on. If the question explicitly involves a database, hit a scratch DB or a local file with a clear "PROTOTYPE, wipe me" name.
4. **Skip the polish.** No tests, no error handling beyond what makes the prototype _runnable_, no abstractions. The point is to learn something fast.
5. **Surface the state.** After every action (logic) or on every variant switch (UI), print or render the full relevant state so the pilot can see what changed.
6. **Capture the answer, keep the code out.** The verdict is the pilot's answer, filed word for word in the Q entry of the tree, and the Q entry points to the prototype's branch. Commit the prototype on its local branch, then remove its worktree with `git worktree remove`: the branch keeps it, and a worktree left behind would feed its tests to every test run started from the clone. The session pushes that branch only when the pilot must open it outside the session. No code from the prototype enters the sketch or a work-package: the decision flows into the NEEDs, and the work-packages build from them.
