# akasi-sheets-analyzer

Pass a Google Sheet URL or ID — get back a structure summary, top 3 patterns, anomalies, and a list of suggested next analyses. Pulls data via Google Workspace MCP. Good for getting a fast read on a spreadsheet someone just dropped in your lap.

## Install

```bash
cp -r akasi-sheets-analyzer ~/.claude/skills/
```

Then restart Claude. Requires the Google Workspace MCP if you want it to fetch sheets directly.

## Trigger

Say one of: "analyze this sheet [URL]", "what's in this spreadsheet", "patterns in this google sheet", "summarize this sheet", "what's weird in this data".

[Back to all skills](../README.md)
