# Life Maxing OS — Workspace Scaffold Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Scaffold `~/Developer/life-maxing/` as a fully wired Claude Code meta-workspace — lazy-pointer CLAUDE.md, brain/build/ship layers, Obsidian vault symlink, project-local self-improvement hook, and agent YAML configs.

**Architecture:** Directory scaffold + config files only (no runnable code). Verification steps use `ls`, `cat`, `wc -l`, and `ln -s` checks instead of test runners. Each task produces committed, usable files.

**Tech Stack:** Bash (scaffold), YAML (agent configs), Markdown (all docs + skills), Claude Code hooks (settings.json), Obsidian MCP (runtime vault access)

---

## File Map

| File | Responsibility |
|---|---|
| `CLAUDE.md` | Lazy-pointer context + auto-growing learned instructions |
| `.claude/settings.json` | Project-local hooks (Stop → self-improvement reminder) |
| `.claude/skills/` | Project-local custom skills (empty dir, populated over time) |
| `.claude/memory/` | Project-scoped claude-mem memory files |
| `brain/goals/Q2.md` | Q2 targets + progress tracker (sourced from Obsidian) |
| `brain/habits/current.md` | Current habit stack + streaks |
| `brain/daily/today.md` | Today's plan (agent-written each morning) |
| `brain/agents/morning-brief.yaml` | Scheduled agent config: daily 9am briefing |
| `brain/agents/weekly-review.yaml` | Scheduled agent config: Sunday weekly review |
| `brain/agents/content-posted.yaml` | Triggered agent config: post → fetch IG metrics |
| `brain/vault` | Symlink → iCloud Obsidian second brain |
| `build/skills/README.md` | Skill registry (what exists, trigger, promote status) |
| `build/skills/experiments/` | WIP skills before promotion |
| `build/learnings/claude-code.md` | Living doc: how Claude Code works |
| `build/learnings/cowork-workflows.md` | Co-Work patterns + discoveries |
| `build/learnings/cloud-agents.md` | Managed/scheduled agents learnings |
| `build/templates/skill-template.md` | Starter template for every new skill |
| `ship/ig-series/README.md` | "Life Maxing with AI — First $1K" series tracker |
| `ship/ig-series/ep-01/notes.md` | Episode 1 notes (this workspace build = ep 1) |
| `ship/agency/README.md` | Mongolian AI automation agency overview |
| `ship/agency/playbooks/` | Empty dir — populated as automations are proven |
| `ship/agency/clients/` | Empty dir — populated per client |
| `projects/lifeos-ios/README.md` | Placeholder for iOS/macOS native app |
| `.gitignore` | Ignore secrets, .env, node_modules |
| `README.md` | Project root overview |

---

## Task 1: Git Init + Root Files

**Files:**
- Create: `README.md`
- Create: `.gitignore`

- [ ] **Step 1: Initialize git repo**

```bash
cd ~/Developer/life-maxing
git init
```

Expected: `Initialized empty Git repository in .../life-maxing/.git/`

- [ ] **Step 2: Create README.md**

```bash
cat > ~/Developer/life-maxing/README.md << 'EOF'
# Life Maxing OS

Claude Code meta-workspace for Saagii.

## Layers
- **brain/** — life ops (agents run your life)
- **build/** — skill factory (make Claude smarter)
- **ship/** — content + business (IG series + agency)
- **projects/** — actual code projects (LifeOS iOS, etc.)

## Quick Start
Open this directory in Claude Code. CLAUDE.md loads your context.
EOF
```

- [ ] **Step 3: Create .gitignore**

```bash
cat > ~/Developer/life-maxing/.gitignore << 'EOF'
.env
.env.local
.DS_Store
node_modules/
*.log
.claude/memory/
brain/vault/
EOF
```

Note: `brain/vault/` is a symlink to iCloud — exclude from git tracking.

- [ ] **Step 4: Verify**

```bash
ls ~/Developer/life-maxing/
# Expected: README.md  .gitignore  docs/
wc -l ~/Developer/life-maxing/README.md
# Expected: 13
```

- [ ] **Step 5: Commit**

```bash
cd ~/Developer/life-maxing
git add README.md .gitignore
git commit -m "feat: init life-maxing OS workspace"
```

