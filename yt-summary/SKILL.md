---
name: yt-summary
description: Search YouTube videos by topic, creator, and date range, then analyze them with NotebookLM to generate actionable summaries. Produces both Markdown and HTML output files with prioritized recommendations, specific clips to watch, use cases, and implementation actions. Use when the user wants to research YouTube content, analyze videos from specific channels, summarize recent videos on a topic, or create video content digests. Triggers on requests like "analyze YouTube videos about X", "summarize recent videos from [channel]", "research what [creator] has been saying about [topic]", "YouTube digest", "video analysis report".
---

# YouTube Analyst with NotebookLM

Search YouTube videos, analyze them through NotebookLM, and produce actionable briefings.

## Input/Output Contract

**Input (via arguments or user message):**
- `topic` (required): search terms
- `creators` (optional): specific channels
- `date_range` (optional, default: last 6 months)
- `count` (optional, default: 5)
- `output_path` (optional, default: current directory)
- `format` (optional, default: both md+html)
- `language` (optional, default: auto-detect from videos)
- `--autonomous`: skip all interactive steps, use defaults, run end-to-end

**Output (files generated):**
- `{output_path}/analysis_YYYY_topic_slug.md`
- `{output_path}/analysis_YYYY_topic_slug.html`
- Prints to stdout: JSON summary with file paths, video count, notebook ID

**Calling from another skill/agent:**
1. Invoke `/yt-summary --autonomous topic="Claude Code" count=10 date_range="last week"`
2. Read the generated files from the output path
3. Do post-processing (branding, email, etc.)

## Autonomous Mode

When invoked with `--autonomous` in the arguments (or by another skill/agent), skip ALL interactive steps:
- Do NOT ask for confirmation of video selection
- Do NOT ask clarifying questions — use defaults for anything unspecified
- Execute all steps end-to-end and generate output files
- Only stop if a critical error occurs (auth expired, zero results after broadened search)

When invoked interactively (no `--autonomous`), ask for confirmation at Step 3 and clarify ambiguous inputs.

## Workflow

1. Parse request
2. Search YouTube via Firecrawl
3. Validate, rank by views, select top N
4. Create NotebookLM notebook with video URLs
5. Query NotebookLM for deep analysis
6. Generate action-oriented output files
7. Print summary to stdout

## Step 1: Parse Request

Extract parameters from arguments or user message:

| Parameter | Default | Notes |
|-----------|---------|-------|
| topic | (required) | Main search terms |
| creators | (none) | Specific channels to filter |
| date_range | Last 6 months | Convert relative dates to absolute |
| count | 5 | How many to analyze |
| output_path | cwd | Where to write files |
| format | both | `md`, `html`, or `both` |
| language | auto | Output language matches video language unless specified |

In autonomous mode: use defaults for anything not provided. Do not ask.
In interactive mode: ask only if topic is missing or date range is ambiguous.

## Step 2: Search YouTube

Use Firecrawl or WebSearch to find YouTube videos. See [references/search-strategies.md](references/search-strategies.md) for query patterns.

1. Build query: `site:youtube.com [creator filter] [topic] [date hints]`
2. Execute 2-3 search variants to maximize coverage
3. Collect metadata per result: title, channel, URL, date, description

**View counts are mandatory.** Use the Return YouTube Dislike API (`returnyoutubedislikeapi.com/votes?videoId=VIDEO_ID`) or scrape from YouTube pages. View count is the primary ranking signal.

Fallback chain: Firecrawl -> WebSearch -> ask user for manual URLs (interactive only).

## Step 3: Validate and Select

1. Filter: only YouTube video pages (no playlists, shorts, channels)
2. Deduplicate by video ID
3. Filter by date range
4. Rank by view count (descending)
5. Select top N videos

In autonomous mode: proceed directly with top N. No confirmation.
In interactive mode: show ranked list, ask for confirmation or edits.

If fewer than N results after filtering:
- Autonomous: proceed with what's available, note in output
- Interactive: offer to broaden search or accept manual URLs

## Step 4: NotebookLM Analysis (REQUIRED)

Always create a NotebookLM notebook and add video URLs as sources. Execute sequentially without pausing:

```bash
# 1. Create notebook
notebooklm create "[Topic] - YouTube Analysis [date]" --json
# Parse notebook ID from output

# 2. Add each video URL
for each URL:
  notebooklm source add "URL" --json
  # Log success/failure, continue on individual failures

# 3. Wait for processing
# Check source list --json until all status=ready (poll every 10s, timeout 120s)

# 4. Query for actionable analysis
notebooklm ask "For each video source: (1) 3 most actionable takeaways, (2) specific timestamps of most valuable segments, (3) concrete tools/commands/configs demonstrated. Be specific." --json

# 5. Cross-video analysis
notebooklm ask "Across ALL videos: (1) Top 5 things to implement in priority order, (2) Which videos contradict each other, (3) Common mistakes/warnings mentioned in multiple videos. Cite specific videos." --json
```

**Error handling (no interactive prompts):**
- Auth expired: print error JSON `{"error": "notebooklm_auth_expired", "action": "run notebooklm login"}`, stop
- Source add fails: log warning, continue with remaining sources
- All sources fail: fall back to Claude direct analysis using scraped metadata
- Query fails: retry once after 10s, then fall back to Claude analysis

## Step 5: Generate Output

Generate files at `{output_path}`. See [references/output-templates.md](references/output-templates.md) for format guidance.

**Required sections (in order):**
1. **TL;DR** - Top 3-5 actions based on video content
2. **Ranking table** ordered by views, with priority badges and clips to watch
3. **What to implement** - action cards with "why" (quote from transcript), "what to do", "reference video"
4. **Debates/contradictions** detected by NotebookLM
5. **Warnings** mentioned across multiple videos
6. **Use cases** grouped by domain
7. **What to skip** - save the reader's time
8. **Context** - releases, news mentioned in videos

**Key principles:**
- View count displayed for every video
- Priority badges: Imprescindible / Alta / Media / Baja / Skip
- Specific clips to watch, not "watch the whole thing"
- Transcript quotes from NotebookLM where available
- "Skip" recommendations as valuable as "watch" recommendations

**File naming:** `analysis_[YYYY]_[topic_slug].md` and `analysis_[YYYY]_[topic_slug].html`
Topic slug: lowercase, spaces to underscores, max 30 chars.

## Step 6: Output Summary

Print to stdout (parseable by calling agents):

```json
{
  "status": "success",
  "videos_analyzed": 8,
  "videos_failed": 2,
  "notebook_id": "uuid-here",
  "files": [
    "analysis_2026_claude_code.md",
    "analysis_2026_claude_code.html"
  ],
  "tldr": ["Action 1", "Action 2", "Action 3"]
}
```

In interactive mode, also show the TL;DR inline and file paths.

## Error Handling

All errors produce structured output. No interactive prompts in autonomous mode.

| Scenario | Action |
|----------|--------|
| Search returns 0 results | Broaden query (remove date filter, simplify terms). If still 0: `{"error": "no_results"}` |
| Some videos inaccessible | Continue with accessible ones. Note in output. |
| NotebookLM auth expired | `{"error": "notebooklm_auth_expired", "action": "run notebooklm login"}` |
| NotebookLM unavailable | Fall back to Claude analysis. Note `"notebooklm": false` in output. |
| Firecrawl unavailable | Fall back to WebSearch |
| View counts unavailable | Estimate from channel size. Note `"views_estimated": true` in output. |
