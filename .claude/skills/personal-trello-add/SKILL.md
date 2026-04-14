---
name: trello-add
description: Create a new Trello card on your board and update the local file. Use when the user wants to quickly add a task.
---

# Add Task

Create a new card on your **Trello** board and append it to the corresponding local markdown file.

## Board Config

```
Board: My Board (YOUR_BOARD_ID)

List → local file mapping:
  Candidates for the week (YOUR_LIST_ID_CANDIDATES)  →  Tasks/candidates/candidates.md
  Doing              (YOUR_LIST_ID_DOING)             →  Tasks/in-progress/in-progress.md  (under ## Doing)
  Waiting            (YOUR_LIST_ID_WAITING)           →  Tasks/in-progress/in-progress.md  (under ## Waiting)
  Key areas of attention (YOUR_LIST_ID_RADAR)         →  Tasks/radar/radar.md
  To Do              (YOUR_LIST_ID_TODO)              →  (not synced locally)
```

> **Setup:** Replace each `YOUR_*` placeholder with your actual Trello list IDs. See README.md.

## Step 1: Parse args

Extract from the user's input:

- **Title** (required): the main task name
- **Description** (optional): explanatory text, usually after a `|` separator
- **Labels** (optional): label names mentioned with `label:` or by name (e.g., `Product Management`)
- **Checklist items** (optional): items mentioned with `checklist:` followed by comma-separated items

Examples:
```
/trello-add Review API docs
/trello-add Review API docs | need to check rate limits and auth flow
/trello-add Review API docs | label: Product Management
/trello-add Review API docs | checklist: read docs, write summary, share with team
/trello-add Review API docs | Product Management | checklist: read docs, write summary
```

If no args are provided, ask the user to provide the full card details in one message:

> What task would you like to add? You can include optional details using this format:
>
> `Task title`
> `Task title | description of what to do`
> `Task title | description | label: Product Management`
> `Task title | description | label: Product Management | checklist: item 1, item 2`
>
> (List will be asked next.)

This should be a plain-text prompt (not AskUserQuestion). Parse the user's reply using the same rules above.

## Step 2: Ask which list

Use `AskUserQuestion` to prompt the user:

**Question:** "Which list should this card go on?"

**Options:**
- Candidates for the week
- Doing
- Waiting
- Radar
- To Do (not synced locally)

## Step 3: Create card on Trello

Call Zapier MCP `trello_create_card` with:
- `board`: `YOUR_BOARD_ID`
- `list`: the list ID from Step 2
- `name`: the title from Step 1
- `desc`: the description (if provided)
- `custom_labels`: array of label names (if provided)
- `checklist_name`: `"Checklist"` (only if checklist items were provided)
- `checklist_items`: array of items (only if provided)
- `output_hint`: `"card id and name"`

## Step 4: Update local file

If the chosen list has a local file (all except To Do):

1. Read the existing local file
2. Append a new card entry at the end of the appropriate section
3. For **In-progress** (Doing/Waiting): insert at the end of the `## Doing` or `## Waiting` section (before the next `##` heading or end of file)
4. For **flat lists** (Candidates, Radar): append at the end of the file

**Card format** (same as `/trello-sync` detail files):
```markdown

- [ ] **Card title** <!-- trello:CARD_ID -->
  `Label Name`
  > Description text
  - [ ] Checklist item 1
  - [ ] Checklist item 2
```

- Include a blank line before the card entry
- Include labels on the metadata line if provided. Omit if none.
- Include description as a blockquote summary if provided. Omit if empty.
- Include checklist items if provided. Omit if none.

If the list is **To Do**, skip the local file update and tell the user the card was created on Trello only (To Do is not synced locally).

## Step 5: Update weekly summary

If the list has a local file (i.e. not To Do):

1. List the files in `Tasks/tasks-this-week/` and use the file with the **highest week number** (i.e. the most recent `YYYY-WXX.md`). If no file exists, skip this step silently.
2. Append the card to the end of that file using this format:

```markdown
- [ ] [**Card title**](../relative/path/to/detail-file.md) <!-- trello:CARD_ID -->
  > Short one-line summary (if description provided, otherwise omit this line)
```

The relative path should be from `Tasks/tasks-this-week/` to the detail file (e.g. `../candidates/candidates.md`).

## Step 6: Confirm

Print a short confirmation:

```
Created: **Card title** → Candidates for the week
Local: Tasks/candidates/candidates.md updated
Weekly: Tasks/tasks-this-week/YYYY-WXX.md updated
```

If To Do: `Created: **Card title** → To Do (not synced locally)`
If weekly file didn't exist: omit the Weekly line.

Then, if the weekly file exists and was updated, output a separator and the full contents of the weekly summary file as a markdown preview:

```
---

<full contents of Tasks/tasks-this-week/YYYY-WXX.md>
```

Omit the preview if:
- The list was To Do (not synced locally)
- The weekly file did not exist (was skipped in Step 5)
