# Roadmaps — Decision Layer Methodology

## What it is

A roadmap is a tool for communicating **intent and direction**. It is not a list of features to be built, and it is not a delivery plan. It uses a shared **Now / Next / Later** format so that anyone outside the team — other teams, leadership, partners — can quickly understand what a team is focused on, what it may explore next, and where it's headed longer term.

## The problem it solves

Most roadmaps are opinion made to look like a plan. They get built once, under pressure to look decisive and complete, and then ossify — because revising a published, dated roadmap reads as failure rather than as a team responding to what it's learned.

The Decision Layer treats the roadmap as the *organisation-facing summary* of an evidence-guided planning process, not the planning process itself. The detailed thinking — evidence, assumptions, bets, trade-offs — lives underneath, in whatever format suits the team. The roadmap is what gets shared upward and outward, and it should stay short, honest, and current enough to be trusted.

## Two layers: team planning vs. shared roadmap

These are deliberately different things, and conflating them is the most common way roadmaps go stale or become feature lists:

- **Team planning (flexible format).** How a team works out what to focus on — GIST, opportunity mapping, bet tables, assumption maps, or something else entirely. This can be a Miro board, a Notion page, an AI-assisted working doc — whatever helps the team move from a broad set of inputs (user evidence, strategic priorities, constraints, capacity) to a clear set of choices. Format is a team's own call.
- **Shared roadmap (fixed format).** What flows *out* of that planning into a common, organisation-wide Now/Next/Later structure. The format is standardised precisely so it's readable and comparable across teams — this is what enables good cross-team trade-off conversations and dependency spotting.

A team's planning method can and should vary. The roadmap format should not.

## Principles

- **Outcomes and intent, not features.** A roadmap item should describe a user problem, an opportunity, a risk to reduce, a constraint to remove, a strategic theme, or a better way of working — not a shipped feature. See the five tests below.
- **Confidence falls as the horizon extends.** Now should be concrete. Next should read as candidate bets, not commitments. Later should read as direction, not a promise.
- **Evidence-linked, not opinion-linked.** Every roadmap item should be traceable to Knowledge Core evidence — a belief, a signal, or user research — even if the underlying team-planning method that produced it varies. If an item has no evidence link, that should be visible, not hidden behind confident-sounding language.
- **Explicitly not doing.** Naming what's deliberately out of scope, and why, reduces repeated relitigating of decisions already made.
- **Living, not static.** A roadmap set once at the start of a planning period and never revisited is not evidence-guided — it's a snapshot of the day it was written.

## The five tests of a good item

A strong roadmap item should pass all five:

1. **Expresses intent or an outcome, not a feature or task.** It describes a user problem, an opportunity, a risk to reduce, a constraint to remove, a strategic theme, or a better way of working — not the thing to be built. "Add a sharing feature" is a feature; "Users can share activity with others quickly and reliably" is an outcome.
2. **Makes clear why the work matters.** A reader should understand what progress, learning, or change the team is aiming for. It should imply value for users and/or the organisation.
3. **Is understandable to someone outside the team.** Plain language, no unexplained jargon, no internal project codenames standing in for meaning.
4. **Leaves room for the team to decide how.** It should not read like a fixed feature commitment or a detailed delivery plan. The "how" — sequencing, testing, delivery — sits beneath the roadmap in the team's own planning space.
5. **Is specific enough to be meaningful.** Vague aspirations like "improve search" or "increase engagement" don't say what better looks like. A good item points at a concrete change in what users can do, or what the organisation can rely on.

Not all work is user-facing. For platform or foundational work, a good item describes **what it enables, what risk it reduces, or how it helps the organisation deliver more value** — e.g. helping other teams move faster, make safer changes, or rely on better data and tools. "Refactor the data pipeline" names a task; "Teams can rely on this data being accurate, structured, and easy to reuse" is the enabling outcome.

| Less useful | Better |
|---|---|
| Add a search filter | Users can find what they need in under a minute |
| Build a notifications feature | Users find out about relevant updates without having to check back manually |
| Refactor the data pipeline | Downstream teams can rely on this data being accurate, structured, and easy to reuse |
| Improve the QA process | We can release changes with greater confidence and fewer late-stage issues |
| Try AI tools | The team can test where AI meaningfully improves quality without reducing trust |
| Fix tech debt | Reduce the technical constraints slowing down [specific future work] |

What the "Better" column has in common: it states an outcome or capability rather than an action on the product, names who benefits where relevant, hints at the standard of success without prescribing the solution, and stays one clear sentence.

