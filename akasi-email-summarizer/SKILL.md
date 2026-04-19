---
name: akasi-email-summarizer
description: Summarize a Gmail thread into sender, subject, top 3 points, action required, and a draft reply. Pulls the thread via Google Workspace MCP or accepts pasted thread text. Use when the user wants a fast read on an email conversation. Trigger phrases: "summarize this email", "summarize this thread", "what's this email about", "draft a reply to this thread", "tldr this email".
---

# Akasi Email Summarizer

Ports the MindStudio "Email Thread Summarizer" agent. Cuts long Gmail threads into a one-screen brief plus a draft reply when action is needed.

## Inputs
- **Thread reference** (required) — one of:
  - Gmail thread ID or message ID
  - Gmail web URL containing the thread ID
  - Pasted thread text (raw email copy/paste)
- **Account** (optional) — `akasi` (default, dennis@akasilabs.com) or `personal` (dlovelace777@gmail.com)
- **Reply tone** (optional) — default "warm direct". Other useful values: "polite decline", "buying time", "ship it"

## Output Format

```
## Sender
<name + email + role/company if known>

## Subject
<exact subject line>

## Top 3 Points
1. <one sentence — the most important fact, decision, or ask>
2. <one sentence>
3. <one sentence>

## Action Required
**Yes/No** — <if yes, the specific thing the recipient needs to do and by when>

## Suggested Reply
<only if action required. 3-6 sentences in Dennis's voice. No greeting block, no signature — Gmail auto-applies "Sincerely, Dennis".>
```

## Steps

1. Determine which Google Workspace MCP server to use:
   - Default → `mcp__google-workspace-akasi__gmail_get`
   - If the user said "personal" → `mcp__google-workspace-personal__gmail_get`
2. If the input is a thread ID or URL, call `gmail_get` with that ID. If pasted text, skip the MCP call.
3. If neither MCP nor pasted text works, ask the user to paste the thread and stop.
4. Read the full thread top-to-bottom. Identify the most recent message and any open question or ask directed at the recipient.
5. Fill in Sender / Subject / Top 3 Points using exact names, dates, dollar amounts, and quoted snippets where useful.
6. Decide Action Required:
   - **Yes** if the latest message asks a question, requests a decision, sets a deadline, or proposes a meeting time
   - **No** if it's an FYI, a thank-you, or a confirmation
7. If Yes, write the suggested reply in Dennis's voice. Direct, warm, contractions. No preamble. No "Hope this finds you well."
8. Do NOT send the email. Drafts only — Dennis approves and sends.

## Voice
- Direct, no preamble. Active voice, contractions.
- Reply drafts: warm but efficient. Get to the point in sentence one.
- No banned words ("leverage", "seamlessly", "unlock", "delve", "robust", "in today's landscape", "game-changer").
- No emojis.

## When NOT to use
- If Dennis just wants to read the thread, not summarize it — open Gmail.
- If the thread is fewer than 3 messages and obviously trivial — skip the formal output, just reply directly.
- If the thread contains attachments that are the actual subject (e.g. "review this PDF") — read the attachment first.
