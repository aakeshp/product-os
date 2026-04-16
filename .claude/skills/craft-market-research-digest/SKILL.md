---
name: digest
description: Generate a weekly research digest from RSS feeds and web searches. Reads topics and sources from Research/sources.yaml, fetches feeds, searches the web, summarizes top articles, and writes a digest to Research/YYYY-WXX.md.
---

# Research Digest

Generate a curated weekly research digest from RSS feeds and web searches.

## Config

Sources are defined in `Research/sources.yaml`. If the file doesn't exist, create it with starter defaults and tell the user to customize it.

## Step 1: Read Config

Read `Research/sources.yaml` and parse:
- `max_per_section` (default: 10)
- `lookback_days` (default: 7)
- `topics` — each with `name` and `search_queries` (list of strings)
- `feeds` — each with `name` and `url`

If the file is missing, create it with the starter template (see `Research/sources.yaml` in the repo) and tell the user: "Created Research/sources.yaml with starter defaults. Edit it to customize your topics and feeds, then run /digest again."

## Step 2: Fetch RSS Feeds

For each feed in config:
1. Use Bash to `curl -sL --max-time 15` the feed URL
2. Parse the XML/Atom response to extract items: title, link, publication date, snippet/description
3. Filter to items published within `lookback_days` of today
4. If curl fails or returns invalid XML, log a warning and skip that feed

Collect all feed items into a single list: `{ title, url, date, snippet, source_name }`.

## Step 3: Web Search

For each topic in config:
1. For each `search_query` in the topic, run `WebSearch` with the query
2. Collect results: title, URL, snippet
3. Deduplicate against RSS items by URL (skip any URL already found in feeds)
4. Tag each result with the topic name

Collect all search results into a list: `{ title, url, snippet, topic, source_name }`.

## Step 4: Read Top Articles

From the combined pool (RSS items + search results):
1. For each topic, select up to `max_per_section` items, ranked by:
   1. **Recency** — newer articles rank higher (within the lookback window)
   2. **Source authority** — established publications and official bodies above personal blogs and link aggregators
   3. **Relevance** — directly on-topic beats tangentially related
   4. **Novelty** — new research, policy, or product launches above commentary on existing news
   - For RSS items not yet assigned to a topic, assign them based on content relevance
2. For each topic, WebFetch the top 5 items only (for full summaries). Remaining items in the section use their RSS/search snippet as the summary. Do NOT WebFetch more than 15 articles total.
   - Prompt: "Extract: (1) the main argument and conclusions in 2-3 sentences, (2) the single most surprising, contentious, or quotable claim, (3) why this matters specifically for someone working in your field. Ignore navigation, ads, and sidebars."
   - If WebFetch fails (paywall, 404, timeout), fall back to the RSS/search snippet

## Step 5: Generate Digest

Calculate the current ISO week: `YYYY-WXX` format (e.g., `2026-W07`).

Write the digest to `Research/YYYY-WXX.md` with this format:

```markdown
---
week: "YYYY-WXX"
generated: YYYY-MM-DDTHH:MM:SS
sources: N feeds, M searches
---
# Research Digest — WXX YYYY

## Highlights
> 2-3 sentence summary of the most noteworthy finds this week across all topics.

## Topic Name

- **[Article title](url)** — *Source name*
  > 2-3 sentence summary: what it says, why it matters.

- **[Article title](url)** — *Source name*
  > Summary.

## Next Topic

- **[Article title](url)** — *Source name*
  > Summary.

---

## All Feed Items
> Quick-reference list of everything from RSS this week (title + link, no summaries).

- [Title](url) — *Feed name* — YYYY-MM-DD
- [Title](url) — *Feed name* — YYYY-MM-DD
```

**Rules:**
- One `## Section` per topic from config, in the order they appear in `sources.yaml`
- Each article: bold linked title, italicized source, blockquote summary
- Summaries should say what the article says AND why it matters
- The Highlights section picks the 2-3 most impactful items across all topics
- The "All Feed Items" section at the bottom lists everything from RSS (not search results), sorted by date descending — title, link, feed name, and date only (no summaries)
- If re-running the same week, overwrite the existing file
- Create the `Research/` directory if it doesn't exist

## Step 5b: Generate Podcast Script

After writing the digest, generate a companion podcast script at `Research/YYYY-WXX-podcast.md`.

**Frontmatter:**
```markdown
---
week: "YYYY-WXX"
type: podcast-script
hosts: ["Alex", "Sam"]
notebooklm: Paste this file into NotebookLM as a text source, then click "Audio Overview"
---
```

**Script structure (target: ~2000–2500 words):**

1. **Cold open** (2–3 exchanges): One host teases the single biggest finding of the week before any formal intro.

2. **Intro** (1–2 exchanges): Week overview — "three things we're talking about today" — sets up what to listen for.

3. **Topic segments** (one per topic from sources.yaml, 4–6 exchanges each):
   - Lead story: the most surprising/contentious finding and why it matters
   - 1–2 supporting stories that reinforce or contrast
   - "So what for us" moment: explicit connection to your field
   - Hosts should disagree or push back where reasonable — make it a conversation, not a list

4. **Cross-topic tension** (2–3 exchanges): Find a thread that cuts across topics and let the hosts debate it.

5. **Rapid fire** (4–6 items): Quick 1-line takes on notable smaller stories that didn't get a full segment.

6. **Outro** (1–2 exchanges): What to watch next week, sign-off.

**Tone rules:**
- Hosts use first names only ("Alex", "Sam"), no job titles
- Conversational but substantive — no filler phrases like "great question" or "absolutely"
- Reference specific numbers, names, and quotes from articles — not vague generalisations
- Hosts can be sceptical, surprised, or enthusiastic — vary the register
- Keep each exchange 2–4 sentences per speaker
- Label each turn as `**Alex:**` or `**Sam:**`

If re-running the same week, overwrite the existing podcast file.

## Step 6: Report

Print a terminal summary:

```
# Digest — WXX YYYY

Found N items across X feeds + Y searches.
Curated Z articles into T topics.

## Top picks
- [Article title](url) — Topic Name
- [Article title](url) — Topic Name
- [Article title](url) — Topic Name

Written to Research/YYYY-WXX.md
Podcast script written to Research/YYYY-WXX-podcast.md
→ Paste the podcast file into NotebookLM as a text source, then click Audio Overview
```

Show one top pick per topic (the single most noteworthy article from each).

## Step 7: Commit

After writing the digest, commit the updated file to git:

1. Stage both files: `git add Research/YYYY-WXX.md Research/YYYY-WXX-podcast.md`
2. Commit with message: `digest: update YYYY-WXX`
   - If this is the first digest for the week, use: `digest: create YYYY-WXX`
3. Do NOT push — just commit locally. The user can push when ready.

To determine create vs update: check if the file existed before this run
(i.e. was it read in Step 1 or created fresh).
