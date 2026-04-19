# Akasi Skills

Free Claude skills ported from MindStudio bootcamp builds. Each one is a real workflow I've used in client work or my own ops, packaged as a Claude skill you can install in 60 seconds and run on your own Claude subscription.

No per-run cost. No platform lock-in. Just markdown.

## Install

**Option A — clone everything:**

```bash
git clone https://github.com/akasi-labs/skills.git ~/akasi-skills && \
  cp -r ~/akasi-skills/akasi-* ~/.claude/skills/
```

Restart Claude. All 9 skills load on next session.

**Option B — grab one skill:**

```bash
git clone --depth 1 https://github.com/akasi-labs/skills.git /tmp/akasi && \
  cp -r /tmp/akasi/akasi-tldr ~/.claude/skills/
```

Swap `akasi-tldr` for whichever skill you want.

**Option C — download from the GitHub web UI:**

Click into any skill folder above, hit the download button on `SKILL.md`, drop it into `~/.claude/skills/<skill-name>/SKILL.md`.

## Skills

| Skill | What it does |
|---|---|
| [akasi-tldr](./akasi-tldr) | TL;DR any web article — 3-sentence summary, 5 takeaways, one action you could take. |
| [akasi-email-summarizer](./akasi-email-summarizer) | Summarize a Gmail thread — sender, subject, top 3 points, action required, draft reply. |
| [akasi-watchpage](./akasi-watchpage) | Track changes to a webpage over time. Snapshots locally, diffs against the prior snapshot. |
| [akasi-sales-collateral](./akasi-sales-collateral) | Generate a full sales pack for a prospect — one-pager, cold email, talk track, objection card. |
| [akasi-youtube-analyzer](./akasi-youtube-analyzer) | Analyze a YouTube video — transcript summary, audience sentiment, content gap. |
| [akasi-sheets-analyzer](./akasi-sheets-analyzer) | Analyze a Google Sheet — structure, top patterns, anomalies, suggested next analyses. |
| [akasi-people-extractor](./akasi-people-extractor) | Extract every person from a text source into a structured list. Optional: append to a Google Sheet CRM. |
| [akasi-newsletter-digest](./akasi-newsletter-digest) | Daily-briefing digest from a list of URLs — per-source summary plus cross-source synthesis. |
| [akasi-research-report](./akasi-research-report) | Structured research report on any question — multiple sources, counter-evidence, confidence rating. |

## The series

I'm porting MindStudio bootcamp builds to free Claude skills. New skill drops every Mon/Wed/Fri.

## License

[MIT](./LICENSE) — use them, fork them, ship them in client work.

---

Built by [Dennis Lovelace](https://akasilabs.com) at Akasi Labs.
