---
name: yt-summary
description: Search YouTube videos by topic, creator, and date range, then analyze them with NotebookLM to generate actionable summaries. Produces both Markdown and HTML output files with prioritized recommendations, specific clips to watch, use cases, and implementation actions. Use when the user wants to research YouTube content, analyze videos from specific channels, summarize recent videos on a topic, or create video content digests. Triggers on requests like "analyze YouTube videos about X", "summarize recent videos from [channel]", "research what [creator] has been saying about [topic]", "YouTube digest", "video analysis report".
---

# YouTube Analyst with NotebookLM

Search YouTube videos, analyze them through NotebookLM, and produce actionable briefings (not just summaries).

## Workflow

1. Parse user request (topic, creators, dates, count, format)
2. Search YouTube via Firecrawl
3. Validate, filter, and collect view counts
4. **Always** create NotebookLM notebook with video URLs as sources
5. Query NotebookLM for deep analysis with citations
6. Generate action-oriented output files (Markdown + HTML)

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
3. For each result, collect metadata: title, channel, date, duration, description, **view count**

**View counts are mandatory.** Use the Return YouTube Dislike API (`returnyoutubedislikeapi.com/votes?videoId=VIDEO_ID`) or scrape from YouTube pages. View count is a key ranking signal.

**If Firecrawl is unavailable:** Use WebSearch as fallback with same query patterns.

Show the user a numbered list of found videos **ranked by views** and ask for confirmation before proceeding.

## Step 3: Validate Results

Before analysis:
- Verify URLs are YouTube video pages (not playlists, shorts, or channels)
- Remove duplicates
- Confirm videos fall within date range
- **Rank by view count** - views signal community validation
- If insufficient results, inform user and offer to broaden search or accept manual URLs

## Step 4: NotebookLM Analysis (REQUIRED)

**This step is NOT optional.** Always create a NotebookLM notebook and add video URLs as sources.

```bash
# 1. Create notebook
notebooklm create "[Topic] - YouTube Analysis [date]" --json

# 2. Add each video URL as source
notebooklm source add "https://youtube.com/watch?v=VIDEO_ID" --json
# Repeat for each video

# 3. Wait for sources to process (spawn background agent if many)
notebooklm source list --json  # Check status

# 4. Query for deep analysis
notebooklm ask "For each video source, provide: (1) the 3 most actionable takeaways, (2) specific timestamps of the most valuable segments, (3) concrete things to implement based on the content. Focus on WHAT TO DO, not what the video says." --json

# 5. Follow-up queries
notebooklm ask "What are the common implementation patterns across all videos? What should someone implement first, second, third?" --json
notebooklm ask "Which videos contradict each other? Where do creators disagree?" --json
```

**If NotebookLM auth is expired:** Ask user to run `! notebooklm login`, then retry.
**If NotebookLM is completely unavailable:** Fall back to Firecrawl scraping + Claude analysis. Note in output that NotebookLM was not used.

## Step 5: Generate Action-Oriented Output

Generate files in the current working directory (or user-specified path). See [references/output-templates.md](references/output-templates.md) for format guidance.

**The output must be ACTION-ORIENTED, not descriptive:**

### Required sections (in order):
1. **TL;DR - Top 3 actions** to take this week based on video content
2. **Ranking table** ordered by views, with priority badges and specific clips/timestamps to watch
3. **What to implement** - concrete action cards with "why", "what to do", and "reference video + timestamp"
4. **Use cases** extracted from videos, grouped by domain
5. **What to skip/ignore** - save the reader's time
6. **Context** - releases, news, or events mentioned across videos

### Key principles:
- View count is displayed prominently for every video
- Every video has a **priority badge** (Alta/Media/Baja/Skip)
- Every video has **specific clips** to watch (timestamps or time ranges), not "watch the whole thing"
- Recommendations say WHAT TO DO, not what the video says
- "Skip" recommendations are as valuable as "watch" recommendations

**File naming:** `analysis_[YYYY]_[topic_slug].md` and `analysis_[YYYY]_[topic_slug].html`

## Step 6: Report to User

After generating files, show:
- File paths created
- Number of videos analyzed
- NotebookLM notebook URL/ID for further exploration
- The TL;DR (top 3 actions) inline

## Error Handling

| Scenario | Action |
|----------|--------|
| Search returns no results | Broaden query, try without date filter, offer manual URL input |
| Some videos inaccessible | Continue with accessible ones, note skipped videos in output |
| NotebookLM auth expired | Ask user to run `! notebooklm login`, then retry |
| NotebookLM completely unavailable | Fall back to Claude analysis, note limitation in output |
| Firecrawl unavailable | Fall back to WebSearch |
| View counts unavailable | Estimate from channel size, note as estimated |