---

## Task 2: CLAUDE.md — The Warm-Start File

**Files:**
- Create: `CLAUDE.md`

- [ ] **Step 1: Create CLAUDE.md**

```bash
cat > ~/Developer/life-maxing/CLAUDE.md << 'EOF'
# Life Maxing OS — Saagii

## Who
Mongolian creator, SAT tutor, vibe coder. 4 areas: Physique / Business / Content / Academic.
Q2 theme: "Drying the Concrete." Goal: First $1K via AI automation by end of Q2.

## Resources (read only what's relevant — do NOT load all upfront)
- Q2 goals + progress → Obsidian MCP: search "Q1_Review_and_Q2_Planning"
- Current habits → Obsidian MCP: search "current habits" OR brain/habits/current.md
- Today's plan → brain/daily/today.md
- Content performance → Obsidian MCP: search "Content_Performance_Data_and_Patterns"
- IG series notes → ship/ig-series/README.md
- Agency playbooks → ship/agency/README.md
- Skill registry → build/skills/README.md
- Claude Code learnings → build/learnings/claude-code.md
- Co-Work patterns → build/learnings/cowork-workflows.md
- Cloud agents → build/learnings/cloud-agents.md
- Raw vault (CLI) → brain/vault/
- LifeOS iOS → projects/lifeos-ios/

## Active Projects
- Life Maxing OS (this workspace) — scaffolding + first agents
- LifeOS iOS — future build in projects/lifeos-ios/
- IG Series — "Life Maxing with AI — First $1K" — ship/ig-series/

## Learned Instructions
<!-- Auto-suggested at session end. Append ≤15-word bullets. Never delete. -->
- Always use context7 for library/framework docs.
- Read only relevant Resources sections — never load all upfront.

## Session Override
<!-- Paste task-specific context before starting. Clear after session. -->

EOF
```

- [ ] **Step 2: Verify line count (must be under 200)**

```bash
wc -l ~/Developer/life-maxing/CLAUDE.md
# Expected: under 40 lines — well within budget
```

- [ ] **Step 3: Commit**

```bash
cd ~/Developer/life-maxing
git add CLAUDE.md
git commit -m "feat: add CLAUDE.md warm-start with lazy pointers"
```

---

## Task 3: .claude/ — Project-Local Config + Self-Improvement Hook

**Files:**
- Create: `.claude/settings.json`
- Create: `.claude/skills/` (empty dir with .gitkeep)
- Create: `.claude/memory/` (empty dir, gitignored)

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p ~/Developer/life-maxing/.claude/skills
mkdir -p ~/Developer/life-maxing/.claude/memory
touch ~/Developer/life-maxing/.claude/skills/.gitkeep
```

- [ ] **Step 2: Create project-local settings.json with Stop hook**

The Stop hook fires at session end and prints a reminder for the self-improvement loop.

```bash
cat > ~/Developer/life-maxing/.claude/settings.json << 'EOF'
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "echo '💡 Session ended. Consider: what ≤15-word rule would make next session faster? Append to CLAUDE.md ## Learned Instructions if worth keeping.'"
          }
        ]
      }
    ]
  }
}
EOF
```

- [ ] **Step 3: Verify the hook JSON is valid**

```bash
python3 -m json.tool ~/Developer/life-maxing/.claude/settings.json > /dev/null && echo "Valid JSON"
# Expected: Valid JSON
```

- [ ] **Step 4: Commit**

```bash
cd ~/Developer/life-maxing
git add .claude/settings.json .claude/skills/.gitkeep
git commit -m "feat: add project-local .claude config with Stop hook for self-improvement"
```

---

## Task 4: brain/ — Life Ops Layer

**Files:**
- Create: `brain/goals/Q2.md`
- Create: `brain/habits/current.md`
- Create: `brain/daily/today.md`

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p ~/Developer/life-maxing/brain/goals/archive
mkdir -p ~/Developer/life-maxing/brain/habits/logs
mkdir -p ~/Developer/life-maxing/brain/daily
mkdir -p ~/Developer/life-maxing/brain/agents
```

- [ ] **Step 2: Create brain/goals/Q2.md**

