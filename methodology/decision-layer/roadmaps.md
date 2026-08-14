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

- **Outcomes and intent, not features.** A roadmap item should describe a user problem, an opportunity, a risk to reduce, a constraint to remove, or a strategic theme — not a shipped feature. See the item-quality table below.
- **Confidence falls as the horizon extends.** Now should be concrete. Next should read as candidate bets, not commitments. Later should read as direction, not a promise.
- **Evidence-linked, not opinion-linked.** Every roadmap item should be traceable to Knowledge Core evidence — a belief, a signal, or user research — even if the underlying team-planning method that produced it varies. If an item has no evidence link, that should be visible, not hidden behind confident-sounding language.
- **Explicitly not doing.** Naming what's deliberately out of scope, and why, reduces repeated relitigating of decisions already made.
- **Living, not static.** A roadmap set once at the start of a planning period and never revisited is not evidence-guided — it's a snapshot of the day it was written.

## What makes a good roadmap item

A good item is clear enough for someone outside the team to understand, while leaving room for the team to decide how to approach it. It should not read like a fixed feature commitment. It should make clear *why the work matters* and *what kind of progress, learning, or change* is being aimed at.

| Less useful | Better |
|---|---|
| Add a search filter | Users can find what they need in under a minute |
| Build a notifications feature | Users find out about relevant updates without having to check back manually |
| Refactor the data pipeline | Downstream teams can rely on this data being accurate, structured, and easy to reuse |
| Improve the QA process | We can release changes with greater confidence and fewer late-stage issues |
| Try AI tools | The team can test where AI meaningfully improves quality without reducing trust |
| Fix tech debt | Reduce the technical constraints slowing down [specific future work] |

Work that isn't user-facing (platform, data, tooling, enabling work) still belongs on the roadmap — it should be clear what it enables, what risk it reduces, or how it helps deliver more value over time.

## Structure: Now / Next / Later

The horizons are roughly time-bound, but they're mainly about **focus and confidence**, not calendar dates.

- **Now** — Work the team is actively focused on, or has high confidence will progress soon. Can include user-facing work, enabling work, active discovery, delivery linked to current priorities, near-term dependencies, or risk reduction.
- **Next** — Work the team may explore once there's capacity or confidence to move beyond Now. Not a firm commitment — candidate bets, follow-on work, problems to investigate, or opportunities needing more evidence. These items should inform what discovery work is worth doing now.
- **Later** — Longer-term vision, strategic themes, or direction. Not a prediction of *when* — a signal of what's important but deliberately not being worked on yet.

Items move between lanes as evidence and confidence change — a Next item can drop to Later if the belief behind it weakens; a Later item can jump to Next if a new signal strengthens it.

## What the roadmap should communicate

The roadmap doesn't need every detail, but for each major item it should be possible to answer:

- Why does this matter?
- What evidence, context, or assumption led here?
- What are we trying to learn, change, enable, or reduce risk around?
- What delivery work is linked to it?
- What dependencies or risks should people know about?
- What's the latest decision?

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

The roadmap should reference Knowledge Core evidence, not restate it. An item's evidence basis should link to the specific belief or signal file, not paraphrase it — keeping the roadmap short enough to actually be read, and meaning an update to the underlying belief is felt in the roadmap automatically.

## See also

- `templates/decisions/roadmap-template.md` — the blank roadmap scaffold
- `prompts/decision/draft-roadmap.md` — prompt to draft or update a roadmap from current Knowledge Core evidence
- `prompts/decision/audit-roadmap-evidence.md` — prompt to check an existing roadmap for weak or stale evidence links
- `.claude/skills/craft-roadmap-builder` — the skill that runs this methodology end to end
