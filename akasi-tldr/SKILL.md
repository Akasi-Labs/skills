---
name: akasi-tldr
description: Generate a TL;DR summary of any web article or page. Pass a URL — get back a 3-sentence executive summary, 5 key takeaways, and one action you could take based on the content. Use when the user wants a fast read on an article, blog post, news story, or research piece. Trigger phrases: "tldr", "summarize this article", "what's this page about", "give me the gist of [URL]".
---

# Akasi TL;DR

Ports the MindStudio "TL;DR Browser Extension" agent to a Claude skill. Same job, runs on the user's own Claude subscription — no per-run cost.

## Inputs
- **URL** (required) — the page to summarize
- **Audience** (optional) — default "busy entrepreneur". Other useful values: "investor", "skeptic", "expert in the field"

## Output Format

```
## TL;DR
<3 sentences. First sentence = the core claim. Second = the supporting evidence/why. Third = the so-what.>

## 5 Key Takeaways
1. <one sentence each, specific not generic>
2. ...
3. ...
4. ...
5. ...

## One Action To Take
<one concrete next step the reader could take in <30 minutes based on what they just learned>
```

## Steps

1. Use the `WebFetch` tool to load the URL. Pass a prompt like *"Extract the main article content. Return the full body text, ignoring nav/footer/ads."*
2. Read the returned content. If WebFetch returned an error or paywall, tell the user and stop.
3. Write the summary in the exact format above.
4. Be specific: use names, numbers, and direct quotes when possible. Strip filler.
5. The "One Action" must be concrete — "set up X tool", "email Y person", "test Z hypothesis" — not "consider learning more about it".

## Voice
- Direct, no preamble.
- Active voice, contractions.
- No banned words ("leverage", "seamlessly", "unlock", "delve", "in today's landscape").
- No emojis.

## When NOT to use
- If the user wants a full deep read or annotated breakdown, use a different approach
- If the URL is behind a login wall, this skill can't see it — say so plainly
