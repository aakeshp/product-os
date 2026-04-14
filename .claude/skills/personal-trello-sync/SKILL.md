---
name: trello-sync
description: Two-way sync between your Trello board and local Tasks/ and Context/ folders. Use when the user wants to pull their current week's tasks from Trello or push local changes back.
---

# This Week Sync

Two-way sync between your **Trello** board and local markdown files.

## Board & List Config (hardcoded)

```
Board: My Board (YOUR_BOARD_ID)

List → Folder mapping:
  Goals              (YOUR_LIST_ID_GOALS)      →  Tasks/goals/goals.md
  Key areas of attention (YOUR_LIST_ID_RADAR)  →  Tasks/radar/radar.md
  Candidates for the week (YOUR_LIST_ID_CANDIDATES) →  Tasks/candidates/candidates.md
  Doing              (YOUR_LIST_ID_DOING)      →  Tasks/in-progress/in-progress.md  (tagged: doing)
  Waiting            (YOUR_LIST_ID_WAITING)    →  Tasks/in-progress/in-progress.md  (tagged: waiting)
  Done               (YOUR_LIST_ID_DONE)       →  Tasks/done/done.md
  To Do              (YOUR_LIST_ID_TODO)       →  (skip — not synced)
```

> **Setup:** Replace each `YOUR_*` placeholder with your actual Trello IDs. Find your board ID in
> the Trello board URL (`trello.com/b/BOARD_ID/board-name`). Find list IDs via the Trello API:
> `GET https://api.trello.com/1/boards/BOARD_ID/lists?key=KEY&token=TOKEN`

## Step 1: Read Local State

For each synced list, read its local markdown file if it exists.
Parse each task line into: `{ checked, title, cardId, sourceList }`.
Extract `cardId` from `<!-- trello:ID -->` comments.
Build a map per file: `cardId → { checked, title, sourceList }`.

**Also read the weekly summary file** (`Tasks/tasks-this-week/YYYY-WXX.md` for the current ISO week) if it exists. Parse any checked items `[x]` from the summary. These checked items feed into Step 3 the same way as checks in individual list files — they trigger a move-to-Done.

## Step 2: Fetch Trello State (incremental)

Not all lists need fetching every run. Quick reference:

| List | Fetch when | Skip when |
|------|-----------|-----------|
| Candidates | Always | — |
| Doing | Always | — |
| Waiting | Always | — |
| Radar | Always | — |
| Goals | Always | — |
| Done | Always | — |

Typical run: 6 API calls.

For each list that IS fetched, use Zapier MCP `trello_find_card` to get all cards. Always pass `open_cards_only: "true"` to exclude archived/closed cards.
Use `output_hint` to request: "card id, name, full description text, labels, due date, any URLs found in the description, and checklists with items and completion status".
For **Radar** only, also include `"and most recent comments"` in the `output_hint`.
Each card returns: id, title, description (full text), labels, due date, checklists (+ comments for Radar).
Build a map per list: `cardId → { title, description, labels, dueDate, urls, checklists, comments }`.

If `trello_find_card` does not return checklist data for a card, fall back to calling `trello_find_checklist` with the card ID to retrieve checklists and their items.

For each list that is SKIPPED, reuse the parsed local state from Step 1 as-is.

## Step 3: Push Local Changes to Trello

Compare local state (Step 1) against Trello state (Step 2):

**Checked items `[x]`** — for each locally checked item with a card ID (skip Goals — they have no checkboxes):
  → Use `trello_move_card_to_list` to move the card to the Done list (`YOUR_LIST_ID_DONE`)

**New items** — for each `[ ]` item WITHOUT a `<!-- trello:CARD_ID -->` comment:
  → Use `trello_create_card` to create a new card on the appropriate Trello list
  → The list is determined by which file the item is in (e.g. items in `Tasks/candidates/candidates.md` go to "Candidates for the week")

**Title changes** — for each item where the local title differs from the Trello card title:
  → Use `trello_update_card` to update the title on Trello

**Deleted items** — for each card ID that was in the local file on last sync but is now missing:
  → Use `trello_archive_card` to archive the card on Trello

## Step 4: Write Local Files (no re-fetch)

Do NOT re-fetch from Trello. Build the final state from data already in memory:
- For all lists: **Trello is authoritative.** Start from the fetched Trello cards, updated with any results from Step 3 pushes (e.g. new card IDs from creates, cards moved to Done). Any card present in the local file but absent from the Trello response has been moved or archived on Trello — drop it from the local file.
- For **Done**: write all fetched cards as `[x]` checked items (same pattern as Candidates/Radar). All cards in the Trello Done list belong in `done.md` regardless of whether they were moved there by this run or directly on Trello.

