# YouTube Search Strategies

## Building Effective Search Queries

### With specific creators
```
site:youtube.com "[Creator Name]" [topic keywords]
```

### With multiple creators (OR search)
```
site:youtube.com ("[Creator1]" OR "[Creator2]") [topic keywords]
```

### By topic only
```
site:youtube.com [topic] [relevant keywords] [year if date-filtered]
```

### By date range
Append year or date context to queries:
```
site:youtube.com [topic] 2024 OR 2025
```

## Extracting Video Metadata

From each search result or YouTube page, extract:

1. **Video ID** - from URL: `youtube.com/watch?v=[VIDEO_ID]`
2. **Title** - page title or `<title>` tag
3. **Channel** - channel name from page
4. **Published date** - from video metadata
5. **Duration** - from video metadata
6. **Description** - first 200 chars of video description
7. **Thumbnail URL** - `https://img.youtube.com/vi/[VIDEO_ID]/mqdefault.jpg`

## Validation Checklist

Before passing to analysis:
- [ ] All URLs are valid YouTube video URLs (contain `watch?v=` or `youtu.be/`)
- [ ] No duplicate videos
- [ ] Videos fall within requested date range
- [ ] Videos match the requested topic/creator
- [ ] Remove shorts, playlists, and channel pages (only individual videos)

## Fallback: Manual URLs

If search fails or returns insufficient results, ask the user to provide URLs directly. Format expected:
```
https://www.youtube.com/watch?v=XXXXXXXXXXX
https://youtu.be/XXXXXXXXXXX
```
