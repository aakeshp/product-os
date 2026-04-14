---
name: task-write
description: Generate an AI-ready task file with full context for Claude Code to act on a bug or improvement to any codebase.
---

You are generating an AI-ready task file that gives Claude Code enough context to independently fix a bug or ship an improvement. Follow these steps exactly.

---

## Step 1 — Gather inputs

**If a file path argument was provided** (e.g. `/craft-task-write [YourOrg]/[Team]/Tasks/2026-03-23-foo.md`):
- Read the existing task file
- Extract: repo (from frontmatter or Repository context), any component/symbol names from Technical context or Relevant files
- Skip all input-gathering rounds below
- Jump directly to Step 1.5 using the extracted repo and component name
- In Step 2, compose an **updated** version — preserve all existing content, only add or improve fields where code search found new information
- In Step 2.5, show the full updated content for review
- In Step 3, **overwrite** the existing file at the same path (do not create a new file)
- In Step 4, commit with: `chore: enrich task — [title]`

**If a Notion task URL or ID was provided** (as an argument or in the user's message):
- Use the Notion MCP (`notion-fetch`) to retrieve the task page
- Extract: title, description, linked resources, labels/tags, acceptance criteria
- Use these as pre-populated defaults — confirm with the user or let them override

**Otherwise**, use `AskUserQuestion` to gather inputs interactively across three rounds. Do not generate the file until all required fields are collected.

### Round 1 — source and type
Use a single `AskUserQuestion` call with 2 questions:
- **Notion URL?** — options: `Yes, I have one` / `No, describe manually`. If yes, stop and ask for the URL, then fetch it via `notion-fetch` and skip to Round 1b.
- **Type** — options: `bug` / `feature` / `improvement` / `refactor` (this becomes the subfolder)

### Round 1b — title, category, repo
Use a second `AskUserQuestion` call with up to 3 questions:
- **Title** — no fixed options; user provides via "Other" free text
- **Category** — a short area label (e.g. `accessibility`, `performance`, `auth`, `content`, `navigation`); user provides via "Other" free text
- **Repo** — no fixed options; user provides `owner/repo-name` via "Other" free text

If Notion was fetched, pre-populate Title from the page title and skip asking for it if already known; still ask for Category and Repo if missing.

### Round 2 — problem & criteria
After Round 1, use a second `AskUserQuestion` call:
- **Problem** — what's broken or what needs to exist (current behaviour vs desired behaviour); user provides via "Other" free text
- **Acceptance criteria** — one item per line; user provides via "Other" free text

### Round 3 — optional fields
Use a third `AskUserQuestion` call for optional context. All can be skipped (user selects "Other" and leaves blank, or selects a skip option):
- **Relevant files** — comma-separated paths, or skip
- **Technical context** — architecture notes, patterns, snippets, or skip
- **Guardrails** — constraints the AI must respect, or skip
- **How to test** — commands to run and what passing looks like, or skip

---

## Step 1.5 — GitHub code search

If a component or symbol name is known (from Notion or user input) and a repo is available:

1. Search for the component in the repo:
   ```bash
   gh api "search/code?q=[component-name]+repo:[owner/repo]" --jq '.items[:5] | .[] | {path: .path, url: .html_url}'
   ```
   - If `gh` is unavailable or returns an error, skip this step silently.
   - If no component name is known, skip this step.

2. If files are found, read a focused slice of the most relevant one:
   ```bash
   gh api "repos/[owner/repo]/contents/[path]" --jq '.content' | base64 -d | head -80
   ```
   - Extract only what's useful: existing structure, imports, patterns relevant to the fix.

3. Update inputs with findings:
   - Add discovered file path(s) to **Relevant files**
   - Add relevant code structure or patterns to **Technical context**
   - Do not duplicate information already from Notion

---

## Step 2 — Compose task file content

- Read `AGENTS.md` and find the `tasks_dir` value from the "task-write output" table.
  If it is not set, stop and tell the user: "Set `Tasks directory` in the `task-write output` table in `AGENTS.md` before running this skill."
- Filename: `[tasks_dir]/[type]/[category]-[slug].md`
  - `[type]` = the type bucket, used as a subdirectory (`bug`, `feature`, `improvement`, `refactor`)
  - `[category]` = the category label, lowercased, spaces to hyphens
  - `[slug]` = title lowercased, spaces to hyphens, special chars stripped
  - The subdirectory is created automatically if it doesn't exist
- Use the template structure from `Product Management/AI Task Writer/task-template.md`
- Fill in all provided fields
- Leave placeholder text (from the template) for anything not provided
- Set `status: ready` if all required fields are filled; otherwise `status: draft`
- **Do not write the file yet** — hold the content for review in Step 2.5

---

## Step 2.5 — Review

Print the full proposed file content to the user, then use `AskUserQuestion` with two options:
- **Looks good — save and commit** → proceed to Step 3
- **I want to make changes** → ask what to change (free text via Other), apply the edits, then loop back and show the updated content for another review

Only proceed once the user approves.

---

## Step 3 — Write file and report

Write the approved content to disk, then print:
```
Created: [tasks_dir]/[type]/[category]-[slug].md
Task: [title] ([type]/[category]) — [repo]
```

---

## Step 4 — Commit and push

```bash
git add "[tasks_dir]/[type]/[category]-[slug].md"
git commit -m "feat: add AI-ready task — [title]"
git push origin main
```
