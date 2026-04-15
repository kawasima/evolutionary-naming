# evolutionary-naming

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![GitHub release](https://img.shields.io/github/v/release/kawasima/evolutionary-naming)](https://github.com/kawasima/evolutionary-naming/releases)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://code.claude.com/docs/en/skills)

A Claude Code skill for **evolutionary naming** — improving names step-by-step through progressive refinement instead of trying to find the perfect name in one shot.

Naming is a process, not a single step. This skill walks names through 7 progressive steps grouped into 3 phases, committing at each step, so improvements are safe, incremental, and reversible.

## What it does

| Phase | Steps | Nature |
|-------|-------|--------|
| Phase 1: Insight to Name | Missing → Nonsense → Honest → Honest and Complete | Universal. Pure naming. Safe. |
| Phase 2: Name to Structure | Honest and Complete → Does the Right Thing | Structural refactoring. Asks permission. |
| Phase 3: Combine for Design | Does the Right Thing → Intent → Domain Abstraction | Domain-level. Asks permission. |

The skill operates in two modes:

- **audit-mode** — exhaustive scan of provided code, classifies every identifier by its current step, outputs a structured table grouped by phase. Does not rename.
- **improve-mode** — interactive single-target improvement. Walks Phase 1 continuously, pauses before Phase 2 and Phase 3.

Match the depth of work to the user's actual story. Most naming improvements should stop after Phase 1.

## Installation

### Via marketplace (recommended)

```
/plugin marketplace add kawasima/evolutionary-naming
/plugin install evolutionary-naming@kawasima-skills
```

### Manual install (single user)

```bash
git clone https://github.com/kawasima/evolutionary-naming.git
cp -r evolutionary-naming/skills/evolutionary-naming ~/.claude/skills/
```

Claude Code auto-discovers skills from `~/.claude/skills/`. No configuration required.

## Usage

The skill activates automatically when you ask Claude to improve naming, refactor a class/method, or review naming quality.

**Audit mode triggers:**
- "このコードの命名で改善余地ある?"
- "命名レビューして"
- "naming audit"

**Improve mode triggers:**
- "`d` を改善して"
- "DocumentManager をリファクタリング"
- "this method's name is unclear"

When the request is ambiguous (large code paste with no specific target), the skill asks which mode to use.

## Files

```
skills/evolutionary-naming/
├── SKILL.md          router and overview
├── reference.md      shared 7-step diagnosis, transitions, red flags
├── audit-mode.md     exhaustive scan workflow + table output
└── improve-mode.md   interactive single-target + pause protocol
```

## License

CC BY 4.0. See [LICENSE](./LICENSE).

## Background and Acknowledgments

The 7-step / 3-phase model used in this skill is based on Arlo Belshee's "Naming as a Process" series (CC BY 3.0), one of the most thorough public treatments of evolutionary naming. The skill adapts that material into Claude Code's skill format with audit/improve modes and phase-boundary safety gates.

References:
- Original blog series: https://arlobelshee.com/tag/naming-is-a-process
- Updated edition: https://www.digdeeproots.com/articles/naming-process/

Other influences worth exploring:
- Refactoring catalogs (extract method, rename, introduce parameter object) — Martin Fowler
- Domain-Driven Design (Whole Value, Ubiquitous Language) — Eric Evans