```bash
cat > ~/Developer/life-maxing/brain/goals/Q2.md << 'EOF'
# Q2 2026 Goals — "Drying the Concrete"

> Source of truth: Obsidian → Q1_Review_and_Q2_Planning.md
> This file is a quick-reference copy. Update Obsidian first.

## Theme
Lock In (April) → Build Momentum (May) → Harvest (June)

## Targets

### Education
- [ ] Accept best university offer (by Apr 30)
- [ ] Visa submitted (by Jun 30)
- [ ] Housing plan documented

### Physique
- [ ] Visible abs
- [ ] Bench 275lb (realistic) / 315lb (stretch)
- [ ] Deadlift 180kg (realistic) / 200kg (stretch)
- [ ] 4x/week training
- [ ] Meal prep every Saturday

### Business
- [ ] 3 clients
- [ ] $500–1000 revenue
- [ ] 4 Claude/Co-Work systems built in April

### Content
- [ ] 1500 followers (realistic) / 2500 (stretch)
- [ ] 5K avg views (realistic) / 10K (stretch)
- [ ] Mino Lee documentation style

### Product (LifeOS)
- [ ] iOS MVP by Jun 15
- [ ] 5–10 beta testers by Jun 30

## Progress Log
| Date | Update |
|---|---|
| 2026-04-16 | Workspace scaffolded — Life Maxing OS live |

EOF
```

- [ ] **Step 3: Create brain/habits/current.md**

```bash
cat > ~/Developer/life-maxing/brain/habits/current.md << 'EOF'
# Current Habit Stack

> Source of truth: Obsidian vault. Update there, sync here when needed.

## Daily Habits
| Habit | Streak | Target | Notes |
|---|---|---|---|
| Gym | - | 4x/week | Mon/Tue/Thu/Fri |
| Meal prep | - | Saturdays | Batch cook |
| Night routine | - | Daily 11pm | Log habits, tasks, read, journal, pray |
| Content post | - | Daily | IG post |

## Tracking
Agent `morning-brief` reads this file and Obsidian habit notes each morning.
To log a habit: use Obsidian app or tell Claude "log [habit] for today".

EOF
```

- [ ] **Step 4: Create brain/daily/today.md**

```bash
cat > ~/Developer/life-maxing/brain/daily/today.md << 'EOF'
# Daily Brief — {{DATE}}

> Written by morning-brief agent at 9am. Read-only after generation.

## Today's Focus


## Tasks


## Habit Check
- [ ] Gym
- [ ] Content post
- [ ] Night routine

## Notes


EOF
```

- [ ] **Step 5: Verify**

```bash
ls ~/Developer/life-maxing/brain/
# Expected: agents/  daily/  goals/  habits/
ls ~/Developer/life-maxing/brain/goals/
# Expected: Q2.md  archive/
```

- [ ] **Step 6: Commit**

```bash
cd ~/Developer/life-maxing
git add brain/
git commit -m "feat: add brain/ life ops layer — goals, habits, daily plan"
```

---

## Task 5: Obsidian Vault Symlink

**Files:**
- Create: `brain/vault` (symlink)

- [ ] **Step 1: Create the symlink**

```bash
ln -s "/Users/saranochirsereglen/Library/Mobile Documents/iCloud~md~obsidian/Documents/second brain" ~/Developer/life-maxing/brain/vault
```

- [ ] **Step 2: Verify the symlink works**

```bash
ls ~/Developer/life-maxing/brain/vault/
# Expected: 00-RAW  10-WIKI  20-OUTPUTS  99-ARCHIVE  HOME.md  Topics  daily notes
```

- [ ] **Step 3: Verify a key file is readable**

```bash
head -5 ~/Developer/life-maxing/brain/vault/HOME.md
# Expected: # Saagii's Second Brain
```

- [ ] **Step 4: Confirm vault is gitignored**

```bash
cd ~/Developer/life-maxing && git status brain/vault
# Expected: nothing (symlink is in .gitignore)
```

Note: `.gitignore` already has `brain/vault/` — symlink won't be committed, which is correct. Each machine has its own iCloud path.

---

## Task 6: brain/agents/ — Agent YAML Configs

