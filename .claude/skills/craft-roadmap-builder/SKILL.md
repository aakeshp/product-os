---
name: craft-roadmap-builder
description: Draft, update, or audit a Now/Next/Later product roadmap from Knowledge Core evidence, following the Decision Layer roadmap methodology. Use when the user wants to build a new roadmap, refresh an existing one for a new period, or check whether a roadmap's evidence still holds up.
---

## Before starting
Read `memory.md` in this skill folder and apply any noted patterns, edge cases, or fixes to this run.

# Roadmap Builder

Draft, update, or audit a Now/Next/Later roadmap, grounded in Knowledge Core evidence rather than opinion. This skill runs the Decision Layer roadmap methodology end to end — it does not duplicate that methodology inline, it reads and follows it.

This skill assumes it's running inside (or alongside) a Knowledge Core repo — the private repo where a team's Direction, Users, Signals, and Beliefs evidence actually lives. Product OS itself holds only the methodology, template, and prompts.

## Step 1: Locate the Product OS methodology

The methodology, template, and prompts this skill depends on live in the Product OS repo, not the Knowledge Core repo. Find them in this order:

1. If the current repo already contains `methodology/decision-layer/roadmaps.md` (i.e. this skill is running inside Product OS itself), use the current repo root as the Product OS path.
2. Otherwise, read the current repo's `AGENTS.md` for a `Product OS path` entry (same pattern as the `Tasks directory` entry used by `craft-task-write`).
3. If neither is found, ask the user for the path to their local Product OS clone via `AskUserQuestion`.

Stop and tell the user to set `Product OS path` in `AGENTS.md` if it can't be resolved — do not guess a path.

Once resolved, read:
- `{product-os}/methodology/decision-layer/roadmaps.md`
- `{product-os}/templates/decisions/roadmap-template.md`
- `{product-os}/prompts/decision/draft-roadmap.md`
- `{product-os}/prompts/decision/audit-roadmap-evidence.md`

## Step 2: Determine mode

Parse from args if given (a file path argument implies Update or Audit). Otherwise use `AskUserQuestion`:

- **Draft a new roadmap** — no existing file
- **Update an existing roadmap** — refresh for a new period or reflect new evidence
- **Audit an existing roadmap** — check evidence links and currency without changing anything

If Update or Audit and no file path was given, ask for it.

## Step 3: Gather Knowledge Core evidence

There's no fixed config for where Direction/Users/Signals/Beliefs evidence lives — it varies per Knowledge Core. Ask the user (via `AskUserQuestion` or free text) for the relevant file(s) or directory to read for this roadmap, e.g. "which Direction, Users, Signals, and Beliefs files should inform this roadmap period?" Read whatever is pointed to.

If the Knowledge Core lives in Notion rather than local files, use the Notion MCP (`notion-fetch`, `notion-search`) to retrieve the equivalent pages. If Notion access fails, stop and tell the user what to reconnect before continuing.

## Step 4: Run the appropriate prompt

- **Draft or Update** — follow `prompts/decision/draft-roadmap.md` exactly, using the gathered evidence and (for Update) the existing roadmap file as inputs. Compose the result using `templates/decisions/roadmap-template.md`.
- **Audit** — follow `prompts/decision/audit-roadmap-evidence.md` exactly against the existing roadmap file and current Knowledge Core evidence. No file is written in this mode — skip to Step 7 with the findings list instead of proposed content.

## Step 5: Determine output location (Draft/Update only)

Read the Knowledge Core repo's `AGENTS.md` for a `Roadmaps directory` entry. If not set, ask the user where roadmaps live (suggest `Decisions/Roadmaps/` as a default) and tell them they can add a `Roadmaps directory` entry to `AGENTS.md` to skip this next time.

Filename: `[roadmaps_dir]/[period-slug]-roadmap.md` (e.g. `2026-q3-roadmap.md`) for a new draft, or the existing file path for an update.

## Step 6: Diff before writing (Update only)

If updating, diff the proposed content against the existing file. Carry forward the existing Review log untouched and only append new entries — never rewrite history.

## Step 7: Review

**Draft/Update** — print the full proposed roadmap content, then use `AskUserQuestion`:
- **Looks good — save** → proceed to Step 8
- **I want to make changes** → ask what to change, apply edits, show again

**Audit** — print the findings list (Broken / Weakened / Unlinked / Stale, with recommended actions per `prompts/decision/audit-roadmap-evidence.md`). Nothing to save — skip to Step 9.

Only proceed to writing once the user approves.

## Step 8: Write file (Draft/Update only)

Before writing, update the frontmatter: `last_updated` to today, `status` (draft until the user says otherwise), and `related` to the Knowledge Core evidence files actually used this run. Then write the approved content to the path determined in Step 5. Do not write if the content is unchanged from the existing file (Update mode).

## Step 9: Report

**Draft/Update:**
```
Written: [path]
Roadmap: [team/product] — [period]
Now: N items · Next: N items · Later: N items
Weakest evidence: [item] — [why]
```

**Audit:**
```
Audited: [path]
Broken: N · Weakened: N · Unlinked: N · Stale: N
Top priority: [finding] — [recommended action]
```

## After completing
1. Spawn a separate agent using the Agent tool to run evals. Pass it the contents of `evals.md` and a brief summary of what was produced this run. The agent should return a pass/fail result for every item.
2. If any evals fail, continue working until they pass, then re-spawn the eval agent.
3. Once all evals pass, update `memory.md` — a brief run log entry if nothing new was learned, or add learnings under the relevant section if something new came up.
