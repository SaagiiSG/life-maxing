# Life Maxing OS — Design Spec
**Date:** 2026-04-15
**Author:** Saagii
**Status:** Approved

---

## Overview

A Claude Code meta-workspace that serves as Saagii's life command center. Three layers run simultaneously:

- **brain/** — life ops (scheduled agents, on-demand conversations, triggered routines)
- **build/** — skill factory (custom Claude skills + Claude Code learning documentation)
- **ship/** — content + business (IG series "Life Maxing with AI — First $1K" + Mongolian AI automation agency)

LifeOS iOS is a sub-project (`projects/lifeos-ios/`) built inside this workspace.

---

## Project Structure

```
~/Developer/life-maxing/
├── CLAUDE.md                        # Lazy pointer + auto-learned instructions (<200 lines)
├── .claude/
│   ├── skills/                      # Project-local custom skills
│   ├── hooks/
│   │   └── settings.json            # PostToolUse → learner (auto-appends to CLAUDE.md)
│   └── memory/                      # Project-scoped memory
│
├── brain/                           # LIFE OPS
│   ├── goals/
│   │   ├── Q2.md                    # Q2 targets + progress (source: Obsidian sync)
│   │   └── archive/
│   ├── habits/
│   │   └── current.md               # Current habit stack (source: Obsidian sync)
│   ├── daily/
│   │   └── today.md                 # Agent-written each morning
│   ├── agents/
│   │   ├── morning-brief.yaml       # Scheduled 9am daily
│   │   ├── weekly-review.yaml       # Scheduled Sunday
│   │   └── content-posted.yaml      # Triggered: content posted → fetch IG metrics
│   └── vault -> /Users/saranochirsereglen/Library/Mobile Documents/iCloud~md~obsidian/Documents/second brain/
│
├── build/                           # SKILL FACTORY
│   ├── skills/
│   │   ├── README.md                # Skill registry
│   │   └── experiments/             # WIP skills before promotion
│   ├── learnings/
│   │   ├── claude-code.md           # How Claude Code works (living doc)
│   │   ├── cowork-workflows.md      # Co-Work patterns + discoveries
│   │   └── cloud-agents.md          # Managed/scheduled agents learnings
│   └── templates/
│       └── skill-template.md        # Starter template for every new skill
│
├── ship/                            # CONTENT + BUSINESS
│   ├── ig-series/
│   │   ├── README.md                # Series overview + episode tracker
│   │   └── ep-01/                   # Per-episode: what was built, script, notes
│   └── agency/
│       ├── README.md                # Mongolian AI automation agency
│       ├── playbooks/               # Per-service automation playbooks
│       └── clients/                 # Per-client folders
│
└── projects/
    └── lifeos-ios/                  # iOS/macOS native app (future build)
```

---

## CLAUDE.md Design

**Hard constraints:** under 200 lines, no full context dumps, pointers only.

```markdown
# Life Maxing OS — Saagii

## Who
Mongolian creator, SAT tutor, vibe coder. 4 areas: Physique / Business / Content / Academic.
Q2 theme: "Drying the Concrete." First $1K target via AI automation.

## Resources (read only what's relevant)
- Q2 goals → Obsidian MCP: search "Q1_Review_and_Q2_Planning"
- Habits → Obsidian MCP: search "current habits" OR brain/habits/current.md
- Daily plan → brain/daily/today.md
- IG series → ship/ig-series/README.md
- Agency → ship/agency/README.md
- Skill registry → build/skills/README.md
- Claude Code learnings → build/learnings/
- Raw vault → brain/vault/
- LifeOS iOS → projects/lifeos-ios/

## Learned Instructions
<!-- Auto-appended by PostToolUse hook via learner skill. ≤15 words each. Never delete. -->
- Always use context7 for library/framework docs.

## Session Override
<!-- Paste task-specific context here before starting. Clear after session. -->
```

---

## Obsidian Sync

**Architecture:** bidirectional, no duplication.

- Obsidian = human source of truth (written by Saagii)
- Claude reads Obsidian via **Obsidian MCP** (`mcp__obsidian__*` tools — already connected)
- Claude writes agent outputs back to Obsidian as notes (morning brief, weekly review, etc.)
- `brain/vault` = symlink to iCloud Obsidian path for direct CLI file access
- CLAUDE.md pointers reference Obsidian MCP search terms, not file copies

**Key Obsidian files Claude will read:**
- `10-WIKI/Life/Q1_Review_and_Q2_Planning.md` — Q2 goals
- `10-WIKI/Content/Content_Performance_Data_and_Patterns.md` — content metrics
- `10-WIKI/Business/LifeOS_Product_Journey.md` — product context

---

## Agent Architecture

| Agent | Type | Schedule/Trigger | What It Does |
|---|---|---|---|
| `morning-brief` | Scheduled | Daily 9am | Reads Q2 goals + today's habits → writes brain/daily/today.md + Obsidian note |
| `weekly-review` | Scheduled | Sunday evening | Reads week's habits + Q2 progress → writes Obsidian weekly review |
| `content-posted` | Triggered | Manual flag | Fetches IG metrics → logs to Obsidian content performance note |
| On-demand | Conversational | Open project | Full context via CLAUDE.md + Obsidian MCP on request |

Agent configs stored in `brain/agents/` as YAML. Scheduled agents use Claude's cloud scheduled tasks feature.

---

## Self-Improvement Loop

```
Task completed in life-maxing/
        ↓
PostToolUse hook → fires learner skill
        ↓
learner: "What ≤15-word rule would've made this faster?"
        ↓
Appends bullet to CLAUDE.md → ## Learned Instructions
        ↓
Next session: Claude reads the rule automatically
```

Hook config in `.claude/hooks/settings.json`. Uses existing `learner` skill from OMC suite.

---

## Skill Factory Flow

1. Identify a repeated workflow in daily use
2. Draft skill in `build/skills/experiments/`
3. Test it in a live session
4. Promote to `.claude/skills/` (project-local) or `~/.claude/skills/` (global)
5. Document what you learned in `build/learnings/`
6. Turn the build process into `ship/ig-series/ep-XX/` content

**Skill template** (`build/templates/skill-template.md`) provides a starting structure for every new skill: trigger conditions, what it does, how to use it, self-improvement annotation.

---

## Content Pipeline

- Every skill built = one potential episode
- Episode folder (`ship/ig-series/ep-XX/`) contains: what was built, the why, a short script outline
- Series: **"Life Maxing with AI — First $1K"** on Instagram
- Agency playbooks in `ship/agency/playbooks/` are extracted from successful personal automations

---

## Success Criteria

- Open the project → Claude is immediately warm (knows Q2 goals, active habits, current projects) without reading >200 lines
- A new skill can go from idea → tested → promoted in a single session
- Morning brief runs automatically and writes to Obsidian
- CLAUDE.md `## Learned Instructions` grows over time without manual effort
- Every skill built has a corresponding episode note in ship/ig-series/

---

## Out of Scope (for now)

- LifeOS iOS app (separate future project inside projects/)
- Mongolian agency client work (structure exists, clients folder is empty)
- Full Co-Work workflow implementation (documented in build/learnings/cowork-workflows.md as learned)
