---
name: akasi-sheets-analyzer
description: Analyze a Google Sheet — structure summary, top 3 patterns, anomalies, and suggested next analyses. Pass a Sheet URL or ID. Uses Google Workspace MCP. Use when the user wants a fast read on a spreadsheet without opening it. Trigger phrases: "analyze this sheet", "what's in this spreadsheet", "patterns in this google sheet", "summarize this sheet", "what's weird in this data".
---

# Akasi Sheets Analyzer

Ports the MindStudio "Google Sheets Analyzer" agent. Pulls metadata and a data range, then surfaces structure, patterns, and outliers.

## Inputs
- **Sheet URL or ID** (required) — full Google Sheets URL or just the ID
- **What to analyze** (optional) — default "structure + patterns + anomalies". Examples: "find duplicate emails", "show me revenue by month", "any rows missing data"
- **Account** (optional) — `akasi` (default, dennis@akasilabs.com) or `personal` (dlovelace777@gmail.com)

## Output Format

```
## Sheet
<title> — <tab name> — <rows> rows × <cols> columns
URL: <url>

## Data Summary
<2 sentences describing what this sheet is — based on column names + sample rows>

**Columns:** <comma-separated list with type guess in parens, e.g. "name (text), revenue (number), signup_date (date)">

## Top 3 Patterns / Insights
1. <specific pattern with numbers — "73% of rows are from California">
2. <specific pattern>
3. <specific pattern>

## Anomalies / Outliers
- <specific row(s) or value(s) that don't fit — quote the data>
- <missing data, duplicates, format inconsistencies>
- <out-of-range values>

## Suggested Next Analyses
1. <specific next question this data could answer>
2. <specific>
3. <specific>
```

## Steps

1. Extract the sheet ID from the URL if a URL was passed.
2. Pick the MCP server: default `mcp__google-workspace-akasi__sheets_getMetadata` and `sheets_getRange`. Switch to `google-workspace-personal` if the user said "personal".
3. Call `sheets_getMetadata` to get sheet title, tab names, and dimensions.
4. Call `sheets_getRange` to pull the data. Default range: first tab, A1 to last column × first 500 rows. If the sheet is small, pull all rows.
5. If the call fails (auth, permissions, missing sheet), say so and stop.
6. Read the data. Identify column types from the header row + sample values.
7. Look for patterns: distributions, ratios, time-based trends, top-N values, common categories.
8. Look for anomalies: blanks, duplicates, format mismatches (e.g. dates as strings), out-of-range numbers, suspiciously round numbers.
9. Suggest 3 next analyses that would be useful given what you saw.
10. Be specific. Quote actual values. Don't say "some rows have issues" — say "rows 47, 102, and 305 have blank email fields."

## Voice
- Direct, no preamble. Reference real columns and real values.
- Don't add caveats unless they matter. If you sampled only the first 500 rows, say so.
- No banned words ("leverage", "seamlessly", "unlock", "delve", "robust", "in today's landscape").
- No emojis.

## When NOT to use
- If the user wants to edit or update the sheet — different tools needed.
- If the sheet is huge (>10k rows) and the user wants full stats — pull to a real analysis tool, not this.
- If the sheet is private and the MCP account doesn't have access — say so plainly.
