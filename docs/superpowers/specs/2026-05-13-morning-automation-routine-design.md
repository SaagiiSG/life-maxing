# Morning Automation Routine — Design Spec

**Date:** 2026-05-13
**Status:** Approved

## Summary

A Claude Code cloud routine that runs at 5am Ulaanbaatar time (UTC+8) every day.
It fully replaces the manual evening-debrief skill. The user never needs to open Notion or Google Calendar — Slack is the single pane of glass.

## Schedule

- Cron: `0 21 * * *` (UTC) = 5:00am UTC+8
- Trigger: cloud routine (remote agent)
- Channel: `#all-saagii` on saagii.slack.com

---

## Notion Config

- Daily Tasks Database ID: `af46a778-874c-45e7-8f87-b94609c880b4`
- Lifemaxxing Page ID: `349844cd-69d1-8114-b2d0-f265146bd457`
- Data Source ID: `71a77568-d779-42df-90fc-d8c5b402bdaa`

## Obsidian Config

- Dashboard: `brain/vault/Dashboard.md`
- Gamification state: `brain/vault/00-RAW/gamification/state.md`
- Today's plan: `brain/daily/today.md`

---

## Step 1 — Wrap up yesterday

1. Derive `yesterday` = today's date (UTC+8) minus 1 day.
2. Fetch Notion Daily Tasks where `date = yesterday`.
3. Split into `done_tasks` (Done = true) and `undone_tasks` (Done = false).
4. Calculate XP:
   - Per task: base 10 + 5 if Academic + 5 if Habit (use XP property if set, else formula)
   - If `undone_tasks` is empty: add 50 bonus
5. Read `brain/vault/00-RAW/gamification/state.md`:
   - Add XP earned to `xp`
   - Streak: if `last_debrief_date == yesterday - 1` → streak + 1, else reset to 1
   - Streak milestone: if `streak % 7 == 0` → add 100 bonus XP
   - Level thresholds: Lv1 Beginner 0–499 | Lv2 Builder 500–999 | Lv3 Grinder 1000–1999 | Lv4 Creator 2000–3999 | Lv5 Machine 4000+
   - XP bar (10 chars): filled = floor((xp % level_range) / level_range × 10)
   - Set `last_debrief_date = yesterday`
6. Write updated state back to `brain/vault/00-RAW/gamification/state.md`.
7. Update `brain/vault/Dashboard.md` with new XP, level, streak, bar, and yesterday's summary row.
8. Update the Notion Lifemaxxing page with yesterday's task summary.

---

## Step 2 — Plan today on GCal

1. Fetch Notion Daily Tasks where `date = today`.
   - If none exist: create them using `brain/daily/today.md` task bullets + default habits (same logic as routines skill).
2. Fetch today's Google Calendar events.
3. Identify **locked events** — do not move or modify these:
   - Events with "flower", "flowers", "gym", "workout" in the title (case-insensitive)
   - Any event already created by this routine (to avoid duplicates on re-run)
4. Build a free-slot timeline from 9:00am to 9:00pm, removing locked blocks.
5. For each Notion task, estimate duration:
   - **Hard task** (title contains: film, record, script, shoot, edit, build, code, develop, implement, "learning ai", "ai course", "learn ai") → 90–120 minutes (use 110 min as default)
   - **Regular task** → 40 minutes
   - Add a 10-minute buffer between tasks
6. Assign tasks to free slots in order. If tasks overflow the day, note the overflow in the Slack message.
7. Create GCal events for each assigned task. Title = Notion task name. Description = Area + Type.

---

## Step 3 — Post Slack summary to #all-saagii

Plain text, no emojis. Format:

```
Good morning, Saagii.

--- Yesterday (YYYY-MM-DD) ---
Done (X/Y): Task A, Task B, Task C
Missed: Task D, Task E
XP: +35 -> 865 total | Level 2 Builder [########..] | 4-day streak

--- Today's Schedule ---
09:00 - 09:40   Business outreach [Business]
09:50 - 11:40   Film AI video ep.02 [Content] [hard]
[11:30 - 13:00  Gym -- locked]
13:00 - 14:50   Build Quartz onboarding [Business] [hard]
15:00 - 15:40   SAT prep session [Academic]
```

If any step fails, still post to Slack with a note like:
```
WARNING: Step 2 (GCal planning) failed — [error summary]. Steps 1 and 3 completed normally.
```

---

