# Knowledge Core — Methodology Overview

## What it is

The Knowledge Core is the foundation of the Product OS. It is a living, structured evidence base that captures everything a PM needs to form good product intuition — and maintains the relationships between those inputs so synthesis happens continuously rather than manually at decision points.

It replaces the expensive, lossy process of reconstructing context every time a significant decision needs to be made.

## The problem it solves

In most organisations, product intuition lives in people's heads. It accumulates through experience, conversation, and exposure to evidence over time — and it's genuinely valuable. But it's also invisible, inconsistent across a team, and leaves when people do.

The result is that context gets reconstructed repeatedly. Every significant decision, every steering paper, every onboarding of a new PM requires someone to manually pull together strategy documents, research files, analytics, and funder or sponsor context that already exists somewhere but not in a connected, trusted form. The quality of decisions depends heavily on who happens to be in the room and how recently they engaged with each source.

The Knowledge Core doesn't replace product intuition — it externalises and augments it. It makes the evidence base that good intuition rests on persistent, shared, and continuously updated, so that the synthesis work happens once and compounds over time rather than being repeated from scratch at every decision point.

## The four input layers

### Direction
Stable but evolving context that sets orientation. Updates on strategy cycles.

- Mission and theory of change
- Organisational strategy
- OKRs and goals

*What makes it evidence-guided:* Direction inputs should carry the evidence and reasoning that produced them, not just the conclusions. A strategy choice should be traceable to the research and context that informed it.

*Note for arms-length and independent organisations:* The Direction layer reflects your organisation's own strategy and theory of change — not your funder or sponsor's priorities directly. Many public sector organisations operate with a degree of independence from their funder. If that applies to you, your funder's priorities belong in the Signals layer as external context to monitor, not here. They matter because shifts in funder priorities may create tension with your strategy over time — but the interpretation of those priorities, and the strategic choices that follow, are yours.

### Users
What you know about the people you serve. Updates when significant new research arrives.

- User research findings
- Personas and segments
- Journey maps
- Market and sector analysis
- Impact reports

*What makes it evidence-guided:* User knowledge should carry confidence levels and recency indicators. "We believe teachers prioritise X" should be accompanied by the evidence that belief rests on and how recently it was validated.

### Signals
The continuous stream of incoming evidence. Updates frequently — some inputs continuously.

- Analytics summaries
- Support ticket themes
- Experiment results
- Funder or sponsor priorities and policy updates
- Partner and stakeholder feedback

*Note on funder priorities:* For organisations operating independently from a funder or sponsor, their priorities belong here rather than in the Direction layer. You monitor them as external signals that may create tension with your own strategy — you don't implement them directly.

*What makes it evidence-guided:* Signals should be interpreted, not just logged. Each signal update should note what it might mean and whether it creates tension with current beliefs.

### Beliefs
The synthesised interpretation layer. The most important and most novel part of the Knowledge Core.

- Current assumptions (with confidence levels and evidence basis)
- Open questions (prioritised by decision relevance)
- Known tensions (where inputs contradict or create friction)
- Confidence scores (how well-evidenced current strategic positions are)

*What makes it evidence-guided:* Beliefs are explicit, not implicit. Every belief carries the evidence it rests on, a confidence level, and — critically — what evidence would change it.

## The synthesis principle

The Knowledge Core is not a document store. The value is not in individual files but in the relationships between them.

A funder priority that creates tension with a user research finding that is showing up in support ticket themes — that connection is where the real product insight lives. The Knowledge Core is designed to make those connections visible rather than leaving them to individual PMs to spot manually.

## How it stays current

| Layer | Update cadence | Trigger |
|-------|---------------|---------|
| Direction | Quarterly / strategy cycles | Strategy review, policy change |
| Users | As research arrives | New research, significant user event |
| Signals | Continuously / weekly | Analytics, tickets, experiments |
| Beliefs | When significant new evidence arrives | Signal update, research finding, policy shift |

## How it gets queried

Through structured prompts in `/prompts/` that ask the Knowledge Core specific questions:

- What do we currently believe about X?
- Where is our evidence weakest?
- What tensions exist between Y and Z?
- What has changed in our signals layer this month?
- Which assumptions have the lowest confidence scores?

## The weekly synthesis habit

A standing agent instruction that scans recent signal updates and asks: does anything here create tension with current beliefs or strategy? This is the early warning system — surfacing issues before they require a steering paper.

See `/prompts/synthesis/weekly-synthesis.md` for the prompt.

## File conventions

Every Knowledge Core file should open with structured frontmatter:

```yaml
---
type: [direction/user/signal/belief]
last_updated: YYYY-MM-DD
confidence: [high/medium/low]
source: [where this came from]
related: [links to connected files]
review_cadence: [continuous/monthly/quarterly/annual]
---
```

This is what makes the Knowledge Core queryable by an agent.

## What this is not

The Knowledge Core is not a project management tool, a roadmap, or a task list. It does not replace your sprint planning or backlog. It is the evidence layer that informs those things — the difference between opinion-driven and evidence-guided product decisions.

## Relationship to the Discovery Engine

The Knowledge Core is the destination for evidence. The Discovery Engine is the methodology for generating and synthesising it. They are designed as connected parts — every discovery practice feeds outputs into the Knowledge Core in a consistent, structured format.

See `/methodology/discovery-engine/overview.md` for the Discovery Engine methodology.