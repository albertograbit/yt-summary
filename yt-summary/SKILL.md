---
name: yt-summary
description: Search YouTube videos by topic, creator, and date range, then analyze them with NotebookLLM to generate structured summaries. Produces both Markdown and HTML output files with executive summaries, individual video analysis, cross-video trends, and key insights. Use when the user wants to research YouTube content, analyze videos from specific channels, summarize recent videos on a topic, or create video content digests. Triggers on requests like "analyze YouTube videos about X", "summarize recent videos from [channel]", "research what [creator] has been saying about [topic]", "YouTube digest", "video analysis report".
---

# YouTube to NotebookLLM Analyzer

Search YouTube videos and analyze them with NotebookLLM to produce structured summary reports.

## Workflow

1. Parse user request (topic, creators, dates, count, format)
2. Search YouTube via Firecrawl
3. Validate and filter results
4. Analyze with NotebookLLM (or Claude fallback)
5. Generate output files (Markdown + HTML)

## Step 1: Parse Request

Extract from the user's message:

| Parameter | Default | Notes |
|-----------|---------|-------|
| Topic/query | (required) | Main search terms |
| Creators | (none) | Specific channels to filter |
| Date range | Last 6 months | Convert relative dates to absolute |
| Video count | 5 | How many to analyze |
| Output format | both | `md`, `html`, or `both` |
| Language | Auto-detect | Output language matches video language unless specified |

If topic is unclear, ask before proceeding. If date range is ambiguous, ask.

## Step 2: Search YouTube

Use Firecrawl search to find YouTube videos. See [references/search-strategies.md](references/search-strategies.md) for query construction patterns.

**Search approach:**
1. Build query: `site:youtube.com [creator filter] [topic] [date hints]`
2. Execute search via Firecrawl with enough results to filter down to target count
3. For each result, scrape the YouTube page to extract full metadata (title, channel, date, duration, description)

**If Firecrawl is unavailable:** Use WebSearch as fallback with same query patterns.

Show the user a numbered list of found videos and ask for confirmation before proceeding to analysis.

## Step 3: Validate Results

Before analysis:
- Verify URLs are YouTube video pages (not playlists, shorts, or channels)
- Remove duplicates
- Confirm videos fall within date range
- If insufficient results, inform user and offer to broaden search or accept manual URLs

## Step 4: Analyze with NotebookLLM

Invoke the `notebooklm` skill with the collected YouTube URLs as sources.

**NotebookLLM instructions:**
- Add all video URLs as sources
- Request comprehensive analysis covering:
  - Individual video summaries
  - Common themes across videos
  - Key takeaways and insights
  - Points of agreement/disagreement between creators

**Fallback (NotebookLLM unavailable):** Use Firecrawl to scrape each video page for available transcript/description data, then analyze directly with Claude's capabilities.

## Step 5: Generate Output

Generate files in the current working directory (or user-specified path).

See [references/output-templates.md](references/output-templates.md) for exact templates and HTML specifications.

**File naming:** `analysis_[YYYY]_[topic_slug].md` and `analysis_[YYYY]_[topic_slug].html`

Where `topic_slug` is the topic in lowercase with spaces replaced by underscores (max 30 chars).

## Step 6: Report to User

After generating files, show:
- File paths created
- Number of videos analyzed
- Brief preview of the executive summary (2-3 lines)

## Error Handling

| Scenario | Action |
|----------|--------|
| Search returns no results | Broaden query, try without date filter, offer manual URL input |
| Some videos inaccessible | Continue with accessible ones, note skipped videos in output |
| NotebookLLM unavailable | Fall back to direct Claude analysis using scraped content |
| Firecrawl unavailable | Fall back to WebSearch |