**Diff before writing**: before overwriting a file, compare the new content against the existing local file. Only rewrite the file if cards were added, removed, moved, or renamed. If nothing changed, leave the file untouched — this preserves the original `synced` timestamp and avoids unnecessary writes.

Write each markdown file with this format:

### Card format (all files)

Every card across all files uses the same field format:

```markdown
- [ ] **Card title** <!-- trello:CARD_ID -->
  `label1` · `label2` · Due: YYYY-MM-DD
  [Readable link text](url1) · [Readable link text](url2)
  > 1-2 sentence summary of what this task involves and what action to take
  - [x] Completed checklist item
  - [ ] Incomplete checklist item
```

**Field rules:**
- **Title**: always bold (`**...**`)
- **Metadata line** (labels + due): inline `code spans` for each label, separated by ` · `, with due date appended after the last label. Omit labels if none. Omit due date if none. If a card has no labels AND no due date, skip this line entirely.
- **Links line**: proper markdown links `[text](url)`. Omit entirely if no URLs found in description.
- **Summary**: blockquote (`> ...`). Omit entirely if description is empty.
- **Comments** (Radar only): show the most recent 2-3 comments as blockquotes below the summary:
  `> 💬 **Name**: Comment text (YYYY-MM-DD)`
  Omit entirely if no comments. Only applies to Radar cards.
- **Checklists**: show checklist items as indented checkboxes below the summary/comments. Use `- [x]` for completed items and `- [ ]` for incomplete items. If a card has multiple checklists, prefix each group with `  **Checklist Name:**` on its own line. If only one checklist, show items directly without a heading. Omit entirely if the card has no checklists. Checklist items are display-only — no two-way sync of checklist state back to Trello.

**Link text extraction:** Instead of bare URLs, extract a readable name from the URL path:
- `https://notion.so/your-org/Content-requirements-1d826cc4e1b1...` → `[Content requirements](url)`
- `https://miro.com/app/board/some-board-name` → `[Miro board](url)`
- `https://docs.google.com/document/d/...` → `[Google Doc](url)`
- `https://docs.google.com/spreadsheets/d/...` → `[Google Sheet](url)`
- If the path segment is unclear or too short, use the domain as the link text: `[notion.so](url)`
- Clean up path slugs: replace hyphens with spaces, strip trailing IDs/hashes, title-case the first word only

**Summary guidance:** Do NOT truncate the description. Instead, read the full description and write a concise 1-2 sentence summary that captures what the task is about and what action to take next. Extract any URLs from the description and list them separately on the Links line. **Every card with a non-empty description MUST have a summary line.** Do not skip summaries for any list including Candidates and In-progress.

### File structures

**Candidates, Radar, Done** — flat list:

```markdown
---
synced: YYYY-MM-DDTHH:MM:SS
board: My Board
---
# <List Name>

- [ ] **Card title** <!-- trello:CARD_ID -->
  `Label` · Due: 2026-02-09
  [Readable link text](url)
  > Summary of what to do
```

**In-progress** — split into Doing/Waiting sections:

```markdown
---
synced: YYYY-MM-DDTHH:MM:SS
board: My Board
---
# In Progress

## Doing
- [ ] **Card title** <!-- trello:CARD_ID -->
  `Label` · Due: 2026-02-09
  [Readable link text](url)
  > Summary of what to do

## Waiting
- [ ] **Card title** <!-- trello:CARD_ID -->
  `Label` · Due: 2026-02-09
  [Readable link text](url)
  > Summary of what to do
```

**Goals** — context, not tasks (no checkboxes):

```markdown
---
synced: YYYY-MM-DDTHH:MM:SS
board: My Board
---
# Goals

- **Goal title** <!-- trello:CARD_ID -->
  `Label` · Due: 2026-02-09
  [Readable link text](url)
  > Summary of what to do
```

## Step 5: Report

### Part 1: Sync changes (conditional)

Only print if Step 3 pushed any changes to Trello. Summarize in one line:

```
Synced: 2 cards moved to done, 1 new card created, 1 archived
```

If no changes were pushed, skip this line entirely.

### Part 2: Terminal summary (always shown)

Print a concise summary to the terminal using the data from Step 4. Use today's date.

```markdown
# This Week — <YYYY-MM-DD>

## In Progress
### Doing (<count>)
- Card title
### Waiting (<count>)
- Card title

## Candidates (<count>)
- Card title
- Card title

## Done (<count>)
- Card completed this week
- Card completed this week

---

## Radar (<count>)
- Key area title
- Key area title
```

