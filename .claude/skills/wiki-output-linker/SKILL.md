---
name: wiki-output-linker
description: Use after creating a wiki entry to link it to existing files in 20-OUTPUTS. Never creates new output files — only connects to what already exists.
---

# Wiki Output Linker

After a wiki entry is saved, scan `20-OUTPUTS/` and add links to any existing output files that are genuinely related to the idea.

## What "genuinely related" means

Link when the output file:
- Covers the same theme, tension, or insight as the wiki entry
- Is a script, episode, or plan where this idea could directly sharpen or inform the content
- Already contains language or narrative that overlaps with the wiki idea

**Do NOT link when:**
- The connection is superficial (both are about "content" in general)
- You'd have to stretch to explain why they connect
- The output file doesn't exist yet

## The Vault Path

Obsidian vault: `/Users/saranochirsereglen/Library/Mobile Documents/iCloud~md~obsidian/Documents/Second Brain/`

Outputs live at: `20-OUTPUTS/` with subfolders:
- `20-OUTPUTS/Series/` — episode scripts and series narrative
- `20-OUTPUTS/Plans/` — project plans
- `20-OUTPUTS/Scripts/` — standalone scripts

## Steps

### Step 1: Scan existing outputs

List all `.md` files under `20-OUTPUTS/`. Read the title and first 10–15 lines of each to understand what it covers.

### Step 2: Match against the wiki idea

For each output file, ask: does this idea directly connect to what this output is doing?

If yes — note the relative Obsidian path: `[[20-OUTPUTS/Series/filename]]`

### Step 3: Update the wiki entry's Related section

Open the just-created wiki entry at `10-WIKI/{Category}/{slug}.md`.

If the `## Related` section is empty or has placeholder text, replace it with the matched links:

```markdown
## Related
[[20-OUTPUTS/Series/Making $1000 Online - Ep1 - AI + Obsidian]]
```

If no genuine matches found, leave `## Related` empty or as-is. Do not add placeholder text.

### Step 4: Report

Output one line per link added:
```
🔗 Linked to: 20-OUTPUTS/Series/{filename}
```

If no links were added: output nothing (skip this step entirely — silence is fine).

## Rules

- **Read before you link** — always read at least the first 15 lines of an output file before deciding it's related
- **One strong link beats three weak ones** — be selective
- **Never create files in 20-OUTPUTS** to have something to link to
- **Never link to files outside 20-OUTPUTS** from this skill (wiki-to-wiki links are handled separately)
