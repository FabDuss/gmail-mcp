---
name: to-questionnaire
description: Turn open questions for the pilot into a questionnaire to answer later. Use when the pilot cannot answer live.
---

Turn what the session needs from the pilot into a **questionnaire**: a Markdown document the pilot fills in when available, alone or with you. Only the session writes one, since only the session talks to the pilot. The recipient is always the pilot, so there is nothing to ask about the send.

## What goes in

Take the questions from the artifacts, never from an interview:

- the open questions of the frontier, from the tree;
- the concepts and scenarios to validate, when a domain change from the architect waits on the pilot;
- what the pilot must look at for a barrier, when the session cannot observe it alone;
- at the closing review, after the NEEDs of the last barrier, each draft this epic created or added to, with the session's recommendation to take it up, keep it or drop it, and each proposal, from the architecture critique, the architect's `designed` entries or the coder notes, with its recommendation to add it to the open draft that carries it, make it a new draft when none does, fix it in this epic when every file it cites is in the epic's diff, or reject it.

Leave out technical questions and anything a scout could find.

## Steps

1. **Gather** the items above. Done when every item waiting on the pilot has a question.
2. **Write** the questionnaire in the pilot's language, following the template below: most important first, one idea per question, grouped under a heading per theme once there are more than a handful. Done when every item is covered.
3. **Hand it over** outside the epic branch: in the conversation, or as a file outside the clone, in `/tmp`, since an untracked file in the clone blocks the end of a turn in the cloud. Never commit it: the pilot's answers are verbatim, and the verbatim lives in the tree only.
4. **File the answers.** Put each answer, word for word, in the tree entry named by the question's identifier; the answer on a concept goes into the concept question it validates. The answer on a NEED of a barrier becomes a FACT reported by the pilot, as the session's observation says. Answers from a closing review are decisions on drafts and proposals: once the NEEDs of the same message are accepted, apply each one, and record them in your barrier entry in the epic file, or in an entry that keeps the status when they come after it, with each item left blank recorded as undecided. Then discard the questionnaire. "All confirmed" confirms every question as written.

## Template

    # <Title>

    **Purpose:** why this questionnaire exists, and the decision riding on it.

    **To confirm everything as written, answer "all confirmed" here:**

    >

    ## Context

    One paragraph for a reader who was not in the conversation: enough to answer well, not a page.

    ## How to answer

    Deadline and rough effort. Partial answers and "I don't know" are useful: flag what you are unsure of rather than skipping it. Your answers go word for word into the epic's decision tree.

    ## <Theme>

    ### Q-012 · <question title>

    _Why this matters: <one line, only where the question could be misread or invite a throwaway answer>._

    >

    ### Concept · <name>

    <definition and scenarios to validate>

    >

    ### Draft · EPIC-031 <title>

    <what it is, in two lines> _Recommendation: <take it up, keep it, or drop it>, because <one line>._

    >

    ### Proposal · <title>

    <what the critique proposes, in two lines> _Recommendation: <add it to EPIC-xxx, make it a new draft, fix it in this epic, or reject it>, because <one line>._

    >

    ## Anything else?

    Anything we did not ask that we should know?

    >

Each question is headed by its Q identifier, the name of the concept it validates, the NEED identifier for a barrier, the draft's EPIC identifier or the proposal's title, so its answer is filed without rereading.