**Rules:**
- In Progress, Candidates, Done = main sections (the weekly work)
- Radar = below a `---` divider (context, not action items)
- Goals = **not displayed** in terminal (they appear in the summary file)
- Show card counts in parentheses next to each heading
- Card titles only — no due dates, labels, or descriptions in the terminal view
- Empty sections: still show the heading with (0), just no bullet items
- **Done section**: List all cards moved to Done **during the current ISO week** (i.e. cards in the weekly summary file's Done section after this sync, including those added by previous syncs this week). Show the count in parentheses. If none, show `## Done (0)` with no bullet items.

### Part 3: Write weekly summary file (overview with links to detail files)

Write a lightweight overview to `Tasks/tasks-this-week/YYYY-WXX.md` (ISO week number, e.g. `2026-W07.md`). This is a **dashboard** — card titles link to the detail files where full information (labels, due dates, links, descriptions, checklists) lives.

**Summary card format** (different from detail files):

```markdown
- [ ] [**Card title**](../relative/path/to/detail-file.md) <!-- trello:CARD_ID -->
  > Short one-line AI summary
```

- Title is a **relative markdown link** to the detail file where full card info lives
- Only include a short ~one-line summary (enough for at-a-glance context). Omit if description is empty.
- **No labels, due dates, links, or checklists** in the summary — those are in the detail files
- Checkboxes and Trello IDs stay (needed for sync/move-to-Done)
- Goals are **not included** in the summary file (context only, lives in `Tasks/goals/goals.md`)

**Link targets from `Tasks/tasks-this-week/`:**
| Section | Relative path |
|---------|--------------|
| Candidates | `../candidates/candidates.md` |
| Doing | `../in-progress/in-progress.md` |
| Waiting | `../in-progress/in-progress.md` |
| Done | `../done/done.md` |
| Radar | `../radar/radar.md` |

**Format:**

```markdown
---
week: "2026-W07"
synced: 2026-02-09T19:57:00
board: My Board
---
# This Week — 2026-02-09

## In Progress
### Doing (2)
- [ ] [**Draft Q1 OKRs**](../in-progress/in-progress.md) <!-- trello:ID -->
  > Write first draft of team OKRs for Q1, circulate for async feedback.

- [ ] [**Review roadmap with stakeholders**](../in-progress/in-progress.md) <!-- trello:ID -->

### Waiting (1)
- [ ] [**Stakeholder sign-off on roadmap**](../in-progress/in-progress.md) <!-- trello:ID -->
  > Waiting for VP approval before moving to implementation.

## Candidates (3)
- [ ] [**Set up weekly planning ritual**](../candidates/candidates.md) <!-- trello:ID -->
  > Decide on a recurring time block each week to run /trello-sync.

- [ ] [**Review API documentation**](../candidates/candidates.md) <!-- trello:ID -->

## Done (0)

---

## Radar (5)
- [ ] [**Improve onboarding conversion rate**](../radar/radar.md) <!-- trello:ID -->
  > Identify biggest drop-off points and prioritise fixes for Q1.
```

**Key rules:**
- In Progress, Candidates, Done = main sections above the `---` divider (In Progress first)
- Radar = below the `---` divider (context, not action items)
- Goals are **not shown** in the summary file — they are context only, stored in `Tasks/goals/goals.md`
- Card titles are links to detail files — full info (labels, due, links, checklists, comments) is only in the detail files
- **Done section**: Show cards completed **this week** — all cards moved to Done during any sync in the current ISO week. Accumulate across syncs by preserving existing Done entries from the current week's summary file when overwriting it. Use `## Done (<count>)` with no total. If none, show `## Done (0)` with no bullet items.
- Empty sections: still show the heading with (0), just no bullet items
- **Checking a box** `[x]` in this file and running `/trello-sync` moves the card to Done on Trello (parsed in Step 1)

**Behavior:**
- If the file for the current week already exists, overwrite it (each sync updates the same week's file)
- New week → new file (previous weeks' files are preserved as history)
- Create the `Tasks/tasks-this-week/` directory if it doesn't exist

### Squad section (append after Radar)

If `Tasks/squad/squad.md` exists, append a `## Squad` section at the bottom of the weekly summary file, below the Radar section. This is **read-only** — no syncing, no IDs, no checkboxes pushed to Notion.

```markdown
## Squad
- Task title [Status]
- Task title [Status]
```

- Read `Tasks/squad/squad.md` and extract task titles + status values
- Use plain `- ` bullet points (no checkboxes, no Notion IDs)
- Status in brackets: `[Status]` — omit if no status available
- If `Tasks/squad/squad.md` does not exist or is empty, skip the Squad section entirely
- Do NOT write to `Tasks/squad/squad.md` — this is display-only
