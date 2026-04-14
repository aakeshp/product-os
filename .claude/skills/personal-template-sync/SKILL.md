---
name: template-sync
description: Sync local AGENTS.md improvements to the public AGENTS.example.md, sanitising real IDs and org-specific strings. Run after filling in real IDs or improving AGENTS.md.
---

# Template Sync

Keeps the public `AGENTS.example.md` in sync with your local `AGENTS.md` by sanitising real IDs and org-specific strings before committing.

## Step 1: Read template config

Read `AGENTS.md` and locate the `## Template config` section.

Parse the table into a sanitisation map: `real_value → placeholder`. Skip any rows where the real value still contains `(your ...` — those are unfilled placeholders, not real values.

If the `## Template config` section is missing or all rows are unfilled, report:
```
Nothing to sync — fill in your real values in the "## Template config" table in AGENTS.md first.
```
and stop.

## Step 2: Produce sanitised output

Take the full text of `AGENTS.md` and apply two transformations:

1. **Remove the `## Template config` section entirely** — find the line `## Template config` and remove everything from that heading to the end of the file (or the next `##` heading if one follows it). This section is local-only and must not appear in `AGENTS.example.md`.

2. **Replace all real values with placeholders** — for each entry in the sanitisation map, replace every occurrence of the real value (exact string match) with its placeholder.

The result is the sanitised output.

## Step 3: Diff against AGENTS.example.md

Read `AGENTS.example.md` and compare it to the sanitised output.

If identical:
```
AGENTS.example.md is already up to date. Nothing to commit.
```
Stop here.

## Step 4: Check skill files for sensitive content

For each real value in the sanitisation map, search all tracked skill files:

```bash
grep -r "REAL_VALUE" .claude/skills/
```

If any matches are found, report them:
```
⚠️  Sensitive content found in skill files — fix before syncing:
  .claude/skills/personal-trello-sync/SKILL.md: line 14 contains "64e63af3..."
```
Stop here. Do not commit until the user has manually removed the sensitive content.

If no matches found, proceed.

## Step 5: Update AGENTS.example.md and commit

Write the sanitised output to `AGENTS.example.md`.

```bash
git add AGENTS.example.md
git commit -m "sync: update AGENTS.example.md"
git push origin main
```

Report:
```
AGENTS.example.md updated and pushed.
Changes: [brief summary of what changed — e.g. "added ## Self-improvement section, 2 new list IDs sanitised"]
```
