---
name: final-exam
description: Generate a summative final examination paper covering a full syllabus, with sections, internal choice, and a marking scheme. Use when the user asks for a final exam, term-end paper, board-pattern paper, semester exam, annual exam, or a high-stakes graded paper from their course material. Do not use for short class tests or ungraded practice.
argument-hint: "[material files or topic]"
---

# Final exam paper

High stakes. A student's grade depends on this. Rigour beats speed.

Run the pipeline in `reference/pipeline.md` with profile
`reference/profiles/final-exam.json`.

## What makes this skill different

**Coverage is the primary obligation.** A final exam that ignores a syllabus unit
is a broken exam, no matter how good the questions are. `blueprint-architect`
must place at least one slot in every unit. If total marks make that impossible,
stop and tell the user which units cannot be covered — do not silently drop them.

**Sections.** Group by question type, ascending in marks. Label them A, B, C, D.
Each section header states its marks and how many questions must be attempted.

**Internal choice.** For every long-answer slot, generate two alternatives on
*different topics* of comparable difficulty. Present as "Answer either (a) or
(b)". Two alternatives on the same topic defeat the purpose.

**Time budget is a hard constraint, not a heuristic.** Compute estimated time at
1.5 minutes per mark plus 10 minutes reading. If it exceeds the stated duration,
cut marks and rebuild the blueprint. Do not proceed with a paper that cannot be
finished.

**The critic runs at maximum strictness.** Every numerical is solved
independently. Every MCQ is attacked without the source material. Nothing ships
on a `revise` verdict — only `pass`.

## Blueprint approval is mandatory

Present the blueprint as a table with columns: Q, Section, Marks, Type, Bloom,
Difficulty, Topic. Then show a unit-wise marks distribution against the syllabus.

Stop. Wait for the user to approve or adjust. Do not write a single question
before they do.

## Paper header

Generate an instructions block: total marks, duration, general instructions,
section-wise attempt rules, and whether calculators or data sheets are permitted.
Ask about permitted aids if the paper contains numericals.

## Output

- `question-paper.md` — student-facing, no answers anywhere
- `answer-key.md` — answers, worked solutions, rubrics, source anchors
- `blueprint.json` — the approved blueprint, so the paper can be regenerated
