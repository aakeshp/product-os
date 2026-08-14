# Draft or update a roadmap

Use this prompt against a Knowledge Core to produce or refresh a whole Now/Next/Later roadmap, per `methodology/decision-layer/roadmaps.md`. For critiquing, rewriting, or drafting a single item in isolation (without deciding its lane), use `prompts/decision/evaluate-roadmap-item.md` instead.

---

You are drafting a roadmap for [team/product] for [period] using `templates/decisions/roadmap-template.md`.

1. **Read the evidence base.** From the Knowledge Core:
   - Direction — current strategic priorities this roadmap should orient around
   - Users — validated needs relevant to this period
   - Signals — recent analytics, feedback themes, experiment results, and external signals
   - Beliefs — current assumptions and their confidence levels, and any open tensions

   Also ask for **available capacity** — how much the team can realistically take on this period, and any known constraints. This isn't Knowledge Core evidence (it isn't one of the four layers) — it's operational context only the team can supply, and Now-lane placement depends on it as much as evidence does. Don't infer capacity from the evidence alone.

2. **Propose items for each lane**, citing the specific evidence file behind each one:
   - **Now** — only items with strong evidence (high-confidence belief, validated user need, or a clear signal) *and* capacity confirmed by the team
   - **Next** — items evidence points toward but that haven't displaced a Now item; candidate bets, not commitments
   - **Later** — items tied to an open question or an early/weak signal; direction, not a delivery promise

   A deliberate strategic bet with no evidence file behind it can still belong on the roadmap — but never fabricate a citation to make it look evidence-backed. For Now/Next, write `Strategic bet — no evidence link` (or the reason it's included anyway, e.g. a leadership commitment) in the Evidence basis column. Later has no Evidence basis column — use its "Open question" column instead, writing `Strategic bet — no open question` if it's a deliberate direction rather than something resolving a specific question.

3. **Check every item against the five tests** in `methodology/decision-layer/roadmaps.md`: intent not feature, clear why it matters, understandable outside the team, room for the team to decide how, specific enough to be meaningful. "Build X feature" should become "users/the team can [outcome]." If an item can't be reframed as an outcome, push back on it rather than including it as-is.

   Items don't have to be user-facing to pass — a technical investment, discovery bet, or ways-of-working experiment passes on the same five tests, judged against what it enables rather than whether it's user-facing (see the item-type list in the methodology). If an item opens with "Explore…" or "Test…" (a discovery bet), check it states *why* the learning matters, not just what will be learned — this is the most common thing to silently drop.

4. **Use the title/summary split when a title alone can't carry the specificity.** Keep the title short and outcome-framed; put what "better" looks like in the Summary column. Judge title and summary together — don't flag a short title as vague if the summary pins it down.

5. **Surface tensions.** If two candidate items are justified by beliefs that conflict, or a Signals-layer entry contradicts a Direction-layer priority, flag it explicitly rather than silently picking a side.

6. **Name what's out.** List anything seriously considered but descoped, and why — especially where the reason is evidence-based (e.g. "belief X has low confidence and no evidence refresh is scheduled this period"). If nothing is genuinely being descoped this period, say so explicitly and note why (e.g. "no live trade-offs this period — capacity matched scope") rather than leaving the section blank.

7. **Score confidence, not just priority.** Every item gets a confidence level, independent of priority. An item can be high priority and low confidence at once — say so, don't average it away.

8. **Check the supporting questions, graded by how load-bearing they are** (see methodology). Load-bearing — why it matters, what led here, what's being changed/learned/enabled/de-risked — must be present or clearly inferable; if an item can't answer these, it's not ready for the shared roadmap yet, keep refining it in team planning first. Light/optional — linked delivery work, dependencies/risks, latest decision — flag gently if absent, don't treat it as a quality failure.

9. **Match the organisation's register**, even for internal-only items. A technically accurate word can still be the wrong tone (e.g. "cheap" vs. "lower-effort") — see the methodology's Register and tone section.

10. If updating an existing roadmap: diff against the current version first. Only move an item's lane if there's a specific evidence change driving it, and record that in the Review log with a date and reason. Do not silently reshuffle lanes. If a pre-existing item is already marked as an unlinked bet, carry that marking forward as-is — don't silently drop it or backfill an evidence citation for it. If evidence has since emerged for it, treat that as a real update: add the citation and note it in the Review log.

Output: the completed `roadmap-template.md` structure, plus a short summary of what changed since the last version (if updating) and which items currently have the weakest evidence.
