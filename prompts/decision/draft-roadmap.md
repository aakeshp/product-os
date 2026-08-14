# Draft or update a roadmap

Use this prompt against a Knowledge Core to produce or refresh a Now/Next/Later roadmap, per `methodology/decision-layer/roadmaps.md`.

---

You are drafting a roadmap for [team/product] for [period] using `templates/decisions/roadmap-template.md`.

1. **Read the evidence base.** From the Knowledge Core:
   - Direction — current strategic priorities this roadmap should orient around
   - Users — validated needs relevant to this period
   - Signals — recent analytics, feedback themes, experiment results, and external signals
   - Beliefs — current assumptions and their confidence levels, and any open tensions

2. **Propose items for each lane**, citing the specific evidence file behind each one:
   - **Now** — only items with strong evidence (high-confidence belief, validated user need, or a clear signal) and available capacity
   - **Next** — items evidence points toward but that haven't displaced a Now item; candidate bets, not commitments
   - **Later** — items tied to an open question or an early/weak signal; direction, not a delivery promise

   A deliberate strategic bet with no evidence file behind it can still belong on the roadmap — but never fabricate a citation to make it look evidence-backed. Write `Strategic bet — no evidence link` (or the reason it's included anyway, e.g. a leadership commitment) in the Evidence basis column instead.

3. **Frame every item as an outcome, not a feature.** Before accepting an item, check it against the item-quality table in `methodology/decision-layer/roadmaps.md`. "Build X feature" should become "users/the team can [outcome]." If an item can't be reframed as an outcome, push back on it rather than including it as-is.

4. **Surface tensions.** If two candidate items are justified by beliefs that conflict, or a Signals-layer entry contradicts a Direction-layer priority, flag it explicitly rather than silently picking a side.

5. **Name what's out.** List anything seriously considered but descoped, and why — especially where the reason is evidence-based (e.g. "belief X has low confidence and no evidence refresh is scheduled this period"). If nothing is genuinely being descoped this period, say so explicitly and note why (e.g. "no live trade-offs this period — capacity matched scope") rather than leaving the section blank.

6. **Score confidence, not just priority.** Every item gets a confidence level, independent of priority. An item can be high priority and low confidence at once — say so, don't average it away.

7. **Check each item can answer:** why it matters, what evidence/assumption led here, what's being learned/changed/enabled/de-risked, what delivery work links to it, what dependencies or risks exist, and what the latest decision is. If an item can't answer these, it's not ready for the shared roadmap yet — keep refining it in team planning first.

8. If updating an existing roadmap: diff against the current version first. Only move an item's lane if there's a specific evidence change driving it, and record that in the Review log with a date and reason. Do not silently reshuffle lanes. If a pre-existing item is already marked as an unlinked bet, carry that marking forward as-is — don't silently drop it or backfill an evidence citation for it. If evidence has since emerged for it, treat that as a real update: add the citation and note it in the Review log.

Output: the completed `roadmap-template.md` structure, plus a short summary of what changed since the last version (if updating) and which items currently have the weakest evidence.
