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