## Item types

A roadmap item is a meaningful area of work or intent — it does not have to be a user-facing feature outcome:

- **User job / problem** — something users are trying to do, or struggling with.
- **Opportunity** — a chance to improve the product or create value.
- **Capability** — a new thing the product or organisation can do.
- **Technical investment** — foundational work; frame by what it enables or what risk it reduces, not the task itself.
- **Growth initiative** — reach, adoption, retention.
- **Discovery bet** — something to investigate or test where confidence is still forming; often "Explore…", "Test…", "Understand…", "Investigate…".
- **Ways-of-working experiment** — a better way for the team to work.
- **Strategic theme** — longer-term direction the team is moving towards.

All eight still have to pass the five tests. Don't penalise a discovery bet or a ways-of-working item for not being user-facing — judge it on the same five tests as everything else.

**Watch the "why" especially on discovery bets.** Once an item opens with "Explore…" or "Test…" it's easy to nail *what* you'll learn and silently drop *why it matters*. "Understand how users choose between competing options" works; "Do some research" doesn't. A scoped bet still needs the reason stated — in the title or carried in the summary — or it reads as activity for its own sake.

## Title and summary are one job split across two fields

An intent sentence that passes all five tests can still be too long-winded to sit as a roadmap title. When that happens, don't force the whole sentence into the title — split it:

- **Title** — short, names the intent or outcome, readable at a glance.
- **Summary** — one or two sentences carrying the specificity the title leaves out: what *better* looks like, or what the work brings into line.

This is the same intent statement distributed across two fields, not two different things — the summary is where test 5's specificity lives when the title is kept short.

**Judge title and summary together, not the title alone.** A short title will often read as borderline-vague in isolation — that's fine, as long as the summary pins down what better means. Don't penalise a title for leaning on its summary; that's the split working as intended. If an item is a bare title with no summary, the title alone must carry the specificity.

## How much detail belongs in an item

The roadmap item is the intent-level statement — usually one clear sentence, plus an optional summary. The deep detail (full evidence, delivery breakdown, sequencing, dependencies) lives underneath, in the team's own planning space, and the item can simply **link** to that backlog or planning entry rather than duplicating it. A link stays current; a copy drifts out of date the moment the underlying plan changes.

Against an item, two more things can usefully sit *on the roadmap itself* without turning it into a delivery plan:

- A short **summary** (see above).
- A **status / "what's happening now" note** — the latest decision or what's in progress, which keeps the roadmap honest between formal reviews.

If someone hands you a backlog link or a pasted task breakdown with no intent sentence at all, that's the signal to help write the missing intent — not to evaluate the link or the task list as if it were the item.

## Register and tone

Word choice matters even on items that will only ever be read inside the organisation. A word can be technically accurate and still land wrong — "cheap" correctly describes low-effort maintenance but reads as off-brand where "easier to maintain" or "lower-effort" fits. Don't treat "it's just internal" as licence for register that doesn't match how the organisation actually talks about its work.

## Common failure modes to flag

- **Feature framing** — "Add / Build / Launch / Ship [thing]": names the solution, not the intent.
- **Task framing** — "Refactor / Fix / Migrate / Improve [internal thing]": names the activity, not what it enables.
- **Vagueness** — "Improve / Increase / Optimise [X]" with no sense of what better means.
- **Jargon or codename** — unreadable to someone outside the team.
- **Over-specified delivery plan** — reads like a committed spec, leaving no room for the team to decide how.
- **Missing the "so what"** — no clear user or organisational value; can't answer "why does this matter?"

## Structure: Now / Next / Later

The horizons are roughly time-bound, but they're mainly about **focus and confidence**, not calendar dates.

- **Now** — Work the team is actively focused on, or has high confidence will progress soon. Can include user-facing work, enabling work, active discovery, delivery linked to current priorities, near-term dependencies, or risk reduction.
- **Next** — Work the team may explore once there's capacity or confidence to move beyond Now. Not a firm commitment — candidate bets, follow-on work, problems to investigate, or opportunities needing more evidence. These items should inform what discovery work is worth doing now.
- **Later** — Longer-term vision, strategic themes, or direction. Not a prediction of *when* — a signal of what's important but deliberately not being worked on yet.

Items move between lanes as evidence and confidence change — a Next item can drop to Later if the belief behind it weakens; a Later item can jump to Next if a new signal strengthens it. Deciding which lane an item belongs in is a sequencing judgement — it depends on the team's capacity, confidence, and dependencies, none of which the item's wording alone reveals. Don't infer a placement from text quality; a well-written item can belong in any lane.

