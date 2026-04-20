---
name: daily-idea-extractor
description: Use when the user asks to extract ideas from daily notes, create a wiki entry, or says "save that idea", "that's worth keeping", or "add to wiki" after a daily note is written.
---

# Daily Idea Extractor

Extract idea-worthy content from daily notes and create permanent wiki entries in the Obsidian vault.

## What Counts as "Idea-Worthy"

Only extract if it passes at least one of these:

| Type | Signal | Example |
|------|--------|---------|
| **Compelling story** | Personal experience with a clear arc or tension | "I got rejected from every uni and still showed up to cafe to study" |
| **Original insight** | Something Saagii observed that isn't common knowledge | "The fraud feeling peaks when ambition and reality drift apart" |
| **Mental model** | A reusable lens for thinking about life/work/people | "Ego without tolerance for hard seasons collapses into fraud" |
| **Decision principle** | A rule derived from lived experience | "Time with someone who genuinely fills you up is not escapism" |
| **Tension worth holding** | A paradox or contradiction that reveals something true | "Wanting to give her the world, but the current self isn't there yet" |

**Do NOT extract:**
- Tasks, schedules, logistical notes
- Vague moods without insight ("felt bad today")
- Observations that are just complaints

## Step 1: Read the Daily Note

Read the most recent daily note at:
`00-RAW/Daily/YYYY-MM-DD.md`

Scan **Notes / Reflections** and **Didn't Do** sections first — ideas live there, not in task lists.

## Step 2: Extract and Classify

For each idea-worthy passage, identify:
- **Title**: Short phrase, evocative (not generic)
- **Category**: Which wiki folder it belongs in

| Life area | Wiki path |
|-----------|-----------|
| Identity / self-concept / ego | `10-WIKI/Identity/` |
| Relationships / Tanan / family | `10-WIKI/Personal/` |
| Business / money / agency | `10-WIKI/Business/` |
| Content / creativity / scripts | `10-WIKI/Creative/` |
| Learning / university / exams | `10-WIKI/Education/` |
| Mental health / habits / energy | `10-WIKI/Life/` |
| Instagram / videos / audience | `10-WIKI/Content/` |

## Step 3: Write the Wiki Entry

Save to: `10-WIKI/{Category}/{kebab-case-title}.md`

```markdown
# {Title}

> {One sentence: what this idea is and why it matters}

## The Idea
{2–4 sentences expanding the insight. **Apply humanizer principles:** write in Saagii's voice — short punchy sentences mixed with longer ones, first-person where it fits, specific feelings over vague claims, no AI vocabulary (no "pivotal", "testament", "landscape", "underscores"), no em dash overuse, no rule-of-three, no boldface headers. Sound like a person who had this realization, not a chatbot summarizing it.}

## Where It Came From
{One sentence referencing the source. Link to daily note.}
> Source: [[00-RAW/Daily/YYYY-MM-DD]]

## Why It Matters
{Write 2–3 short perspective takes, each from a distinct character lens. Choose the 2–3 most relevant from:}

- **The Coach** *(identity shift lens)*: {How does this idea change who Saagii is becoming, not just what he does?}
- **The Creator** *(content/storytelling lens)*: {What story does this unlock? How does it connect to the audience?}
- **The Strategist** *(business/execution lens)*: {How does this sharpen focus or remove a blocker toward the first $1K goal?}
- **Future Saagii** *(10-years-out lens)*: {What would the version of Saagii who already made it say about this moment?}

{Pick the 2 or 3 lenses where the insight is genuinely interesting. Skip any where the angle is forced. Keep each perspective to 1–2 sentences.}

## Related
{Optional: [[link to related wiki entries]]}
```

Add a **Next Step** section at the end — one concrete, actionable thing to do with this idea:

```markdown
## Next Step
{Write this as a **personal coach specializing in identity shift and mindset improvement**. The action should:
- Be one specific, concrete thing Saagii can do today or this week (not "think about it")
- Frame it in terms of *who he's becoming*, not just what needs to get done
- Sound like a direct coaching prompt — second person ("Do this", "Say this", "Write this") with the stakes named
- Connect the insight to his Q2 identity arc: creator, coach, first-$1K earner

Bad: "Write a script hook using this tension"
Good: "The version of you who already has 10K followers leads with this tension in EP-01. Write the first 3 sentences of that hook tonight — not to post it, but to prove to yourself you can say it out loud."}
```

**Keep entries under 25 lines. Dense > long.**

## Step 4: Tell the User

After writing, output:
```
💡 Wiki entry created: 10-WIKI/{Category}/{title}.md
   "{one-line summary of the idea}"
```

If multiple ideas extracted, list all of them.

## Common Mistakes

- **Extracting tasks as ideas** — "do Duolingo prep" is not an idea
- **Summarizing the day** — the wiki is not a diary, it's a knowledge base
- **Over-writing** — if it takes more than 10 lines to explain, the idea isn't clear yet
- **Wrong category** — when in doubt, use `10-WIKI/Life/`