## Routine Prompt (paste into Claude Code Routines)

```
You are running Saagii's 5am daily automation. Execute all three steps. Use UTC+8 as the local timezone for all date calculations.

## Config
- Notion Daily Tasks DB: af46a778-874c-45e7-8f87-b94609c880b4
- Notion Lifemaxxing Page: 349844cd-69d1-8114-b2d0-f265146bd457
- Obsidian Dashboard: brain/vault/Dashboard.md
- Obsidian Gamification State: brain/vault/00-RAW/gamification/state.md
- Today's plan file: brain/daily/today.md
- Slack channel: #all-saagii on saagii.slack.com

## Step 1 — Wrap up yesterday
1. Derive yesterday's date (today UTC+8 minus 1 day).
2. Fetch all Notion pages from the Daily Tasks DB where date property = yesterday.
3. Split into done_tasks (Done = true) and undone_tasks (Done = false).
4. Calculate xp_earned:
   - For each done task: use XP property if set, else: base 10 + 5 if Area=Academic + 5 if Type=Habit
   - If undone_tasks is empty: add 50 bonus
5. Read brain/vault/00-RAW/gamification/state.md. If missing, initialize with xp:0, level:1, level_name:Beginner, streak:0, last_debrief_date:null.
6. Update state:
   - xp += xp_earned
   - streak: if last_debrief_date == day before yesterday -> streak+1, else reset to 1
   - if streak % 7 == 0: xp += 100
   - Level thresholds: Lv1 Beginner 0-499 | Lv2 Builder 500-999 | Lv3 Grinder 1000-1999 | Lv4 Creator 2000-3999 | Lv5 Machine 4000+
   - XP bar (10 chars, # for filled, . for empty): filled = floor((xp % level_range) / level_range * 10)
   - last_debrief_date = yesterday
7. Write state back to brain/vault/00-RAW/gamification/state.md.
8. Update brain/vault/Dashboard.md: new XP total, level, streak, XP bar, add a row to the daily log table for yesterday.
9. Update the Notion Lifemaxxing page (349844cd-69d1-8114-b2d0-f265146bd457) with yesterday's task counts and XP.

## Step 2 — Plan today on GCal
1. Fetch Notion pages from the Daily Tasks DB where date = today. If none exist, read brain/daily/today.md, extract task bullets, create Notion pages using the routines skill logic (infer Area, calculate XP, add default habits).
2. Fetch today's Google Calendar events.
3. Mark as locked (do not move): any event with "flower", "flowers", "gym", or "workout" in the title (case-insensitive), and any event whose description contains "[morning-routine-auto]".
4. Build a free timeline from 09:00 to 21:00 today (UTC+8), removing locked blocks.
5. Estimate task durations:
   - Hard task keywords (case-insensitive): film, record, script, shoot, edit, build, code, develop, implement, "learning ai", "ai course", "learn ai" -> 110 minutes
   - All other tasks -> 40 minutes
   - Add 10-minute gap between tasks
6. Assign tasks to free slots in chronological order. Note any overflow tasks that did not fit.
7. Create a Google Calendar event for each assigned task. Title = task name. Description = "[morning-routine-auto] Area: <area>". Do not create duplicates — check if a matching event already exists first.

## Step 3 — Post to Slack
Post a plain text message (no emojis) to #all-saagii on saagii.slack.com in this format:

Good morning, Saagii.

--- Yesterday (<date>) ---
Done (<X>/<total>): <comma-separated task names>
Missed: <comma-separated task names, or "none">
XP: +<xp_earned> -> <new_total> total | Level <N> <name> [<bar>] | <streak>-day streak

--- Today's Schedule ---
<HH:MM> - <HH:MM>   <task name> [<area>]<optionally " [hard]">
<HH:MM> - <HH:MM>   <locked event name> -- locked
...
<If overflow:>
Did not fit: <task names>

If any step fails, still post to Slack with a plain text warning noting which step failed and why. Always attempt all steps.
```

---

## GitHub Requirement

Claude Code cloud routines require the project to be in a GitHub repository. The routine prompt is self-contained and references file paths relative to the repo root. Ensure `brain/`, `docs/`, and `brain/daily/today.md` are committed and not gitignored.

---

## What This Replaces

- Manual `/evening-debrief` skill invocation
- Manual dashboard updates to `brain/vault/Dashboard.md`
- Manual Notion Lifemaxxing page updates
- Manual GCal time-blocking
