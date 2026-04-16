# Skill Registry

All custom skills built for the Life Maxing OS.

## Skill Lifecycle
1. **Draft** — in `experiments/` (WIP, not yet promoted)
2. **Project-local** — promoted to `.claude/skills/` (works in this project only)
3. **Global** — promoted to `~/.claude/skills/` (works in all projects)

## Active Skills

| Skill | Scope | What It Does | Trigger |
|---|---|---|---|
| (none yet) | — | — | — |

## Experiments (WIP)

| Skill | Status | Notes |
|---|---|---|
| (none yet) | — | — |

## How to Promote a Skill
```bash
# Project-local
cp build/skills/experiments/my-skill.md .claude/skills/my-skill/SKILL.md

# Global
cp build/skills/experiments/my-skill.md ~/.claude/skills/my-skill/SKILL.md
```
