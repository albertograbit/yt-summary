# yt-summary

Claude Code skill that searches YouTube videos by topic, creator, and date range, then analyzes them with NotebookLLM to generate structured summaries.

## Features

- Search YouTube videos with flexible filters (topic, creator, date range)
- Analyze video content via NotebookLLM (with Claude fallback)
- Generate Markdown and HTML reports with:
  - Executive summary
  - Individual video analysis
  - Cross-video trends and patterns
  - Key insights

## Installation

Copy the `yt-summary/` directory to your Claude Code skills folder:

```bash
# Personal (all projects)
cp -r yt-summary/ ~/.claude/skills/yt-summary/

# Project-specific
cp -r yt-summary/ .claude/skills/yt-summary/
```

## Usage

In Claude Code, the skill triggers automatically on requests like:

- "Analyze YouTube videos about AI in education"
- "Summarize recent videos from Dot CSV"
- "Research what Midudev has been saying about React"
- `/yt-summary`

## Dependencies

- **Firecrawl** plugin (for YouTube search) or WebSearch fallback
- **NotebookLLM** skill (optional, for deep analysis) or Claude direct analysis fallback

## License

MIT
