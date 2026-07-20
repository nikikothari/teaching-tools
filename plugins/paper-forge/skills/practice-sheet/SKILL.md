---
name: practice-sheet
description: Generate an ungraded practice sheet or worksheet with a difficulty ladder, worked examples, hints, and full solutions. Use when the user asks for a practice sheet, worksheet, homework sheet, drill sheet, revision questions, or practice problems for students to work through. Do not use for graded exams or tests.
argument-hint: "[topic] [material files]"
---

# Practice sheet

Nobody is being graded. The sheet exists to build fluency, so its job is to
*teach*, not to measure.

Run the pipeline in `reference/pipeline.md` with profile
`reference/profiles/practice-sheet.json`.

## What makes this skill different

**No marks. Difficulty labels instead.** Marks signal judgment. Replace the marks
column with a difficulty indicator per question.

**A difficulty ladder, not a difficulty mix.** Order questions so each one is
reachable from the last. The first hard question should be solvable by a student
who worked the mediums above it. Never open with the hardest item — a student who
fails Q1 closes the sheet.

**Worked example at the top of each section.** Before the first question of a
section, show one fully solved problem of the same shape. Label it clearly as an
example. This is the difference between a worksheet and a wall.

**Hints, not answers.** Each question carries a collapsed hint — a single line
naming the concept or the first step, never the method. Format as
`> **Hint:** ...` under the question. The hint should reduce the search space, not
the thinking.

**Solutions explain, they do not just state.** For every question, the solutions
file gives the answer *and* one sentence on why the obvious wrong approach fails.
For numericals, show every step with units.

**Spiral coverage.** Later questions may combine two earlier concepts. This is
encouraged and is the reason a practice sheet exists at all. Instruct
`blueprint-architect` that `coverage_rule` is `spiral`: the final 20% of slots
should require synthesis across topics already drilled.

**Critic strictness is medium.** A slightly ambiguous practice question costs a
student thirty seconds. A slightly ambiguous exam question costs them a grade.
Do not over-reject here — but groundedness is still absolute.

## Output

- `{subject}-practice-sheet.pdf` / `.docx` — questions, difficulty labels, hints, worked examples
- `{subject}-solutions.pdf` / `.docx` — full solutions with the wrong-approach note

Both are student-facing. The student is meant to try first, then check.
