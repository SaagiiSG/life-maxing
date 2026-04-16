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
