---
name: akasi-watchpage
description: Track changes to a webpage over time. Pass a URL — get a snapshot saved locally, diffed against the previous snapshot, with a summary of what changed and why it matters. Use when the user wants to monitor a competitor page, pricing page, jobs page, docs page, or any page that may shift. Trigger phrases: "watch this page", "track changes to", "what changed on", "diff this page", "monitor this URL".
---

# Akasi Watchpage

Ports the MindStudio "Website Change Tracker" agent. Pulls the page now, compares to the most recent saved snapshot, and reports the delta.

## Inputs
- **URL** (required) — the page to watch
- **What to watch for** (optional) — default "any meaningful content change". Examples: "pricing changes", "new product on the page", "team page additions", "any new blog post"

## Output Format

```
## Page
<URL> — checked <date/time>

## Summary of Changes
<2-3 sentences describing what's different vs the prior snapshot. If first run: "First snapshot saved — no prior version to compare.">

## What Changed
- <bullet 1: specific delta — added/removed/edited text, prices, links>
- <bullet 2>
- <bullet 3>

## Why It Matters
<1-2 sentences tied to the "what to watch for" input — or general business interpretation>

## Recommended Action
<one concrete next step in <30 minutes — "raise our price", "email the prospect", "update battle card", "ignore — cosmetic only">

## Snapshot
Saved to: `~/.claude/skills/akasi-watchpage/snapshots/<slug>-<YYYY-MM-DD>.md`
```

## Steps

1. Slugify the URL → `<host-and-path>` lowercase, hyphens.
2. List existing snapshots in `~/.claude/skills/akasi-watchpage/snapshots/` matching the slug. Identify the most recent one (if any).
3. Use `WebFetch` on the URL with a prompt like *"Return the full visible text content of this page — headings, body, prices, CTAs, footer. Strip nav menus and ads."*
4. If WebFetch fails or hits a paywall, tell the user and stop.
5. Save the new snapshot to `~/.claude/skills/akasi-watchpage/snapshots/<slug>-<YYYY-MM-DD>.md` with a YAML frontmatter (`url`, `fetched_at`, `watch_for`) plus the body content.
6. If a prior snapshot exists, diff the two: identify added lines, removed lines, edited lines. Focus on substantive changes (text, prices, links, CTAs) — ignore whitespace and tracking parameter shuffles.
7. Write the report in the format above. Be specific: quote exact old → new strings.
8. If first run, skip the diff and write "First snapshot saved — no prior version to compare."

## Partial Port Note
This is a **partial port** of the MindStudio app. MindStudio could schedule recurring checks via cron and store snapshots in cloud storage. This skill:
- Stores snapshots in the local skill folder (`snapshots/`) — survives as long as the user keeps the folder
- Runs only when the user invokes it — no automatic schedule
- To make it recurring, pair with the `loop` skill (e.g. `/loop 1d /watchpage <url>`) or the `schedule` skill for true cron

## Voice
- Direct, no preamble. Active voice, contractions.
- Quote the actual changed text — don't paraphrase.
- No banned words ("leverage", "seamlessly", "unlock", "delve", "robust", "in today's landscape").
- No emojis.

## When NOT to use
- If the user wants a one-time read — use `akasi-tldr` instead.
- If the page requires login — WebFetch can't see it. Say so.
- If the user wants real-time alerts on change — this is poll-on-demand only. Use a paid uptime/change service for that.