**Files:**
- Create: `brain/agents/morning-brief.yaml`
- Create: `brain/agents/weekly-review.yaml`
- Create: `brain/agents/content-posted.yaml`

- [ ] **Step 1: Create morning-brief agent config**

```bash
cat > ~/Developer/life-maxing/brain/agents/morning-brief.yaml << 'EOF'
name: morning-brief
type: scheduled
schedule: "0 9 * * *"   # 9:00am daily
description: Daily morning briefing agent

prompt: |
  You are Saagii's morning brief agent. Run every morning at 9am.

  Steps:
  1. Read brain/daily/today.md
  2. Use Obsidian MCP to search "Q1_Review_and_Q2_Planning" — get today's relevant Q2 priorities
  3. Use Obsidian MCP to search "current habits" — get today's habit targets
  4. Rewrite brain/daily/today.md with:
     - DATE filled in (today's date)
     - Today's Focus: top 1-3 priorities aligned with Q2 goals
     - Tasks: specific actionable items for today
     - Habit Check: checkboxes for today's habits
  5. Create a new Obsidian note at "00-RAW/Inbox/daily-brief-YYYY-MM-DD.md" with the same content

  Keep it under 30 lines. Concrete, specific, no fluff.

outputs:
  - brain/daily/today.md
  - obsidian: "00-RAW/Inbox/daily-brief-{date}.md"
EOF
```

- [ ] **Step 2: Create weekly-review agent config**

```bash
cat > ~/Developer/life-maxing/brain/agents/weekly-review.yaml << 'EOF'
name: weekly-review
type: scheduled
schedule: "0 19 * * 0"   # Sunday 7pm
description: Weekly review and planning agent

prompt: |
  You are Saagii's weekly review agent. Run every Sunday evening.

  Steps:
  1. Use Obsidian MCP to search "Q1_Review_and_Q2_Planning" — get Q2 targets
  2. Read brain/habits/current.md — get habit stack
  3. Use Obsidian MCP to search Inbox for this week's daily briefs (last 7 days)
  4. Synthesize:
     - What got done this week vs Q2 targets (be honest, use ✅/❌/⏳)
     - Habit completion rate (estimate from daily briefs)
     - Top win of the week (one specific thing)
     - One thing to do differently next week (one specific change)
     - Q2 progress: are you on track? (simple red/yellow/green)
  5. Create Obsidian note at "00-RAW/Inbox/weekly-review-YYYY-Www.md"

  Keep it under 40 lines. Honest, no motivational fluff.

outputs:
  - obsidian: "00-RAW/Inbox/weekly-review-{year}-W{week}.md"
EOF
```

- [ ] **Step 3: Create content-posted trigger config**

```bash
cat > ~/Developer/life-maxing/brain/agents/content-posted.yaml << 'EOF'
name: content-posted
type: triggered
trigger: manual   # User runs this after posting content
description: Fetch IG metrics after posting and log to Obsidian

prompt: |
  You are Saagii's content metrics agent. Run after each Instagram post.

  Input required (user provides):
  - Post URL or shortcode
  - Content type (reel / carousel / story)
  - Caption hook (first line)

  Steps:
  1. Use Obsidian MCP to search "Content_Performance_Data_and_Patterns" — get baseline metrics
  2. Log the new post to Obsidian at "00-RAW/Inbox/content-log-YYYY-MM-DD.md":
     - Date, type, hook, URL
     - Note: "Check metrics in 24h — views, saves, follows"
  3. Remind user: "Come back in 24h to log views/saves/follows manually."

  Note: Real-time IG API access depends on token availability.
  If no API: log post details only, prompt user to paste metrics manually.

outputs:
  - obsidian: "00-RAW/Inbox/content-log-{date}.md"
EOF
```

- [ ] **Step 4: Verify all three configs are valid YAML**

```bash
python3 -c "import yaml; yaml.safe_load(open('brain/agents/morning-brief.yaml'))" && echo "morning-brief: OK"
python3 -c "import yaml; yaml.safe_load(open('brain/agents/weekly-review.yaml'))" && echo "weekly-review: OK"
python3 -c "import yaml; yaml.safe_load(open('brain/agents/content-posted.yaml'))" && echo "content-posted: OK"
```

