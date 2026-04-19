---
name: akasi-research-report
description: Generate a structured research report on any question. Pulls multiple web sources, synthesizes findings, surfaces counter-evidence, and assigns a confidence level. Optional depth — quick (3 sources), standard (5), deep (10+). Use when the user has a real question that needs grounded answers, not vibes. Trigger phrases: "research", "build a research report on", "what does the evidence say about", "research deep dive", "look into".
---

# Akasi Research Report

Ports the MindStudio "Research Report Generator." Multi-source synthesis with explicit counter-evidence and confidence rating — closer to a memo than a Google search summary.

## Inputs
- **Research question** (required) — the actual question, not just a topic. "Should small dental practices use Instagram ads in 2026?" beats "Instagram ads."
- **Depth** (optional) — `quick` (3 sources), `standard` (5, default), `deep` (10+)

## Output Format

```
# Research Report: <question>
Depth: <quick/standard/deep> — <N> sources — <date>

## Executive Summary
<3 sentences. Sentence 1 = the answer. Sentence 2 = the strongest reason. Sentence 3 = the most important caveat.>

## Key Findings
1. <finding> — <source 1>
2. <finding> — <source 2>
3. <finding> — <source 3>
4. <finding> — <source 4>
5. <finding> — <source 5>

## Counter-Evidence
<what argues against the conclusion. 2-4 sentences. Cite the dissenting sources by number.>

## Confidence Level
**<low / medium / high>** — <one sentence on why. What would raise the confidence?>

## Open Questions
- <what's still unknown>
- <what would change the answer if we knew it>
- <what to research next>

## Sources
1. <title> — <URL>
2. <title> — <URL>
...
```

## Steps

1. Read the research question. If it's a topic not a question, ask the user to sharpen it and stop.
2. Set source target by depth:
   - `quick` → 3 sources
   - `standard` → 5 sources (default)
   - `deep` → 10+ sources
3. Use `WebSearch` to find relevant sources. Prioritize: primary sources, recognized publications, authoritative blogs, recent (last 18 months unless evergreen). Skip SEO-spam content farms.
4. Use `WebFetch` on each promising URL with a prompt like *"Extract the main content. Return body text relevant to: <question>"*
5. If a fetch fails, log it but don't substitute fluff — try another source or note the gap.
6. As you read, take notes: what's the claim, what's the evidence, who's saying it.
7. Write the executive summary first — the answer in 3 sentences. Then back it up.
8. Key findings: each one tied to a numbered source. No claims without citation.
9. Counter-evidence is mandatory. If you can't find any, say "no significant counter-evidence found in <N> sources" — and lower the confidence accordingly.
10. Confidence level:
    - **High** = multiple independent sources agree, primary data exists, counter-evidence is weak
    - **Medium** = sources agree but data is thin, or sources disagree but lean one direction
    - **Low** = sources disagree, data is missing, or the question is too new to have evidence
11. Open questions = what would change the answer if we knew it. Specific.
12. Cite every source in the Sources block at the end, numbered to match in-text references.

## Voice
- Direct. No "it depends" hedges unless genuinely warranted.
- Distinguish opinion from evidence. "Source 3 argues" vs "Source 3 found."
- The counter-evidence section is the most important — don't skip it to make the answer look stronger.
- No banned words ("leverage", "seamlessly", "unlock", "delve", "robust", "in today's landscape").
- No emojis.

## When NOT to use
- If the user wants a quick read on one article — use `akasi-tldr`.
- If the user wants synthesis across a list they already gathered — use `akasi-newsletter-digest`.
- If the question requires real-time data (stock price, live event) — WebFetch may be too stale; flag it.
