---
skill: craft-roadmap-builder
---
# Roadmap Builder — Memory

Read at the start of every run. Update at the end.

## Learnings
- When the methodology states a rule that must apply to every item ("every item gets a confidence level", "cite evidence or mark as a bet"), check it's actually enforced consistently across *every* lane/table in the template and *every* branch (draft, update, audit) in the prompts — not just the common case. Three of three initial eval failures were this same shape: a rule stated in one file but missing from a specific table column or edge case in another.
- Deliberate no-evidence "strategic bet" items are a legitimate roadmap category, not a gap to hide — give them an explicit convention (e.g. a fixed placeholder string) rather than letting an agent choose between fabricating a citation or omitting the item.

## Run log
- 2026-08-13: Initial creation via craft-skill-builder, generalized from an internal OKR-planning/roadmap guidance doc. Took 4 eval passes to reach all-PASS — fixed 3 real cross-file consistency gaps (Later table missing Confidence column; no fallback text for an empty "Explicitly not doing" section; no convention for zero-evidence deliberate bets, including how Update mode should carry them forward). No real interactive run yet — evals were static/design reviews only.
