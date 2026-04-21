---
name: evening-debrief
description: >
  Evening debrief routine. Fires when user says: "lets wrap up today",
  "wrap up today", "let's wrap up", "end of day", "daily wrap", or any
  clear signal they want to close out the day and review their tasks.
---

# Evening Debrief

You are running the evening debrief routine for Saagii's Life Maxing OS.

## Notion Config
Daily Tasks Database ID: `af46a778-874c-45e7-8f87-b94609c880b4`
Data Source ID: `71a77568-d779-42df-90fc-d8c5b402bdaa`
Lifemaxxing Page ID: `349844cd-69d1-8114-b2d0-f265146bd457`

## Step 1: Fetch today's Notion tasks

1. Get today's date from the `currentDate` system context (format: YYYY-MM-DD).
2. Use `mcp__claude_ai_Notion__notion-fetch` on the database URL:
   `https://www.notion.so/af46a778874c45e78f87b94609c880b4`
3. From the results, collect all pages where `date:Date:start` equals today's date.
4. For each page, record:
   - `name` — the task title (Name property)
   - `done` — true if Done checkbox is checked, false otherwise
   - `area` — the Area select value (Business / Content / Academic / Physique / Habits)
   - `type` — the Type select value (Habit / Task)

If zero tasks exist for today, tell the user: "No tasks logged for today. Did you run /routines?" and stop.

Build two lists:
- `done_tasks` — all tasks where done = true
- `undone_tasks` — all tasks where done = false

After building the two lists, also set:
- `total` = len(done_tasks) + len(undone_tasks)

## Step 2: Calculate and update XP

**XP rules:**
- Each done task = +10 XP
- All tasks done (0 undone) = +50 bonus
- Each done Academic area task = +5 bonus
- Each done Habit type task that is done = +5 bonus

**Calculation:**
```
xp_today = (len(done_tasks) × 10)
           + (5 × count of done tasks where area == "Academic")
           + (5 × count of done tasks where type == "Habit")
if len(undone_tasks) == 0: xp_today += 50
```

**Read XP state from Obsidian:**
Use Obsidian MCP to read `00-RAW/gamification/state.md`.
If the file doesn't exist, create it with:
```
xp: 0
level: 1
level_name: Beginner
streak: 0
last_debrief_date: null
```

**Update state:**
- `xp` += xp_today
- Streak: if `last_debrief_date` == yesterday → streak += 1, else reset to 1
- Streak milestone: if streak % 7 == 0 → xp_today += 100 (weekly streak bonus)
- Level thresholds: Lv1 Beginner 0–499 | Lv2 Builder 500–999 | Lv3 Grinder 1000–1999 | Lv4 Creator 2000–3999 | Lv5 Machine 4000+
- Build XP bar (10 chars): ████░░░░░░ — filled = floor((xp % level_range) / level_range × 10)
- `last_debrief_date` = today's date

## Step 3: Accountability check

Display the undone tasks list to the user:

```
❌ You didn't finish these today:
  • [task name] (Area)
  • [task name] (Area)
  ...
```

If `undone_tasks` is empty, skip this step and tell the user: "🎯 Perfect day — everything done!"

For each undone task, ask the user (grouped is fine):
"What happened with these? For each one, reply: done / tomorrow / drop"

Process their answers:
- **done** → use `mcp__claude_ai_Notion__notion-update-page` to set Done = true on that page. Add 10 XP to xp_today (re-check all-done bonus).
- **tomorrow** → use `mcp__claude_ai_Notion__notion-create-pages` to create a copy with tomorrow's date (same Name, Area, Type; Done = false).
- **drop** → acknowledge, no Notion change.

After processing, update:
- `done_final` = len(done_tasks) + count of tasks where user answered "done" in this step

**Write final XP state:**
Now that all XP mutations from Steps 2 and 3 are complete, write the updated values to `00-RAW/gamification/state.md` via Obsidian MCP:
- `xp` = previous_xp + xp_today (including any additions from Step 3 task ticks)
- `streak`, `last_debrief_date`, `level`, `level_name` as calculated in Step 2

Before writing, compute:
- `old_level` = the level determined at the START of Step 2 (before adding xp_today)
- `new_level` = the level determined from the new total XP
- If `new_level > old_level`, set `level_up = true` (used in Step 5 summary)

## Step 4: Plan tomorrow

1. Get tomorrow's date (today + 1 day, format YYYY-MM-DD).
2. Fetch tomorrow's existing Notion tasks (same approach as Step 1, using tomorrow's date).
   - If tasks exist, show them: "📅 Already on tomorrow's list: [task names]"
   - If none: "Tomorrow's list is empty."
3. Include any tasks moved from Step 3 ("tomorrow" answers) — already created, confirm them.
4. Ask the user: **"What else goes on tomorrow? List tasks (or say 'done' to skip)."**
5. For each task the user provides, infer the Area:
   - Mentions "gym", "workout", "physique" → Physique
   - Mentions "post", "content", "IG", "film", "script" → Content
   - Mentions "SAT", "university", "academic", "tutor", "Duolingo" → Academic
   - Mentions "journal", "pray", "read", "routine", "habit", "night routine", "log" → Habits
   - Everything else → Business
6. Create each task in Notion: `data_source_id: 71a77568-d779-42df-90fc-d8c5b402bdaa`, tomorrow's date, Type = "Task", Done = false.
7. Run the calendar blocker for tomorrow's new tasks:
   - Use `mcp__claude_ai_Google_Calendar__list_events` to fetch tomorrow's existing events (calendarId: primary, Asia/Ulaanbaatar timezone).
   - Area → time block mapping:
     | Area     | Window             | Duration | Title format                  |
     |----------|--------------------|----------|-------------------------------|
     | Academic | 10:00am – 12:00pm  | 90 min   | `🔴 [Deep Work] {task}`      |
     | Business | 10:00am – 12:00pm  | 90 min   | `🟡 [Work] {task}`           |
     | Content  | 5:00pm – 8:00pm    | 45 min   | `📱 [Content] {task}`        |
     | Habits   | 11:00pm – midnight | 30 min   | `⚙️ [Routine] {task}`       |
     | Physique | skip               | —        | Fixed recurring — never touch |
   - Never overwrite existing events. 10-minute buffer between consecutive blocks.
   - Never touch "Flowers" or "Gym" events.
   - Create GCal events using `mcp__claude_ai_Google_Calendar__create_event` (calendarId: primary, Asia/Ulaanbaatar timezone).

## Step 5: Final summary

Print one clean block:

```
✅ Day wrapped!

📋 Today: [done_final] / [total] tasks done
🎮 XP → +[xp_today] XP earned (total: [new_xp] | Level [level] — [level_name])
   [xp_bar]
🔥 Streak: [streak] days
[if level up: 🎉 LEVEL UP! You're now Level [level] — [level_name]!]

📅 Tomorrow ([YYYY-MM-DD]):
[list of tomorrow's confirmed tasks with time blocks if scheduled]

[if any tasks couldn't be scheduled: ⚠️ Couldn't schedule: {task} — window full]

Good night. 🌙
```

Notes:
- Do not add motivational fluff beyond the template above.
- If tomorrow's list is empty: show "Tomorrow: nothing planned yet."
- Keep it under 20 lines total.