Expected: all three print `OK`

- [ ] **Step 5: Commit**

```bash
cd ~/Developer/life-maxing
git add brain/agents/
git commit -m "feat: add agent YAML configs — morning-brief, weekly-review, content-posted"
```

---

## Task 7: build/ — Skill Factory Layer

**Files:**
- Create: `build/skills/README.md`
- Create: `build/skills/experiments/.gitkeep`
- Create: `build/learnings/claude-code.md`
- Create: `build/learnings/cowork-workflows.md`
- Create: `build/learnings/cloud-agents.md`
- Create: `build/templates/skill-template.md`

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p ~/Developer/life-maxing/build/skills/experiments
mkdir -p ~/Developer/life-maxing/build/learnings
mkdir -p ~/Developer/life-maxing/build/templates
touch ~/Developer/life-maxing/build/skills/experiments/.gitkeep
```

- [ ] **Step 2: Create build/skills/README.md — the skill registry**

```bash
cat > ~/Developer/life-maxing/build/skills/README.md << 'EOF'
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

EOF
```

- [ ] **Step 3: Create build/learnings/claude-code.md**

```bash
cat > ~/Developer/life-maxing/build/learnings/claude-code.md << 'EOF'
# How Claude Code Works — Living Doc

> Updated as I learn. Source: hands-on sessions + official docs.
> Last updated: 2026-04-16

## Architecture
- **Claude Code** = CLI that runs Claude in your terminal with full filesystem + tool access
- **Skills** = markdown prompt files in `~/.claude/skills/` or `.claude/skills/` that auto-trigger
- **Plugins** = bundles of skills (e.g. superpowers, claude-mem, frontend-design)
- **Hooks** = shell commands that fire on events (SessionStart, Stop, PreToolUse, PostToolUse)
- **MCP Servers** = external integrations (Obsidian, Supabase, Google Calendar, Figma, etc.)

## Skill Anatomy
```
~/.claude/skills/my-skill/
└── SKILL.md    # Trigger conditions + prompt instructions
```

Level 7 skills (like `learner`) can self-modify their Expertise section.

## Hook Events
| Event | When |
|---|---|
| `SessionStart` | When Claude Code session opens |
| `UserPromptSubmit` | Before each user message |
| `PreToolUse` | Before any tool call |
| `PostToolUse` | After any tool call (success) |
| `PostToolUseFailure` | After any tool call (failure) |
| `Stop` | When session ends |

## CLAUDE.md Loading
- Global: `~/.claude/CLAUDE.md`
- Project: `[project]/CLAUDE.md`
- Both load every session — keep them lean (<200 lines)

## Key Patterns Discovered
- Use lazy pointers in CLAUDE.md instead of full context dumps
- Skills with ≤15-word learned rules in CLAUDE.md accumulate over time
- Obsidian MCP = best bridge between human notes and Claude agents

## Open Questions
- [ ] How do Co-Work sessions hand off state between agents?
- [ ] What's the exact YAML format for cloud scheduled tasks?
- [ ] Can skills trigger other skills programmatically?

EOF
```

- [ ] **Step 4: Create build/learnings/cowork-workflows.md**

```bash
cat > ~/Developer/life-maxing/build/learnings/cowork-workflows.md << 'EOF'
# Co-Work Workflows — Living Doc

> Claude Co-Work = multiple Claude agents collaborating on a shared task.
> Updated as I experiment. Last updated: 2026-04-16

## What Is Co-Work
Co-Work lets you spin up multiple Claude agents that can see each other's outputs
and coordinate on complex tasks. Think of it as a small team of AIs.

## Use Cases for Life Maxing
- [ ] Morning brief: one agent reads Obsidian, another formats + writes the brief
- [ ] Skill building: one agent experiments, another reviews + improves the skill
- [ ] Content pipeline: one agent drafts script, another critiques for hook quality
- [ ] Agency work: one agent analyzes client needs, another builds the automation

## Setup
TBD — explore via `anthropic-skills:setup-cowork` skill.

## Patterns Discovered
(Populate as experiments run)

