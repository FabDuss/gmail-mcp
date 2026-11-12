# Notice

This skill is written for Cairn. It draws on three sources:

- The article *Tracer Bullets: Keeping AI Slop Under Control* by Matt Pocock, https://www.aihero.dev/tracer-bullets, read on 2 October 2026: the tracer bullet as a small end-to-end slice through every layer, built to get feedback early.
- `skills/engineering/to-tickets` in https://github.com/mattpocock/skills at commit d81f3a1, MIT licence: the vertical-slice rules, "what to build from the user's perspective, not layer by layer", "avoid file paths", wide refactors as the exception sequenced in expand and contract, and prefactoring first, which becomes levers first.
- `skills/principle-sequence-verifiable-units` in the `pstack` folder of https://github.com/cursor/plugins at commit fae2c6e, MIT licence: each unit ends in a verifiable state, and delivery is ordered so the sequence proves itself.

The tracker, the quiz to the user and the acceptance-criteria checkboxes of to-tickets are left out: Cairn has no tracker, the pilot approves at the barrier, and the DoD lives in `design.md`.

`to-tickets`: MIT License, Copyright (c) 2026 Matt Pocock. `principle-sequence-verifiable-units`: MIT License, Copyright (c) 2026 Lauren Tan.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
