---
name: trello-update
description: Quick updates to existing Trello cards — add a comment, change status, update description, or mark done. Use when the user wants to modify an existing card without running a full sync.
---

# Update Card

Make quick changes to an existing card on your **Trello** board.

## Board Config

```
Board: My Board (YOUR_BOARD_ID)

List IDs:
  Candidates for the week  YOUR_LIST_ID_CANDIDATES
  Doing                    YOUR_LIST_ID_DOING
  Waiting                  YOUR_LIST_ID_WAITING
  Done                     YOUR_LIST_ID_DONE
  To Do                    YOUR_LIST_ID_TODO
  Radar                    YOUR_LIST_ID_RADAR
  Goals                    YOUR_LIST_ID_GOALS

Local file mapping:
  Candidates  →  Tasks/candidates/candidates.md
  Doing       →  Tasks/in-progress/in-progress.md  (under ## Doing)
  Waiting     →  Tasks/in-progress/in-progress.md  (under ## Waiting)
  Done        →  Tasks/done/done.md
  Radar       →  Tasks/radar/radar.md
  Goals       →  Tasks/goals/goals.md
```

> **Setup:** Replace each `YOUR_*` placeholder with your actual Trello IDs. See README.md.

## Step 1: Parse args

The args describe the **action**, not the card. Card is always picked interactively.

Supported patterns:

```
/trello-update                              → ask action, then pick card
/trello-update comment: looks good          → pick card, then post comment "looks good"
/trello-update done                         → pick card, then move to Done
/trello-update move to Waiting              → pick card, then move to Waiting
/trello-update move to Doing               → pick card, then move to Doing
/trello-update move to Candidates           → pick card, then move to Candidates
/trello-update description: new text here   → pick card, then update description
```

Parse the args into:
- **action**: one of `comment`, `move`, `done`, `description`, or `unknown`
- **payload**: the text after the colon (for comment/description), or the target list name (for move)

If no args or unrecognized pattern → `action = unknown` (will ask in Step 3).

## Step 2: Pick the card

Read local files to build a card list:
1. Read `Tasks/in-progress/in-progress.md` — extract cards from both `## Doing` and `## Waiting` sections
2. Read `Tasks/candidates/candidates.md` — extract cards

Parse each line matching `- [ ] **Card title** <!-- trello:CARD_ID -->` into `{ title, cardId, list }`.

Present cards using `AskUserQuestion`:
- Show up to **4 cards** from Doing and Waiting (active work comes first) as options
- The user can always pick "Other" to see more cards or search
- If the user picks "Other":
  - Show Candidates cards as options (up to 4)
  - If they pick "Other" again, ask for a keyword and use `trello_find_card` with that keyword on the board to search

Each option label should be the card title. The description should show the list name (e.g., "Doing", "Waiting").

## Step 3: Determine action

If the action was already parsed from args in Step 1, skip this step.

Otherwise, use `AskUserQuestion`:

**Question:** "What do you want to do with **Card title**?"

**Options:**
- **Add a comment** — "Post a comment on the card"
- **Mark as done** — "Move to Done list"
- **Change status** — "Move to a different list"
- **Update description** — "Replace the card's description"

## Step 4: Execute the action

### Comment
- If no comment text was provided in args, ask the user: "What's the comment?"
- Call `trello_create_comment`:
  - `board`: `YOUR_BOARD_ID`
  - `card`: the card ID
  - `text`: the comment text
  - `output_hint`: `"confirmation that comment was posted"`

### Move / Change status
- If no target list was provided, use `AskUserQuestion`:
  - **Question:** "Move **Card title** to which list?"
  - **Options:** Doing, Waiting, Candidates, To Do (exclude the card's current list)
- Call `trello_move_card_to_list`:
  - `card`: the card ID
  - `to_board`: `YOUR_BOARD_ID`
  - `to_list`: the target list ID
  - `output_hint`: `"confirmation that card was moved"`

### Mark as done
- Call `trello_move_card_to_list`:
  - `card`: the card ID
  - `to_board`: `YOUR_BOARD_ID`
  - `to_list`: `YOUR_LIST_ID_DONE`
  - `output_hint`: `"confirmation that card was moved to Done"`

### Update description
- If no description text was provided in args, ask the user: "What should the new description be?"
- Call `trello_update_card`:
  - `card`: the card ID
  - `desc`: the new description text
  - `desc_overwrite`: `"true"`
  - `output_hint`: `"confirmation that description was updated"`

## Step 5: Update local files (if list changed)

Only needed when a card **moved between lists** (move or done actions). Skip for comment and description actions.

1. Read the source file where the card currently lives
2. Remove the card entry (the `- [ ] **Card title** <!-- trello:ID -->` line and all its indented continuation lines — labels, links, summary, checklists)
3. Write back the source file

4. If the target list has a local file (all except To Do):
   - Read the target file
   - Append the card entry at the appropriate location:
     - **In-progress**: at the end of the `## Doing` or `## Waiting` section (before the next `##` heading or end of file)
     - **Flat lists** (Candidates, Done, Radar): at the end of the file
   - The card entry format is: `- [ ] **Card title** <!-- trello:CARD_ID -->`
     - Keep any labels, links, and summary lines from the original entry
   - Write the target file

5. If the target list is To Do, only remove from the source file (To Do is not synced locally)

**Also update the weekly summary file** — list the files in `Tasks/tasks-this-week/` and use the file with the **highest week number** (i.e. the most recent `YYYY-WXX.md`). If no file exists, skip. Otherwise:
- Remove the card entry from its current section
- If the card moved to Done: add it under the `## Done` section
- If the card moved to another synced list: add it under the appropriate section with a link to the detail file
- Use the summary card format: `- [ ] [**Card title**](../relative/path) <!-- trello:CARD_ID -->`

## Step 6: Confirm

Print a single-line confirmation:

- **Comment**: `Commented on **Card title**: "comment text"`
- **Move**: `Moved **Card title** → Doing`
- **Done**: `Completed **Card title** → Done`
- **Description**: `Updated description for **Card title**`

If a local file was updated, add: `Local files updated.`
