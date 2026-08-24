---
name: craft-roadmap-builder
description: Draft, update, or audit a Now/Next/Later product roadmap from Knowledge Core evidence, following the Decision Layer roadmap methodology. Use when the user wants to build a new roadmap, refresh an existing one for a new period, or check whether a roadmap's evidence still holds up.
---

## Before starting
Read `memory.md` in this skill folder and apply any noted patterns, edge cases, or fixes to this run.

# Roadmap Builder

Draft, update, or audit a whole Now/Next/Later roadmap, grounded in Knowledge Core evidence rather than opinion. This skill runs the Decision Layer roadmap methodology end to end — it does not duplicate that methodology inline, it reads and follows it.

This skill assumes it's running inside (or alongside) a Knowledge Core repo — the private repo where a team's Direction, Users, Signals, and Beliefs evidence actually lives. Product OS itself holds only the methodology, template, and prompts.

If the user just wants feedback on, or help writing, a single item — no evidence-gathering, no whole-roadmap view — that's `craft-roadmap-item` instead. Redirect there rather than doing a lightweight version of this skill's job.

## Step 1: Determine mode

Parse from args if given (a file path argument implies Update or Audit). Otherwise use `AskUserQuestion`:

- **Draft a new roadmap** — no existing file
- **Update an existing roadmap** — refresh for a new period or reflect new evidence
- **Audit an existing roadmap** — check evidence links and currency without changing anything

If Update or Audit and no file path was given, ask for it.

## Step 2: Locate the Product OS methodology

The methodology, template, and prompts this skill depends on live in the Product OS repo, not the Knowledge Core repo. Find the path in this order:

1. If the current repo already contains `methodology/decision-layer/roadmaps.md` (i.e. this skill is running inside Product OS itself), use the current repo root as the Product OS path.
2. Otherwise, read the current repo's `AGENTS.md` for a `Product OS path` entry (same pattern as the `Tasks directory` entry used by `craft-task-write`).
3. If neither is found, ask the user for the path to their local Product OS clone via `AskUserQuestion`.

Stop and tell the user to set `Product OS path` in `AGENTS.md` if it can't be resolved — do not guess a path.

## Step 3: Read only what this mode needs

Always read `{product-os}/methodology/decision-layer/roadmaps.md`. Then, based on the mode from Step 1:

- **Draft or Update** — also read `{product-os}/templates/decisions/roadmap-template.md` and `{product-os}/prompts/decision/draft-roadmap.md`.
- **Audit** — also read `{product-os}/prompts/decision/audit-roadmap-evidence.md`. The template isn't needed — audit checks the existing file's content, it doesn't compose a new one.

Don't read the prompt file for the mode you're not running — it isn't needed and just adds noise to context.

## Step 4: Gather Knowledge Core evidence and capacity (Draft/Update only)

There's no fixed config for where Direction/Users/Signals/Beliefs evidence lives — it varies per Knowledge Core. Ask the user (via `AskUserQuestion` or free text) for the relevant file(s) or directory to read for this roadmap, e.g. "which Direction, Users, Signals, and Beliefs files should inform this roadmap period?" Read whatever is pointed to.

Also ask for **available capacity** — how much the team can realistically take on this period, and any known constraints. This isn't Knowledge Core evidence (it's not one of the four layers) — it's operational context only the team can supply, and Now-lane placement in `draft-roadmap.md` depends on it as much as evidence does. Don't skip this and don't infer it from the evidence alone.

If the Knowledge Core lives in Notion rather than local files, use the Notion MCP (`notion-fetch`, `notion-search`) to retrieve the equivalent pages. If Notion access fails, stop and tell the user what to reconnect before continuing.

Audit mode doesn't need this step — it checks an existing roadmap's evidence links against current Knowledge Core state, not against fresh capacity input.

## Step 5: Run the prompt read in Step 3

- **Draft or Update** — follow `prompts/decision/draft-roadmap.md` exactly, using the gathered evidence and (for Update) the existing roadmap file as inputs. Compose the result using `templates/decisions/roadmap-template.md`.
- **Audit** — follow `prompts/decision/audit-roadmap-evidence.md` exactly against the existing roadmap file and current Knowledge Core evidence. No file is written in this mode — skip to the Review step with the findings list instead of proposed content.

## Step 6: Determine output location (Draft/Update only)

Check for a **roadmap registry** first — the Knowledge Core repo's `AGENTS.md` may point to a config file (look for an entry like `Squad roadmap registry`) mapping a team name to an output target and a conventions doc, instead of a local file. If a registry resolves the team to a target:

- Use that target directly — don't ask where to save.
- Read whatever conventions doc the registry points to (page/file structure, any team-specific rules) before composing content. Don't assume the local-file template below applies as-is; the conventions doc is authoritative for that target.
- If the target is a Notion page, proceed to the Notion-aware version of Step 9.

If no registry entry resolves, fall back to the local-file flow: read the Knowledge Core repo's `AGENTS.md` for a `Roadmaps directory` entry. If not set, ask the user where roadmaps live (suggest `Decisions/Roadmaps/` as a default) and tell them they can add a `Roadmaps directory` entry to `AGENTS.md` to skip this next time.

Filename: `[roadmaps_dir]/[period-slug]-roadmap.md` (e.g. `2026-q3-roadmap.md`) for a new draft, or the existing file path for an update.

## Step 7: Diff before writing (Update only)

If updating, diff the proposed content against the existing file. Carry forward the existing Review log untouched and only append new entries — never rewrite history.

## Step 8: Review

**Draft/Update** — print the full proposed roadmap content, then use `AskUserQuestion`:
- **Looks good — save** → proceed to Step 9
- **I want to make changes** → ask what to change, apply edits, show again

**Audit** — print the findings list (Broken / Weakened / Unlinked / Stale, with recommended actions per `prompts/decision/audit-roadmap-evidence.md`). Nothing to save — skip to Step 10.

Only proceed to writing once the user approves.

## Step 9: Write (Draft/Update only)

**Local file target:** Before writing, update the frontmatter: `last_updated` to today, `status` (draft until the user says otherwise), and `related` to the Knowledge Core evidence files actually used this run. Then write the approved content to the path determined in Step 6. Do not write if the content is unchanged from the existing file (Update mode).

**Notion page target:** Re-fetch the page's current content immediately before writing — never reconstruct it from earlier in the conversation. Someone may have hand-edited it since you last looked, and a blind full-page rewrite silently reverts that. Prefer a targeted, section-scoped update over a full-page rewrite wherever the change is localized, so an edit to one section structurally can't clobber another. If a fresh fetch shows the page's style has diverged from what the conventions doc describes (shorter phrasing, a dropped clause), match what's actually there rather than reintroducing the doc's example verbatim — treat it as a signal about preference, not a one-off to fix. Skip the write if nothing has actually changed.

## Step 10: Report

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