## Resources
- Setup skill: `anthropic-skills:setup-cowork`
- Docs: build/learnings reference (check context7 for latest)

EOF
```

- [ ] **Step 5: Create build/learnings/cloud-agents.md**

```bash
cat > ~/Developer/life-maxing/build/learnings/cloud-agents.md << 'EOF'
# Cloud Agents & Scheduled Tasks — Living Doc

> Managed agents that run on a cron schedule in the cloud.
> Updated as I experiment. Last updated: 2026-04-16

## What Are Cloud Agents
Agents that run remotely on Anthropic's infrastructure on a schedule.
You define a prompt + cron schedule, they run without your machine being open.

## Our Scheduled Agents
| Agent | Schedule | Config |
|---|---|---|
| morning-brief | 9am daily | brain/agents/morning-brief.yaml |
| weekly-review | Sunday 7pm | brain/agents/weekly-review.yaml |
| content-posted | Manual trigger | brain/agents/content-posted.yaml |

## Setup Commands
Use the `anthropic-skills:schedule` skill or `/schedule` to register agents:
```
/schedule
```
Then point at the YAML config in brain/agents/.

## Patterns Discovered
(Populate as agents are deployed and iterated)

## Open Questions
- [ ] How to pass dynamic inputs (today's date) to scheduled agent prompts?
- [ ] Can agents write directly to Obsidian via MCP when running in cloud?
- [ ] What's the retry behavior on agent failure?

EOF
```

- [ ] **Step 6: Create build/templates/skill-template.md**

```bash
cat > ~/Developer/life-maxing/build/templates/skill-template.md << 'EOF'
---
name: [skill-name]
description: [One line — what does this skill do?]
level: 5
trigger: [when should this auto-activate?]
status: experiment   # experiment | project-local | global
---

# [Skill Name]

## Trigger
Activate this skill when: [describe the situation that needs this skill]

## What It Does
[2-3 sentences. Be specific about inputs → outputs.]

## Steps

1. [First thing Claude does]
2. [Second thing]
3. [Output / result]

## Example Usage
```
[Example prompt that triggers this skill]
```

## Self-Improvement Annotation
After using this skill, ask: "What ≤15-word rule would make this skill faster next time?"
If found: append to CLAUDE.md → ## Learned Instructions AND update this file's Steps section.

## Promotion Checklist
- [ ] Tested in at least 2 real sessions
- [ ] Self-improvement annotation used at least once
- [ ] Documented in build/skills/README.md
- [ ] Promoted to .claude/skills/ (project-local) or ~/.claude/skills/ (global)

EOF
```

- [ ] **Step 7: Verify**

```bash
ls ~/Developer/life-maxing/build/
# Expected: learnings/  skills/  templates/
ls ~/Developer/life-maxing/build/skills/
# Expected: README.md  experiments/
ls ~/Developer/life-maxing/build/learnings/
# Expected: claude-code.md  cloud-agents.md  cowork-workflows.md
```

- [ ] **Step 8: Commit**

```bash
cd ~/Developer/life-maxing
git add build/
git commit -m "feat: add build/ skill factory — registry, learnings, templates"
```

---

## Task 8: ship/ — Content + Business Layer

**Files:**
- Create: `ship/ig-series/README.md`
- Create: `ship/ig-series/ep-01/notes.md`
- Create: `ship/agency/README.md`
- Create: `ship/agency/playbooks/.gitkeep`
- Create: `ship/agency/clients/.gitkeep`

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p ~/Developer/life-maxing/ship/ig-series/ep-01
mkdir -p ~/Developer/life-maxing/ship/agency/playbooks
mkdir -p ~/Developer/life-maxing/ship/agency/clients
touch ~/Developer/life-maxing/ship/agency/playbooks/.gitkeep
touch ~/Developer/life-maxing/ship/agency/clients/.gitkeep
```

- [ ] **Step 2: Create ship/ig-series/README.md**

```bash
cat > ~/Developer/life-maxing/ship/ig-series/README.md << 'EOF'
# Life Maxing with AI — First $1K
**Instagram Series by @Saagii**

## Concept
Document building AI automations that manage my life AND make money.
Show the real process: idea → skill built → how it works → what I made from it.

## Format
- Platform: Instagram Reels (primary) + carousel breakdowns
- Length: 50 seconds max per reel
- Day: flexible, but consistent
- Hook style: start with the unexpected result, then explain how

## Episode Structure (per folder)
```
ep-XX/
├── notes.md       # What was built, why, key moments
├── script.md      # Reel script (hook + body + CTA)
└── assets/        # Screenshots, screen recordings
```

## Episodes

| # | Title | What Was Built | Status |
|---|---|---|---|
| 01 | Building My AI Life OS | Life Maxing OS workspace scaffold | 🎬 Filming |

## Revenue Tracking
| Source | Amount | From Episode |
|---|---|---|
| (none yet) | — | — |

**Target:** $1,000 by end of Q2 2026

EOF
```

- [ ] **Step 3: Create ship/ig-series/ep-01/notes.md**

```bash
cat > ~/Developer/life-maxing/ship/ig-series/ep-01/notes.md << 'EOF'
# Episode 01 — Building My AI Life OS

**Date:** 2026-04-16
**Status:** 🎬 Filming
**Built:** Life Maxing OS workspace scaffold

## What Was Built
A Claude Code meta-workspace that runs my life:
- brain/ layer — agents read my Obsidian vault and write daily briefs
- build/ layer — skill factory where I build and test custom AI workflows
- ship/ layer — this series + agency playbooks
- Self-improving CLAUDE.md that grows smarter each session

## The Hook
"I built an AI system that reads my life goals every morning and tells me exactly what to do."

## Key Moments to Show
1. The brain/vault symlink — Obsidian second brain wired directly into Claude
2. CLAUDE.md under 200 lines — lazy pointers not data dumps
3. morning-brief agent YAML — scheduled to run at 9am
4. The Stop hook — Claude reminds itself to learn at session end

## What's Next (ep-02 preview)
Building the first custom skill — morning-brief goes live.

## Script
See script.md (TBD — draft after filming the build session)

EOF
```

- [ ] **Step 4: Create ship/agency/README.md**

```bash
cat > ~/Developer/life-maxing/ship/agency/README.md << 'EOF'
# AI Automation Agency — Mongolia

## Concept
Use everything learned building Life Maxing OS to automate Mongolian businesses with Claude.
Each personal skill I build = potential service offering.

## Target Clients
- Small businesses in Mongolia needing automation (scheduling, customer responses, reporting)
- Mongolian businesses with repetitive backend workflows
- Content creators needing AI-assisted production pipelines

## Service Model
1. Audit business workflows (what's repeated? what takes hours?)
2. Build a Claude automation for the top 1-2 workflows
3. Deliver + train client
4. Monthly retainer for maintenance + new automations

## Pricing (draft)
- Discovery call: free
- Single automation: $200–500
- Monthly retainer: $150–300/month

## Playbooks
As automations are proven in personal use → extract to `playbooks/` → offer to clients.

## Clients
| Client | Status | Revenue |
|---|---|---|
| (none yet) | — | — |

## Q2 Target
3 clients, $500–1000 revenue

EOF
```

- [ ] **Step 5: Verify**

```bash
ls ~/Developer/life-maxing/ship/
# Expected: agency/  ig-series/
ls ~/Developer/life-maxing/ship/ig-series/
# Expected: README.md  ep-01/
```

- [ ] **Step 6: Commit**

```bash
cd ~/Developer/life-maxing
git add ship/
git commit -m "feat: add ship/ content + business layer — IG series ep-01 + agency"
```

---

## Task 9: projects/ — Placeholder for LifeOS iOS

**Files:**
- Create: `projects/lifeos-ios/README.md`

- [ ] **Step 1: Create directory + placeholder**

```bash
mkdir -p ~/Developer/life-maxing/projects/lifeos-ios

cat > ~/Developer/life-maxing/projects/lifeos-ios/README.md << 'EOF'
# LifeOS iOS

**Status:** Planned — Q2 target: MVP by June 15, 2026

## What It Is
Native iOS/macOS app version of LifeOS.
The web version (Next.js + Tldraw) is ~75% complete at ~/Developer/lifeos/.

## When to Start
After the Life Maxing OS workspace is stable and the first Co-Work/agent systems are proven.

## Links
- Web version: ~/Developer/lifeos/
- PRD: ~/Developer/lifeos/PRD.md
- Q2 plan: brain/goals/Q2.md

## Stack (planned)
- Swift / SwiftUI
- CloudKit or Supabase
- TBD based on what's learned from web version

EOF
```

- [ ] **Step 2: Commit**

```bash
cd ~/Developer/life-maxing
git add projects/
git commit -m "feat: add projects/lifeos-ios placeholder"
```

---

## Task 10: Final Verification

- [ ] **Step 1: Full structure check**

```bash
find ~/Developer/life-maxing -not -path '*/\.*' -not -path '*/brain/vault/*' | sort
```

Expected output includes:
```
life-maxing/CLAUDE.md
life-maxing/README.md
life-maxing/brain/agents/content-posted.yaml
life-maxing/brain/agents/morning-brief.yaml
life-maxing/brain/agents/weekly-review.yaml
life-maxing/brain/daily/today.md
life-maxing/brain/goals/Q2.md
life-maxing/brain/habits/current.md
life-maxing/build/learnings/claude-code.md
life-maxing/build/learnings/cloud-agents.md
life-maxing/build/learnings/cowork-workflows.md
life-maxing/build/skills/README.md
life-maxing/build/templates/skill-template.md
life-maxing/docs/superpowers/plans/2026-04-16-life-maxing-os-scaffold.md
life-maxing/docs/superpowers/specs/2026-04-15-life-maxing-os-design.md
life-maxing/projects/lifeos-ios/README.md
life-maxing/ship/agency/README.md
life-maxing/ship/ig-series/README.md
life-maxing/ship/ig-series/ep-01/notes.md
```

- [ ] **Step 2: Verify CLAUDE.md is under 200 lines**

```bash
wc -l ~/Developer/life-maxing/CLAUDE.md
# Expected: <50
```

- [ ] **Step 3: Verify vault symlink is live**

```bash
ls ~/Developer/life-maxing/brain/vault/ | head -5
# Expected: folder contents of Obsidian second brain
```

- [ ] **Step 4: Verify git log**

```bash
cd ~/Developer/life-maxing && git log --oneline
# Expected: 7-8 commits from this scaffold session
```

- [ ] **Step 5: Final commit — docs**

```bash
cd ~/Developer/life-maxing
git add docs/
git commit -m "docs: add design spec and implementation plan"
```

---

## Task 11: Register Scheduled Agents (Post-Scaffold)

> This task runs AFTER the scaffold is complete. Uses the `anthropic-skills:schedule` skill.

- [ ] **Step 1: Open schedule skill**

In Claude Code, run:
```
/oh-my-claudecode:schedule
```
or invoke: `anthropic-skills:schedule`

- [ ] **Step 2: Register morning-brief**

Point the skill at `brain/agents/morning-brief.yaml`.
Schedule: `0 9 * * *` (9am daily)

- [ ] **Step 3: Register weekly-review**

Point at `brain/agents/weekly-review.yaml`.
Schedule: `0 19 * * 0` (Sunday 7pm)

- [ ] **Step 4: Verify agents appear in schedule list**

```
/schedule list
```

Expected: morning-brief and weekly-review listed as active.

---

## Self-Review

**Spec coverage check:**
- ✅ brain/build/ship structure → Tasks 4, 7, 8
- ✅ CLAUDE.md <200 lines, lazy pointers → Task 2
- ✅ Learned Instructions section + Stop hook → Tasks 2, 3
- ✅ Obsidian vault symlink → Task 5
- ✅ Agent YAML configs (3 agents) → Task 6
- ✅ Skill registry + template → Task 7
- ✅ IG series ep-01 notes → Task 8
- ✅ Agency structure → Task 8
- ✅ LifeOS iOS placeholder → Task 9
- ✅ Scheduled agent registration → Task 11

**Placeholder scan:** No TBDs in structural files. cowork-workflows.md and cloud-agents.md have "TBD — explore via skill" which is intentional (living docs, not specs).

**Type consistency:** No code types — all markdown/YAML, no cross-file type references.

Plan is complete and self-consistent.
