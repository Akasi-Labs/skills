# akasi-email-summarizer

Summarize a Gmail thread into sender, subject, top 3 points, action required, and a draft reply. Pulls the thread via Google Workspace MCP if you've got it wired up — otherwise paste the thread text and it works the same.

## Install

```bash
cp -r akasi-email-summarizer ~/.claude/skills/
```

Then restart Claude.

## Trigger

Say one of: "summarize this email", "summarize this thread", "what's this email about", "draft a reply to this thread", "tldr this email".

[Back to all skills](../README.md)
