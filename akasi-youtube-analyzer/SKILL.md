---
name: akasi-youtube-analyzer
description: Analyze a YouTube video — transcript summary, audience sentiment from comments, most-asked question, and the content gap the audience wants filled. Pass a YouTube URL. Use for competitive research, content ideation, or evaluating a creator. Trigger phrases: "analyze this youtube video", "what's the audience saying about", "youtube comments analysis", "summarize this video", "content gap on this video".
---

# Akasi YouTube Analyzer

Ports the MindStudio "YouTube Video + Comments Analyzer" agent. Pulls transcript and top comments, then surfaces what's working and what's missing.

## Inputs
- **YouTube URL** (required) — full URL or short youtu.be link
- **Focus** (optional) — default "general". Other useful values: "what should I make next", "is this creator worth following", "what does this niche care about"

## Output Format

```
## Video
<title> — <channel> — <duration if known>
URL: <url>

## Video Summary (5 bullets)
- <bullet 1: the core thesis or hook>
- <bullet 2: main supporting point>
- <bullet 3>
- <bullet 4>
- <bullet 5: the close / CTA / outcome>

## Audience Sentiment
**Overall:** <positive / mixed / negative / divided> — <one sentence why>

**Three specific themes:**
1. <theme + 1-2 paraphrased examples>
2. <theme + 1-2 paraphrased examples>
3. <theme + 1-2 paraphrased examples>

## Most-Asked Question in Comments
<the actual question, paraphrased — plus how often it appears or who's asking>

## Content Gap
<what's missing from this video that the audience clearly wants. One sentence on what it is, one sentence on why it matters.>
```

## Steps

1. Use `WebFetch` on the YouTube URL with a prompt like *"Return the video title, channel name, description, and any visible transcript or captions. Also return the top visible comments."*
2. If WebFetch returns the transcript, summarize it into 5 specific bullets — names, numbers, examples, not generic phrases.
3. If WebFetch can't get the transcript (YouTube often blocks), say: *"Transcript not retrievable via WebFetch. Paste the transcript or video description and I'll continue."* — and stop until the user provides it.
4. Comments: WebFetch usually returns only the top few. Work with what you get. If comments are blocked entirely, say: *"Comments not accessible — analyzing video only. For full sentiment analysis, paste the top 20 comments."*
5. Identify themes by looking for patterns: shared complaints, shared praise, shared follow-up questions.
6. Most-asked question = the question (or close variant) that appears the most. If only 1-2 comments are visible, surface the top question among them and note the limitation.
7. Content gap = what the comments are asking for that the video didn't cover. Be specific.

## Voice
- Direct, no preamble. Quote real comment language when useful.
- Don't editorialize the creator — analyze the audience.
- No banned words ("leverage", "seamlessly", "unlock", "delve", "robust", "in today's landscape").
- No emojis.

## When NOT to use
- If the user wants a transcript only — paste the URL into a transcript tool, this skill is for analysis.
- If the video is private or members-only — WebFetch can't see it.
- If the user wants competitive analysis across many videos — run this multiple times then synthesize.
