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
