# Audit a roadmap

Use this prompt to check whether an existing roadmap still holds up, per `methodology/decision-layer/roadmaps.md`. Good to run on the review cadence set in the roadmap's frontmatter (sprint/fortnightly light check, monthly team review, mid-period trade-off review, end-of-period retro), or whenever a significant Signals-layer update lands.

---

You are auditing [roadmap file] against its own content and the current state of the Knowledge Core.

**Item framing**

1. For every item, check it's still framed as an outcome/intent, not a feature — flag any that have drifted into feature-list language (e.g. "build X" instead of "users can Y").

**Evidence and confidence**

2. Check each evidence link still resolves, and that the linked file still says what the roadmap claims.
3. Check whether the linked belief's confidence has changed since `last_updated`. A Now item resting on a downgraded belief should be flagged immediately, not left for the next scheduled review.
4. Check for new Signals-layer updates since `last_updated` that create tension with a Now or Next item.
5. Check for items with no evidence link at all — not necessarily wrong, but should be visibly marked as a bet rather than presented as equivalent to evidence-backed items.

**Currency**

6. For each major item, answer: is this still in the right lane? What's happened since the last review? Has confidence changed? Are we still solving the right problem? Are there new risks, blockers, or dependencies? Should it continue, adapt, pause, stop, or be marked done?
7. Check the "Explicitly not doing" list — has new evidence emerged that would justify revisiting a descoped item?

**Output**

Findings list, ranked by severity:
- **Broken** — evidence link no longer resolves, or the linked file's claim has reversed
- **Weakened** — confidence downgraded, new tension surfaced, or an item has drifted back into feature-list framing
- **Unlinked** — item has no evidence basis on record
- **Stale** — no activity or review reflected since the last cadence checkpoint

For each finding, recommend the specific action: move lane, reframe as an outcome, add an evidence link, flag for the next full review, or leave as-is with a noted rationale. Do not silently edit the roadmap — findings feed the Review log, and lane changes should still be made deliberately (see `prompts/decision/draft-roadmap.md`).
