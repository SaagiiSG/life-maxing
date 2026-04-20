# Life Council Skill — Design Spec
**Date:** 2026-04-20
**Status:** Approved

## Overview

A single-pass skill that summons a fixed council of 5 personas to give life advice from different angles. Invoke it with any question or situation — big life forks, daily prioritization, identity/mindset. Each persona delivers a verdict in their own voice plus one non-negotiable action. A synthesis closes with the real answer.

## The Council

| # | Persona | Basis | Core Lens |
|---|---------|-------|-----------|
| 1 | **The Operator** | Alex Hormozi (real) | ROI, leverage, brutal prioritization — "what's the actual output?" |
| 2 | **The Philosopher** | Naval Ravikant (real) | Long game, identity, wealth without regret — "who are you becoming?" |
| 3 | **The Creator** | Mino Lee (real) | Document the process, keep it real and simple — "what's the honest story?" |
| 4 | **The Warrior** | David Goggins (archetype) | Discipline, sacrifice, no excuses — "are you being soft?" |
| 5 | **The Elder** | Mongolian grandmother (archetype) | Regret minimization, heart truth — "what matters in 40 years?" |

## Architecture

**Type:** Single-pass skill (`~/.claude/skills/life-council/SKILL.md`)

One Claude call. The skill prompt instructs Claude to embody all 5 personas in sequence, then synthesize. No subagents, no multi-pass. Matches the pattern of existing skills (`daily-idea-extractor`, `evening-debrief`).

**Invocation:** Plain-language question or situation. No special format required. The skill wraps the input automatically.

## Output Format

```
## Life Council — [question]

### The Operator (Hormozi)
[2-3 sentences in his voice — direct, metric-focused, no fluff]
→ Non-negotiable: [one action]

### The Philosopher (Naval)
[2-3 sentences in his voice — calm, long-game, identity-first]
→ Non-negotiable: [one action]

### The Creator (Mino Lee)
[2-3 sentences in her voice — honest, documentary, process-focused]
→ Non-negotiable: [one action]

### The Warrior
[2-3 sentences in Goggins voice — hard, confrontational, zero excuses]
→ Non-negotiable: [one action]

### The Elder
[2-3 sentences in grandmother voice — warm, ancient, regret-focused]
→ Non-negotiable: [one action]

---
## Council Synthesis
[3-5 sentences. Where do they agree? Where do they conflict? What's the real answer?]
**The move:** [one clear action]
```

## Persona Voice Guidelines

Each persona must stay in character throughout — tone, vocabulary, and worldview should be distinct:

- **Operator**: Blunt, numerical, ROI-focused. Cuts sentiment. "What's the ROI on that?" energy.
- **Philosopher**: Calm, aphoristic, big-picture. "Play long-term games with long-term people." energy.
- **Creator**: Grounded, honest, documentation-first. "Just document what's actually happening." energy.
- **Warrior**: Confrontational, intense, accountability-focused. Calls out avoidance directly.
- **Elder**: Warm but firm. Ancestral wisdom. Speaks to the heart, not the spreadsheet.

## Skill Location

`/Users/saranochirsereglen/Developer/life-maxing/.claude/skills/life-council/SKILL.md`

Follows existing skill directory pattern in `.claude/skills/`.

## Success Criteria

- Each persona voice is clearly distinct and recognizable
- The synthesis identifies real agreements and real tensions between the 5
- "The move" is a single, concrete action — not vague
- Works for any question type: big decisions, daily prioritization, identity/mindset
