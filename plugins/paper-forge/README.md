# Paper Forge

Four assessment generators sharing one pipeline and three agents.

| Skill | Invoke | Produces |
|---|---|---|
| Final exam | `/paper-forge:final-exam` | Full-syllabus graded paper, sections, internal choice |
| Class test | `/paper-forge:class-test` | 20-mark test on named topics, misconception map |
| Practice sheet | `/paper-forge:practice-sheet` | Ungraded ladder with hints and worked examples |
| Quick quiz | `/paper-forge:quick-quiz` | 8-item recall quiz on one concept |

## Install

```bash
claude plugin marketplace add your-user/teaching-tools
claude plugin install paper-forge@teaching-tools

# development
claude --plugin-dir ./paper-forge
```

## Architecture

```
paper-forge/
├── .claude-plugin/plugin.json
├── skills/                      # thin: policy only
│   ├── final-exam/SKILL.md
│   ├── class-test/SKILL.md
│   ├── practice-sheet/SKILL.md
│   └── quick-quiz/SKILL.md
├── agents/                      # shared: all four skills use these
│   ├── blueprint-architect.md
│   ├── item-writer.md
│   └── paper-critic.md
└── reference/
    ├── pipeline.md              # the six phases, written once
    ├── profiles/*.json          # what each skill overrides
    ├── blueprint-schema.json
    └── bloom-levels.md
```

**Skills are thin. The pipeline is shared.** A skill states what kind of paper it
is and what it forbids; `reference/pipeline.md` states how any paper gets made.
Fixing a distractor bug means editing `agents/item-writer.md` once, not four times.

**Profiles are data, not prose.** `reference/profiles/*.json` is where the
differences live — mark totals, critic strictness, coverage rules. Adding a fifth
paper type is a new profile plus a twenty-line SKILL.md.

## Development

```bash
claude plugin validate ./paper-forge --strict
claude --debug --plugin-dir ./paper-forge
/reload-plugins            # after editing anything outside SKILL.md
```

Leave `version` unset while iterating — Claude Code caches on the version string
and will not pick up new commits until you bump it.

Debug one stage in isolation by @-mentioning the agent directly:

```
@paper-forge:paper-critic  review these five items against chapter-4.pdf
```
