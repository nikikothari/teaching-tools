# teaching-tools

A Claude Code plugin marketplace for teachers.

## Add this marketplace

```bash
claude plugin marketplace add nikikothari/teaching-tools
claude plugin install paper-forge@teaching-tools
```

## Plugins

### paper-forge

Generate assessments from course material — pasted text, photos of notes, slides,
or PDFs.

| Skill | Invoke | Produces |
|---|---|---|
| Final exam | `/paper-forge:final-exam` | Full-syllabus graded paper with sections and internal choice |
| Class test | `/paper-forge:class-test` | 20-mark test on named topics, with a misconception map |
| Practice sheet | `/paper-forge:practice-sheet` | Ungraded difficulty ladder with hints and worked examples |
| Quick quiz | `/paper-forge:quick-quiz` | 8-item recall quiz on a single concept |

All four share one pipeline and three subagents: `blueprint-architect`,
`item-writer`, and `paper-critic`.

## Local development

```bash
claude --plugin-dir ./plugins/paper-forge
claude plugin validate ./plugins/paper-forge --strict
```

## Publishing

1. Push this repo to GitHub.
2. Set a `version` in `plugins/paper-forge/.claude-plugin/plugin.json` for your
   first tagged release. Leave it unset while iterating.
