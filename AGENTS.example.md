# AGENTS.md

> **Setup:** Copy this file to `AGENTS.md` and replace all `YOUR_*` placeholders with your own
> values. `AGENTS.md` is gitignored — your config stays private.

Instructions for AI agents working in this repo.

## What this project is

An open-source product management operating system. It provides methodology, prompts, and templates for AI-assisted product management workflows, along with craft skills for Claude Code.

## Project structure

```
methodology/
  knowledge-core-methodology/  — How to build and maintain a structured knowledge base
  discovery-engine/            — Research and discovery frameworks
  decision-layer/              — Decision-making frameworks and principles
  operational-layer/           — Day-to-day PM operational practices
prompts/
  discovery/                   — Prompts for research and discovery workflows
  synthesis/                   — Prompts for synthesising insights and research
  decision/                    — Prompts for decision-making and prioritisation
  product-operations/          — Prompts for product operations workflows
templates/
  knowledge-core/              — Templates for knowledge base artefacts
  discovery/                   — Templates for discovery artefacts
  decisions/                   — Templates for decision artefacts
  product-operations/          — Templates for operational artefacts (e.g. task-template.md)
.claude/
  skills/                      — Craft skill definitions (one directory per skill, each with SKILL.md)
  settings.local.json          — Project settings (gitignored — not committed)
.mcp.json                      — MCP server config
```

## Skills

Skills live under `.claude/skills/` and are slash commands invoked in Claude Code.

| Skill | Command | Description |
|---|---|---|
| craft-market-research-digest | `/craft-market-research-digest` | Generate weekly research digest from RSS + web search |
| craft-notion-sync | `/craft-notion-sync` | Sync from a Notion board into a local markdown file |
| craft-task-write | `/craft-task-write` | Generate an AI-ready task file with full context for Claude Code |

## task-write

AI-ready task files give Claude Code enough context to independently fix a bug or ship an improvement. The `/craft-task-write` skill generates these files and writes them to the configured directory.

| Item | Value |
|---|---|
| Tasks directory | `YOUR_TASKS_DIRECTORY` |
| Template | `templates/product-operations/task-template.md` |

## Git

### Operational changes (skill runs, digest, task-write)
These only modify local files. After each run:
1. Stage only the files that were changed.
2. Commit with a concise message.
3. Do NOT push — local commits only.

### Structural changes (skill improvements, new skills, config updates)
These modify tracked files visible on GitHub. Use a branch and PR:
1. Create a branch: `git checkout -b <type>/<short-description>`
   - `skill/` — improving an existing skill
   - `feat/` — new skill or feature
   - `fix/` — correcting broken behaviour
   - `chore/` — config or structural updates
2. Commit changes with a concise message.
3. Push the branch and open a PR: `gh pr create --fill`
4. Review the PR safety checklist before merging.
5. Merge and delete the branch.

## MCP

MCP server config is in `.mcp.json`. If tools are unavailable, re-authenticate via `/mcp` in Claude Code.

## Self-improvement

After completing any skill run, evaluate whether the skill instructions were complete and accurate:

- If you had to improvise, work around unclear instructions, or the result was suboptimal — propose a minimal, targeted update to the relevant `SKILL.md` file.
- Show the proposed change as a diff (before/after the specific lines).
- Ask for approval with `AskUserQuestion` (options: "Apply it" / "Skip").
- If approved:
  1. Create a branch: `git checkout -b skill/[skill-name]-[brief-description]`
  2. Update the skill file.
  3. Commit: `improve: [skill-name] — [one-line description]`
  4. Open a PR: `gh pr create --fill`
- Keep changes minimal — fix only what caused the issue, don't refactor.
