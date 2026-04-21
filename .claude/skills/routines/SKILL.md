---
name: routines
description: >
  Daily routines manager. Fires when user says "/routines", "show my routines",
  "what's on my list", "set up today's tasks", or "what do I have today".
  Populates Notion with today's tasks and habits if empty, then displays live status.
---

# Daily Routines

You are running the routines skill for Saagii's Life Maxing OS.

## Notion Config
Daily Tasks Database ID: `af46a778-874c-45e7-8f87-b94609c880b4`

## Step 1: Get context

- Today's date: use `currentDate` from system context (format: YYYY-MM-DD)
- Day of week: derive from date (1=Mon, 2=Tue, 3=Wed, 4=Thu, 5=Fri, 6=Sat, 7=Sun)
- Read `brain/daily/today.md` — extract bullet points under `## Tasks`

## Step 2: Query Notion for today's entries

Use `mcp__claude_ai_Notion__notion-fetch` to query the database.
The database URL is: `https://www.notion.so/af46a778874c45e78f87b94609c880b4`

Fetch the database and look for pages where the Date property equals today's date.

If rows already exist for today → skip to Step 5 (display only).
If no rows exist for today → proceed to Step 3.

## Step 3: Populate today's tasks

Create Notion pages using `mcp__claude_ai_Notion__notion-create-pages`.

**Always create (every day):**
| Task | Area | Type |
|---|---|---|
| Content post (IG) | Content | Habit |
| Night routine (log, read, journal, pray) | Habits | Habit |

**Gym days only (Mon / Tue / Thu / Fri — day of week 1, 2, 4, 5):**
| Task | Area | Type |
|---|---|---|
| Gym session | Physique | Habit |

**Focus tasks from today.md:**
Read `brain/daily/today.md`. Extract each bullet under `## Tasks` (strip the leading `- [ ] ` or `- [x] `). For each task, infer the Area:
- Mentions "gym", "workout", "physique" → Physique
- Mentions "post", "content", "IG", "film", "script" → Content
- Mentions "SAT", "university", "academic", "tutor", "Duolingo" → Academic
- Everything else → Business

Create each page with:
```json
{
  "parent": { "database_id": "af46a778-874c-45e7-8f87-b94609c880b4" },
  "properties": {
    "Name": { "title": [{ "type": "text", "text": { "content": "<task name>" } }] },
    "Date": { "date": { "start": "<YYYY-MM-DD>" } },
    "Done": { "checkbox": false },
    "Area": { "select": { "name": "<area>" } },
    "Type": { "select": { "name": "<type>" } }
  }
}
```

Avoid duplicating habit tasks if they already appear in today.md's task list.

## Step 4: Create Google Calendar time blocks

Only run this step when new Notion tasks were just created in Step 3. If tasks already existed (skipped to Step 5), skip this step too.

Use `mcp__claude_ai_Google_Calendar__list_events` to fetch today's existing events.
- `calendarId`: `primary`
- Time range: today 00:00 – today 23:59 (local time, Asia/Ulaanbaatar, UTC+8)

Build a list of occupied time ranges from the results.

**Time-block mapping (cognitive load hierarchy):**

| Area     | Target window      | Duration | Title format                        |
|----------|--------------------|----------|-------------------------------------|
| Academic | 10:00am – 12:00pm  | 90 min   | `🔴 [Deep Work] {task name}`       |
| Business | 10:00am – 12:00pm  | 90 min   | `🟡 [Work] {task name}`           |
| Content  | 5:00pm – 8:00pm    | 45 min   | `📱 [Content] {task name}`        |
| Habits   | 11:00pm – midnight | 30 min   | `⚙️ [Routine] {task name}`       |
| Physique | skip               | —        | Already a fixed recurring event — never touch |

**Scheduling rules:**
- Work window: 10:00am – midnight only. Never book outside this range.
- Never overwrite existing events.
- Never create events that overlap "Flowers" or "Gym" recurring events.
- 10-minute buffer between consecutive blocks in the same window.
- If multiple tasks share the same window, sequence them back-to-back (with 10 min buffer) within the window.
- If a task's window is fully occupied, skip it and flag it in Step 5's output.

**For each task (excluding Physique):**
1. Look up its target window from the table above.
2. Find the first available slot in that window that fits the duration, accounting for existing events and 10-min buffers.
3. Create the GCal event using `mcp__claude_ai_Google_Calendar__create_event`:
   - `calendarId`: `primary`
   - `summary`: title per format above
   - `start`: ISO 8601 datetime (e.g. `2026-04-21T10:00:00+08:00`)
   - `end`: ISO 8601 datetime (start + duration)

Track which tasks got blocks and which couldn't be scheduled (window full).

## Step 5: Display current status

After fetching all today's tasks (whether pre-existing or just created), display:

```
📋 Today's Routines — [Weekday, Month DD]

Physique
  ✅ Gym session        ← Done = true
  ☐  Gym session        ← Done = false

Habits
  ✅ Night routine (log, read, journal, pray)
  ☐  Content post (IG)

Business
  ☐  Pick next Claude/Cowork system to build

[X / Y done]  →  tick tasks off in Notion on your phone. Run /routines again to refresh.
```

Rules:
- Use ✅ for Done = true, ☐ for Done = false
- Group by Area (Physique → Business → Content → Academic → Habits order)
- Show total completion fraction at bottom
- If all done: show "🎯 All done for today!"

If Step 4 ran (new tasks were created), append a calendar summary after the completion fraction:

```
📅 Calendar blocks created:
  🔴 [Deep Work] Review university offers — 10:00am–11:30am
  🟡 [Work] Check Cowork systems — 11:40am–1:10pm
  📱 [Content] Content post (IG) — 5:00pm–5:45pm
  ⚙️ [Routine] Night routine — 11:00pm–11:30pm
```

If any task couldn't fit: `⚠️ Couldn't schedule: {task name} — window full`
If Step 4 was skipped (tasks pre-existed): omit the calendar section entirely.

## Step 6: Update progress bar in Notion

After displaying status, update the progress bar callout on the Habit Tracker page.

**Calculate:**
- `done` = count of today's tasks where Done = true
- `total` = total count of today's tasks
- `pct` = round(done / total * 100)
- `bar` = `done` filled blocks (🟩) + `(total - done)` empty blocks (⬜), up to `total` blocks max

Example: 2/5 done → `🟩🟩⬜⬜⬜  2 / 5 done (40%)`

**Update the Habit Tracker page** (`fa0dd361-4960-4c30-a7b2-6b3374c7029f`) using `mcp__claude_ai_Notion__notion-update-page` with `update_content` command:

```json
{
  "page_id": "fa0dd361-4960-4c30-a7b2-6b3374c7029f",
  "command": "update_content",
  "content_updates": [
    {
      "old_str": "\t**Today's Progress — <previous date>**\n\t<previous bar>",
      "new_str": "\t**Today's Progress — <YYYY-MM-DD>**\n\t<bar>  <done> / <total> done (<pct>%)"
    }
  ]
}
```

Match the old_str by looking for the callout that starts with `**Today's Progress`. Replace it with the updated date and bar.
If all done: use `🎯 All done! 🟩🟩🟩🟩🟩  <total> / <total> (100%)` instead.