**A register tell helps with writing (not deciding) the right horizon:**
- A **Now** item names a concrete change underway.
- A **Next** item usually opens as a bet or question — "Explore…", "Test…", "Understand…", "Investigate…".
- A **Later** item reads as a standing intent or theme — "Be…", "Enable…", "Become…", "Treat … as …" — directional and outcome-shaped, deliberately not time-bound.

Items at every horizon must still pass the five tests. A Later item is pitched at the level of direction rather than a specific deliverable, but it's still a real, well-formed item — not an excuse for vagueness.

## What the roadmap should communicate

The roadmap doesn't need every detail, but for each major item it should be possible to answer:

- Why does this matter?
- What evidence, context, or assumption led here?
- What are we trying to learn, change, enable, or reduce risk around?
- What delivery work is linked to it?
- What dependencies or risks should people know about?
- What's the latest decision?

These don't all need to be spelled out in the item text — a good item should make the first three reasonably inferable and shouldn't contradict them. Grade the rest by how strictly to enforce them, so the check sharpens items without nagging:

- **Load-bearing** (must be present or clearly inferable): why it matters, what led here, and what's being changed, learned, enabled, or de-risked.
- **Light / optional** (flag absence gently, never as a quality failure): linked delivery work, dependencies and risks, and the latest decision. The status note in particular updates on the team's own review cadence — its absence at any single moment isn't a fault.

## Keeping the roadmap current

A roadmap set at the start of a planning period and never revisited stops being trustworthy. A light, sustainable rhythm:

| Cadence | What to do |
|---|---|
| Every sprint / fortnight | Check current work still links to roadmap items. Update obvious changes as they happen — don't wait for a formal review. |
| Monthly | Review Now / Next / Later as a team. Check progress, confidence, blockers, and learning. |
| Mid-period | Bigger trade-off moment — decide whether anything should move, pause, stop, or be de-scoped. Check the roadmap still reflects the team's best current understanding. |
| End of period | Capture what changed, what was learned, and what should inform the next planning cycle — so the next period is a continuation, not a blank page. |

For each major item at review time:
- Is this still in the right lane — Now, Next, or Later?
- What's happened since the last review?
- What have we learned, and has confidence changed?
- Are we still solving the right problem, or enabling the right thing?
- Are there new risks, blockers, or dependencies?
- Should we continue, adapt, pause, stop, or mark as done?
- Who needs to know about any change?

## Where roadmap evidence comes from

Roadmap items should draw on the Knowledge Core's four layers (see `methodology/knowledge-core-methodology/overview.md`):

- **Direction** — the strategic priority or goal an item serves.
- **Users** — the validated need or research finding an item responds to.
- **Signals** — the analytics shift, feedback theme, or experiment result that raised or lowered an item's priority.
- **Beliefs** — the confidence level behind an item, and whether it creates tension with another current belief.

Use judgement: some items will be strongly evidenced; others are a strategic bet. Both are fine, as long as the level of certainty is clear rather than implied.

## File conventions

```yaml
---
type: roadmap
status: draft | committed | archived
last_updated: YYYY-MM-DD
review_cadence: monthly
related: [links to the Direction/Users/Signals/Beliefs files this roadmap responds to]
---
```

See `templates/decisions/roadmap-template.md` for the full structure.

## Relationship to the Knowledge Core

The roadmap should reference Knowledge Core evidence, not restate it. An item's evidence basis should link to the specific belief or signal file, not paraphrase it — keeping the roadmap short enough to actually be read, and meaning an update to the underlying belief is felt in the roadmap automatically. The same principle applies to any linked planning detail underneath an item: link to the source, don't recreate it, or the copy will drift out of date the moment the source changes.

## See also

- `templates/decisions/roadmap-template.md` — the blank roadmap scaffold
- `prompts/decision/draft-roadmap.md` — prompt to draft or update a whole roadmap from current Knowledge Core evidence
- `prompts/decision/audit-roadmap-evidence.md` — prompt to check an existing roadmap for weak or stale evidence links
- `prompts/decision/evaluate-roadmap-item.md` — prompt to evaluate, rewrite, or draft a single item without deciding its lane
- `.claude/skills/craft-roadmap-builder` — drafts, updates, or audits a whole roadmap (places items in lanes, since it gathers both evidence and capacity across the full period)
- `.claude/skills/craft-roadmap-item` — evaluates, rewrites, or drafts individual items (never places a lane — it only ever sees one item at a time, without the capacity/confidence context that placement needs)
