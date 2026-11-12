---
name: agent-writing
description: Writing anything an agent reads in Cairn. Use when writing an artifact: intent, tree, design.md, plan.md, or an entry of an epic file or a work-package file.
---

An agent reads every skill, role prompt and artifact in Cairn. These rules make that reader take the same path on every run.

## What you write

You write artifacts and entries: apply [`PROSE-RULES.md`](PROSE-RULES.md) to their prose, follow the Cairn rules below, and use the names in [`VOCABULARY.md`](VOCABULARY.md). Skills and role definitions are never yours to write: they are protected, and Cairn's authors write them with the authoring references kept with the spec.

## Cairn rules

1. **English for every agent reader.** Write skills, prompts, artifacts, the domain glossary and ADRs in English. What is addressed to the pilot stays in the pilot's language: the conversation, the pilot's verbatim answers in the tree, the *Pilot* line of `CONTEXT.md`, the visible text of a prototype, and any document the pilot reads or answers, such as a questionnaire.
2. **Identifiers.** EPIC-, Q-, FACT-, NEED-, TASK-, DEC-, PLAN-, WP-, followed by three digits. An identifier is never renumbered or reused. Anything a human reads shows the title, with the identifier inside it; bare identifiers are for links.
3. **One fact, one place.** Write each fact once and point to it everywhere else. A copy belongs only where a reader must work from one file alone, as a brief does; the copy then carries the identifier of its source.
4. **A skill describes a craft and the rules of the game.** It never lists the cases a written rule already decides, and it carries no exemption clause. The expert sizes the work and records the sizing decision in one line of its artifact.
5. **Canonical names.** Use the names in `VOCABULARY.md` exactly, with no translation and no synonym.

## Done when

- Every rule of `PROSE-RULES.md` holds on the prose of the artifact.
- Each of the five Cairn rules holds on every line.
