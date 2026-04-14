---
name: notion-sync
description: Two-way sync between your squad's Notion board and local Tasks/squad/squad.md. Use when the user wants to pull squad tasks from Notion or push local status changes (checked items) back.
---

# Notion Squad Sync

Two-way sync between your **squad's Notion database** and the local `Tasks/squad/squad.md` file.

## Database Config (hardcoded)

```
Database: Squad (see AGENTS.md for the database ID)
Local file: Tasks/squad/squad.md
```

## Step 1: Read Local State

Read `Tasks/squad/squad.md` if it exists. If the file does not exist, treat local state as empty.

Parse each task line into: `{ checked, title, pageId, status, description }`.
Extract `pageId` from `<!-- notion:PAGE_ID -->` comments.

Build two maps:
- **Checked items** (`[x]` with a notion:ID) — these need a status push to Notion
- **New items** (`[ ]` without a notion:ID) — these need creating in Notion

## Step 2: Fetch Notion State

Call `notion_query_database` with the squad database ID from AGENTS.md.

Fetch all pages where status ≠ Done (active tasks only). For each page, extract:
- `id` (page ID)
- `title` (Name/Title property)
- `status` (Status property value)
- `assignee` (Assigned / Person property — first member name if multiple)
- `due` (Due date property, formatted as YYYY-MM-DD)
- `description` (first paragraph of page body, if accessible)

If any items were checked in Step 1, also fetch those specific pages regardless of status (needed to confirm they exist before pushing).

## Step 3: Push Local Changes to Notion

Compare local state (Step 1) against Notion state (Step 2):

**Checked items `[x]`** — for each locally checked item with a page ID:
→ Call `notion_update_page` to set the Status property to `Done`

**New items** — for each `[ ]` item WITHOUT a `<!-- notion:PAGE_ID -->` comment:
→ Call `notion_create_page` in the squad database with the title from the local file
→ Record the new page ID returned

**Title changes** — for each item where the local title differs from the Notion page title:
→ Call `notion_update_page` to update the title

## Step 4: Write Local File (no re-fetch)

Do NOT re-fetch from Notion. Use the data already in memory (Step 2 state + Step 3 results).

**Notion is authoritative** — pages absent from the Notion response have been completed or deleted; drop them from the local file (they will naturally disappear since we only fetch non-Done tasks).

**Diff before writing**: compare new content against existing file. Only rewrite if something changed.

Write `Tasks/squad/squad.md` using this format:

### File header

```markdown
---
synced: YYYY-MM-DDTHH:MM:SS
database: Squad
---
# Squad Tasks
```

### Task format

```markdown
- [ ] **Task title** <!-- notion:PAGE_ID -->
  `Status` · Assigned: Name · Due: YYYY-MM-DD
  > 1–2 sentence description from the Notion page body.
```

**Field rules:**
- **Title**: always bold (`**...**`)
- **Metadata line**: Status in inline code span, then Assigned and Due if present. Omit Assigned if no assignee. Omit Due if no due date. If only a status and nothing else, still show the status. If no metadata at all, skip the line.
- **Description**: blockquote (`> ...`), 1–2 sentences max. Omit if the page body is empty or inaccessible.
- **notion:PAGE_ID**: store the Notion page UUID in an HTML comment on the same line as the title (never on its own line)
- Do NOT include checklist items — Notion task pages don't have checklists in this workflow

**Group tasks by Status** if there are multiple distinct statuses among active tasks (e.g. `## In Progress`, `## Review`). If all tasks share one status, use a flat list with no section headers.

## Step 5: Report

Print a terminal summary:

```
# Squad Sync — W<NN> <YYYY>

Fetched N tasks from Notion squad database.
Pushed M status updates to Done.
Created K new pages in Notion.

## Active tasks (N)
- Task title [Status]
- Task title [Status]

Updated Tasks/squad/squad.md
```

If no changes were pushed (M = 0 and K = 0), omit the "Pushed" and "Created" lines.
If the file was unchanged (diff showed no difference), replace the last line with: `Tasks/squad/squad.md unchanged`.
