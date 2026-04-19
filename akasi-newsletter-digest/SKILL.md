---
name: akasi-newsletter-digest
description: Build a daily-briefing digest from a list of URLs (newsletters, blog posts, articles). Each source gets a 1-line summary, key insight, and why-it-matters. Plus synthesis: top 3 themes, one contrarian take, one action item. Use when the user wants a morning briefing across multiple reads. Trigger phrases: "digest these articles", "morning briefing on", "newsletter digest", "synthesize these links", "daily briefing".
---

# Akasi Newsletter Digest

Ports the MindStudio "Newsletter Digest Agent." Reads N sources, summarizes each, then synthesizes patterns across all of them.

## Inputs
- **URLs** (required) — list of URLs to digest. 2-15 works best.
- **Theme/focus** (optional) — default "general business". Examples: "AI tools for SMBs", "small business marketing", "what should I tell clients this week"

## Output Format

```
# Daily Briefing — <date>
Theme: <focus or "general">

---

## Sources

### 1. <article title>
<URL>
**Summary:** <one sentence>
**Key insight:** <the non-obvious point — one sentence>
**Why it matters:** <one sentence tied to the theme>

### 2. <article title>
<URL>
**Summary:** ...
**Key insight:** ...
**Why it matters:** ...

(repeat for each source)

---

## Synthesis

### Top 3 themes across all sources
1. <theme + which sources cover it>
2. <theme + which sources>
3. <theme + which sources>

### One contrarian take
<the angle most of these sources are missing or getting wrong, in 2-3 sentences>

### One action item
<a concrete thing to do this week, <30 minutes, based on what you just read>
```

## Steps

1. Take the list of URLs. If only one was passed, ask the user if they meant `akasi-tldr` instead.
2. For each URL, use `WebFetch` with a prompt like *"Extract the main article content. Return body text, ignore nav/footer/ads."*
3. If a URL fails (paywall, error, redirect), note it in the output as `<URL> — unable to fetch` and skip the section.
4. For each successfully fetched source: write the title, 1-sentence summary, key insight (the non-obvious takeaway), and why-it-matters (tied to the theme if given).
5. After all sources are summarized, look across them for patterns: what shows up multiple times? What's the consensus? What's the disagreement?
6. Top 3 themes = the most common patterns. Cite which sources cover each.
7. Contrarian take = what these sources are missing, getting wrong, or assuming. Be specific.
8. Action item = one concrete thing to do this week based on what was just read. Not "consider learning more" — something like "test X with Smile4Less" or "draft an email to Y."
9. Format as one clean markdown block. The user should be able to paste this into Obsidian or send as an email.

## Voice
- Direct, no preamble. Each entry is one screen of useful info.
- Quote specific stats, names, and claims from the source.
- The contrarian take should actually be contrarian — not a hedge.
- No banned words ("leverage", "seamlessly", "unlock", "delve", "robust", "in today's landscape").
- No emojis.

## When NOT to use
- If only one URL — use `akasi-tldr`.
- If the user wants a deep research report on a topic — use `akasi-research-report`.
- If the user wants tracked deltas vs yesterday's digest — that's a different skill (this is one-shot synthesis).
