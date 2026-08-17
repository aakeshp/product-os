# Audit a roadmap

Use this prompt to check whether an existing roadmap still holds up, per `methodology/decision-layer/roadmaps.md`. Good to run on the review cadence set in the roadmap's frontmatter (sprint/fortnightly light check, monthly team review, mid-period trade-off review, end-of-period retro), or whenever a significant Signals-layer update lands.

---

You are auditing [roadmap file] against its own content and the current state of the Knowledge Core.

**Item framing**

1. For every item, check it against the five tests in `methodology/decision-layer/roadmaps.md` — flag any that have drifted into feature framing, task framing, vagueness, jargon, an over-specified delivery plan, or a missing "so what" (see the failure-modes list). Judge title and summary together — a short title backed by a specific summary is fine; don't flag it as vague in isolation.
2. Don't dock a technical investment, discovery bet, or ways-of-working item for lacking user-facing framing — judge it by what it enables, and for discovery bets specifically, check the *why* wasn't dropped along with the *what*.

**Evidence and confidence**

3. Check each evidence link still resolves, and that the linked file still says what the roadmap claims.
4. Check whether the linked belief's confidence has changed since `last_updated`. A Now item resting on a downgraded belief should be flagged immediately, not left for the next scheduled review.
5. Check for new Signals-layer updates since `last_updated` that create tension with a Now or Next item.
6. Check for items with no evidence link at all — not necessarily wrong, but should be visibly marked `Strategic bet — no evidence link` (Now/Next) or `Strategic bet — no open question` (Later) rather than presented as equivalent to evidence-backed items.

**Currency**

7. For each major item, answer the supporting questions from the methodology, weighting them as load-bearing vs. light: is the item still solving the right problem, has confidence changed, are there new risks or dependencies, should it continue/adapt/pause/stop/be marked done. Don't treat a missing status note as a defect on its own — flag it gently, it's expected to lag between reviews. Dependencies is a Now/Next field; status/latest-decision is Now only. Later items are direction, not active work, so don't flag either as absent there.
8. Check the "Explicitly not doing" list — has new evidence emerged that would justify revisiting a descoped item?

**Output**

Findings list, ranked by severity:
- **Broken** — evidence link no longer resolves, or the linked file's claim has reversed
- **Weakened** — confidence downgraded, new tension surfaced, or an item has drifted back into feature/task/vague framing
- **Unlinked** — item has no evidence basis (Now/Next) or open question (Later) on record and isn't visibly marked as a bet
- **Stale** — no activity or review reflected since the last cadence checkpoint

For each finding, recommend the specific action: move lane, reframe as an outcome, add an evidence link, flag for the next full review, or leave as-is with a noted rationale (this last option covers a properly-disclosed strategic bet — don't flag those as Unlinked). Do not silently edit the roadmap — findings feed the Review log, and lane changes should still be made deliberately (see `prompts/decision/draft-roadmap.md`).
