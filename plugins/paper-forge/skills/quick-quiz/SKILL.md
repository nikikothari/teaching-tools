---
name: quick-quiz
description: Generate a fast 5-minute recall quiz on a single concept, mostly multiple choice and true/false, for use at the start or end of a class. Use when the user asks for a quick quiz, warm-up questions, exit ticket, bell-ringer, oral quiz, or a handful of rapid recall questions. Do not use for graded tests or full practice sheets.
argument-hint: "[concept]"
---

# Quick quiz

Five minutes at the start or end of a lesson. Eight questions. No ceremony.

Run the pipeline in `reference/pipeline.md` with profile
`reference/profiles/quick-quiz.json`.

## What makes this skill different

**One concept, many facets.** Not eight questions on eight topics — eight
questions on one topic, each testing a different angle. Skip the blueprint table;
just state the eight facets you will cover and proceed.

**Answerable in under 30 seconds each.** If a question requires paper and pencil,
it does not belong here. No multi-step numericals. A one-step substitution is
acceptable; a two-step derivation is not.

**Recall and understand only.** Bloom levels above `understand` cost more than 30
seconds by definition. If the user asks for hard application questions, tell them
`/paper-forge:practice-sheet` is the right tool and offer to switch.

**Distractors still matter — arguably more.** With only eight items and no partial
credit, a weak distractor makes the whole quiz uninformative. The critic runs at
high strictness on distractor quality specifically, and at medium on everything
else.

**No blueprint approval step.** The profile sets `blueprint_approval: skip`.
Generate, critique, ship. The whole interaction should take one turn.

## Output

A single sheet, delivered as `{subject}-quiz.pdf` and `{subject}-quiz.docx`,
with two parts separated by a horizontal rule:

- Questions, numbered, no answers
- An answer line below the rule: `1. b  2. a  3. True ...`

Also print the quiz directly in the conversation, so the teacher can read it off
a screen without opening a file. This is the one skill where the chat output
matters more than the file.
