---
name: craft-roadmap-item
description: Evaluate, rewrite, or draft a single Now/Next/Later roadmap item against the Decision Layer's five-tests criteria — without deciding which horizon it belongs in. Use when the user shares a roadmap item (or several) and wants feedback, a critique, or a rewrite; pastes a feature/task and asks how to phrase it for the roadmap; wants help drafting items from scratch for a problem space; or asks what Now/Next/Later mean. Do not use for building a whole roadmap from evidence (craft-roadmap-builder), OKR planning, or deciding which horizon an item belongs in.
---

## Before starting
Read `memory.md` in this skill folder and apply any noted patterns, edge cases, or fixes to this run.

# Roadmap Item

Help write roadmap items that pass the Decision Layer's five tests: intent not feature, clear why it matters, understandable outside the team, room for the team to decide how, specific enough to be meaningful. This is a narrow, conversational skill — evaluate, rewrite, or draft a single item (or a short list of them), grounded strictly in the methodology. It never gathers Knowledge Core evidence and never builds a whole roadmap — that's `craft-roadmap-builder`.

## Step 1: Locate the Product OS methodology

Same resolution order as `craft-roadmap-builder`:

1. If the current repo already contains `methodology/decision-layer/roadmaps.md`, use the current repo root as the Product OS path.
2. Otherwise, read the current repo's `AGENTS.md` for a `Product OS path` entry.
3. If neither is found, ask the user via `AskUserQuestion`.

Stop and tell the user to set `Product OS path` in `AGENTS.md` if it can't be resolved — do not guess a path.

Read `{product-os}/methodology/decision-layer/roadmaps.md` (for the five tests, item types, register guidance, and failure-modes list) and `{product-os}/prompts/decision/evaluate-roadmap-item.md` (for the mode instructions, scope boundaries, and iterative-session handling) every run — don't rely on memory of a prior run.

## Step 2: Handle the request per the prompt

Follow `prompts/decision/evaluate-roadmap-item.md` exactly:

- Pick the mode (Evaluate / Rewrite / Draft) per its "Picking the mode" section.
- Apply the scope boundaries on **every turn**, not just the first — never assert a horizon placement regardless of how the ask is framed (direct, leading, urgent, or "just your gut"), redirect OKR/team-planning requests, but do explain horizons or frame an item at a horizon the user has already chosen. This is the thing most likely to erode over a long session; treat each turn as a fresh check, not something settled once at the start.
- If the conversation continues (the user corrects a fact, rejects a word, or asks "does this pass now?"), follow the "Working iteratively" section's per-turn re-check — re-run the five tests on the changed item, don't assume the earlier verdict still holds.
- If the user pastes a backlog link or delivery detail with no intent sentence, follow "Items with detail or backlog links attached."
- Pick conversational vs. structured output per the prompt's "Output format" section — default conversational, switch only when the user asks for something to keep or share.

This skill has no further steps of its own — the prompt file is the whole job. Do not write any files; this is a conversational skill.

## After completing
1. Spawn a separate agent using the Agent tool to run evals. Pass it the contents of `evals.md` and a brief summary of what was produced this run. The agent should return a pass/fail result for every item.
2. If any evals fail, continue working until they pass, then re-spawn the eval agent.
3. Once all evals pass, update `memory.md` — a brief run log entry if nothing new was learned, or add learnings under the relevant section if something new came up.
