---
name: akasi-people-extractor
description: Extract every person mentioned in a text source (article URL, pasted text, or email thread) into a structured list — name, role, company, context, source. Optionally append rows to a Google Sheet CRM. Use when the user wants to harvest contacts from research. Trigger phrases: "extract people from", "who's mentioned in", "pull contacts from this article", "build a contact list from", "people extractor".
---

# Akasi People Extractor

Ports the MindStudio "Person Extractor → Sheet CRM" agent. Reads a source, finds every person, and either prints a markdown table or appends to a Google Sheet.

## Inputs
- **Source** (required) — one of:
  - URL (article, blog, press release, news story)
  - Pasted text
  - Gmail thread ID or URL
- **Target sheet URL** (optional) — Google Sheet to append rows to. If omitted, output as a markdown table.
- **Account** (optional, only if writing to sheet) — `akasi` (default) or `personal`

## Output Format

If no target sheet:

```
## Source
<URL or "Pasted text" or "Gmail thread <id>">
Extracted: <date/time> — <N> people found

| Name | Role | Company | Context | Source |
|------|------|---------|---------|--------|
| <name> | <role/title> | <company> | <what they did or said in 1 sentence> | <URL or "thread"> |
| ... |
```

If target sheet provided:

```
## Source
<URL / pasted text / thread>

## Extracted
<N> people found. Appended to sheet: <sheet URL>

(Markdown table preview of rows added — same columns as above)
```

## Steps

1. Get the source text:
   - If URL → use `WebFetch` to pull the body
   - If pasted text → use it directly
   - If Gmail reference → use `mcp__google-workspace-akasi__gmail_get` (or personal variant)
2. Read the text carefully. Find every named person — first + last name preferred, but include single names if that's all that's given.
3. For each person, extract:
   - **Name** — exact spelling from source
   - **Role** — title or role described in the source ("CEO", "researcher", "VP Eng"). If unstated, write "unspecified".
   - **Company** — org they're associated with in this source. If unstated, "unspecified".
   - **Context** — one sentence on what they did, said, or why they're mentioned. Quote when useful.
   - **Source** — the URL or "Gmail thread <id>" or "pasted text"
4. De-duplicate. If the same person is mentioned multiple times, merge into one row with combined context.
5. Skip generic mentions ("a spokesperson said") with no name.
6. If no target sheet → output the markdown table.
7. If target sheet provided:
   - Extract sheet ID from URL
   - Use `mcp__google-workspace-akasi__sheets_getMetadata` to confirm the sheet exists and check the header row
   - If headers don't match (Name / Role / Company / Context / Source), tell the user and ask whether to proceed anyway or adjust column mapping
   - Use the appropriate sheets append tool to add the rows. If no append tool is available in the MCP, output the rows as a tab-separated block the user can paste in.
8. Don't fabricate. If a person's role or company isn't in the source, write "unspecified" — never guess.

## Voice
- Direct, no preamble. The output is data, not prose.
- Use exact name spellings from the source.
- No banned words in any contextual writing.
- No emojis.

## When NOT to use
- If the user wants enrichment (LinkedIn lookup, email finding) — this skill only extracts what's in the source. Pair with a separate enrichment tool.
- If the source is huge and the user wants only key people — ask for the filter criteria first.
- If the sheet is in a different account than the configured MCP — say so and stop.
