# Output Templates

## Markdown Template

Use this template for the `.md` output file. Name: `analysis_[YYYY]_[topic_slug].md`

```markdown
# YouTube Analysis: [TOPIC]
**Period:** [start_date] - [end_date]
**Videos analyzed:** [count]
**Generated:** [current_date]

---

## Videos Analyzed
| # | Title | Channel | Date | Duration | URL |
|---|-------|---------|------|----------|-----|
| 1 | ...   | ...     | ...  | ...      | ... |

---

## Executive Summary
[2-3 paragraph overview synthesizing all videos]

---

## Individual Analysis

### Video 1: [Title]
- **Channel:** [name]
- **Date:** [date]
- **Duration:** [duration]
- **URL:** [url]

**Summary:**
[3-5 sentence summary]

**Key Points:**
- Point 1
- Point 2
- Point 3

**Takeaways:**
- Takeaway 1

---

[Repeat for each video]

---

## Trends and Patterns
[Cross-video comparative analysis: common themes, divergent opinions, evolution of ideas]

## Key Insights
[Most important conclusions from the analysis as a whole]

## Recurring Topics
[Tags or themes that appear frequently across videos]
```

## HTML Template

Generate a self-contained HTML file named `analysis_[YYYY]_[topic_slug].html` with these requirements:

- All CSS inline (single portable file)
- Responsive design (mobile-friendly)
- Clean, modern aesthetic with a dark/light balanced palette
- Navigation sidebar or sticky header for sections
- Card layout for each video (include YouTube thumbnail via `https://img.youtube.com/vi/[VIDEO_ID]/mqdefault.jpg`)
- Collapsible sections for individual video analysis
- Visual summary section with key stats (video count, channels, date range)
- Smooth scroll between sections
- Print-friendly styles

Color scheme suggestion: dark header (#1a1a2e), accent (#e94560), cards on light background (#f5f5f5).

Use semantic HTML5 elements. No external dependencies (no CDN links).
