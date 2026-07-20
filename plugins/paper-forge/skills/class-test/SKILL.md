---
name: class-test
description: Generate a short graded class test on one or two recently taught topics, typically 20 marks in 30 minutes. Use when the user asks for a class test, unit test, weekly test, surprise test, chapter test, or a quick graded assessment on specific topics. Do not use for full syllabus final exams or ungraded practice sheets.
argument-hint: "[topic] [material files]"
---

# Class test

Low ceremony, narrow scope. A teacher wants this in five minutes, tomorrow morning.

Run the pipeline in `reference/pipeline.md` with profile
`reference/profiles/class-test.json`.

## What makes this skill different

**Scope is narrow and enforced.** Ask which topics were taught. Generate on those
topics only. If the material contains five chapters and the user names one, the
other four do not exist for this paper. This is the single most common thing a
teacher will get wrong in the prompt — confirm the scope before generating.

**No long answers.** A 30-minute test has no room for one. If the user asks for
one anyway, tell them the time budget does not allow it and offer a longer
duration instead.

**Depth over breadth.** A final exam asks one question per topic. A class test
asks four questions on one topic, each attacking a different facet: the
definition, the application, the common misconception, the edge case. Instruct
`blueprint-architect` accordingly — a class test is *allowed* to concentrate.

**Diagnostic value is the point.** Every MCQ distractor should map to a specific
misconception the teacher would want to know about. When a class gets a
distractor wrong en masse, the teacher learns what to reteach. Include a
`misconception` line per distractor in the answer key.

**Blueprint approval is optional.** Show the table, then continue after a beat
unless the user interrupts. Do not block on approval for a 20-mark test.

**Sections by question type.** Group slots by `type` (MCQ, Short Answer,
Numerical, ...) into labelled sections, in the type's natural order from the
blueprint. Each section restarts question numbering at 1. Section header states
the section's mark total, e.g. `Section A — Multiple Choice (6 marks)`.

**Marks on the right.** Print each question's mark value flush right on the
question's own line, e.g. `1. What is X?` on the left, `[2]` right-aligned on
the same line. Section headers likewise right-align their total, e.g.
`Section A — Multiple Choice` left, `[6]` right.

## Output

- `{subject}-class-test.pdf` / `.docx` — student-facing
- `{subject}-answer-key.pdf` / `.docx` — answers, source anchors, and a
  **misconception map**: which distractor indicates which gap in understanding

## Reteach note

After the answer key, add three lines: if students miss Q_n, they likely do not
understand X. This is what turns a test into a teaching instrument.
