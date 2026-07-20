# The shared pipeline

Every Paper Forge skill runs these six phases. The skill supplies a **profile**
and a set of **overrides**; the phases below do not change.

Read the skill's profile JSON from `reference/profiles/` before starting.

---

## Phase 1 — Read the material

Read every file supplied. Images and PDFs go through the Read tool directly.

Produce a **topic map**: a numbered list of distinct concepts, each with a
locator (page number, slide number, or an anchor phrase under ten words).

Everything generated downstream must trace back to an entry in this map. If any
part of a photo is illegible, say so — never infer the missing content.

Apply the profile's `scope` rule:
- `full_syllabus` — every topic in the map is in play.
- `narrow` — ask the user which topics; refuse to generate beyond them.
- `single_concept` — one topic only, drilled in depth.

Separately, check the supplied files for **branding assets**: a school logo
image, a school/institution name, a paper title, or a reference paper whose
header and instruction wording should be matched. These are formatting inputs,
not content — they produce no topic-map entries.

- If any are supplied, reuse them as given: same logo file, same name, same
  title, same instruction phrasing and layout style as the reference.
- If none are supplied, fall back to a generic default header: no logo, a
  plain title built from `{subject} — {paper type}`, and a placeholder school
  name line the user can find-and-replace.

## Phase 2 — Resolve the spec

Start from the profile's defaults. Ask the user only for what the profile marks
`ask_user: true`, all in one message, and tell them the defaults so they can
reply "defaults are fine".

Never ask about fields the profile has already fixed. A class test does not ask
about long-answer weighting; it has none.

## Phase 3 — Blueprint

Invoke `blueprint-architect` with the topic map, the resolved spec, and the
profile.

Render the blueprint as a compact table. If the profile sets
`blueprint_approval: required`, stop and wait for the user. If `optional`, show
it and continue unless they interrupt.

This is the cheapest point to catch bad topic coverage. Do not skip it, even for
a five-question quiz.

## Phase 4 — Write items

Invoke `item-writer` once per question type present in the blueprint, in
parallel. Pass the profile's `item_writer_mode`, which controls whether items
carry hints, worked explanations, or nothing beyond the answer.

## Phase 5 — Critique

Invoke `paper-critic` with the full item set, the blueprint, and the profile's
`critic_strictness`.

Rewrite every `reject` by re-invoking `item-writer` with the critic's reason
attached. Loop at most twice. If an item fails three times, surface it to the
user rather than dropping it silently.

## Phase 6 — Assemble

Derive `{subject}` once: a short kebab-case slug from the topic map's subject
or chapter name (e.g. `photosynthesis`, `thermodynamics-ch4`). If the material
gives no clear subject name, ask the user for one rather than guessing.

Create the profile's `output_dir` if it does not exist. Write each filename in
the profile's `outputs` array into that folder, substituting `{subject}`. Each
paper type keeps its own folder — files never land loose at the project root.

Compose the student-facing file's header using the branding assets (or
default) resolved in Phase 1, then an instructions block sized to the
profile's `stakes`:

- `high` — full detail, matching a real board paper: exam name, subject,
  maximum marks, time allowed, a reading-time note if reading time is
  separate from writing time, "answers on paper provided separately" where
  applicable, section-wise attempt rules (e.g. "attempt all of Section A and
  any four of Section B"), a note that marks are shown in brackets, and
  permitted aids (calculator, data sheet) if asked about.
- `medium` or `none` — only the essentials: attempt-all note, total marks
  (if any), time allowed. Skip reading-time notes, aid permissions, and other
  minutiae — a class test or practice sheet is not the place for them.

Before writing:

1. Sum the marks. They must equal the target exactly.
2. Confirm every item has a non-empty source anchor.
3. Confirm no two items test the same concept.

Never place answers in a student-facing file.

---

## Invariants — true for every skill

- **Answerable from the supplied material alone.** An item needing outside
  knowledge is out of scope, regardless of how good it is.
- **Every item carries a source anchor** in the teacher-facing file. A teacher
  who cannot verify an item in ten seconds will not use it.
- **Never fabricate** a figure, table, or diagram reference absent from the
  material.
- **Match reading level** to the stated grade.
