---
name: akasi-sales-collateral
description: Generate a full sales collateral pack for a specific prospect — one-pager outline, cold email, discovery-call talk track, and objection handling card. Use when prepping for a pitch, demo, or first meeting with a new lead. Trigger phrases: "build sales collateral for", "prep me for [prospect]", "write a one-pager for", "make a pitch pack", "discovery call prep".
---

# Akasi Sales Collateral Generator

Ports the MindStudio "Sales Collateral Generator" agent. Turns 5 inputs into a four-piece pack ready to use in a single sales motion.

## Inputs
- **Prospect name** (required) — person you're selling to
- **Prospect company** (required) — their org
- **What they sell** (required) — their product/service in one line
- **Your product/service** (required) — what you're pitching
- **Key benefit** (required) — the one outcome they get if they buy

## Output Format

```
## Prospect
<Name> — <Company> — sells <what they sell>

---

## 1. One-Pager Outline
**Headline:** <8-12 words tying their pain to your benefit>
**Subhead:** <one sentence — who it's for, what it does, what changes>

**Sections:**
- Problem: <2 sentences naming the specific pain in their world>
- Solution: <2 sentences — what you do, in plain English>
- How it works: <3 bullets, no jargon>
- Proof: <stat / quote / case slot — leave placeholder if no proof>
- Pricing or next step: <one line>
- CTA: <one specific action — "book a 20-min call", "see the demo">

---

## 2. Cold Email Draft
Subject: <6-9 words, no clickbait, references something specific>

<3-5 sentence body. Sentence 1 = why I'm reaching out (specific to them). Sentence 2 = the pain. Sentence 3 = the outcome. Sentence 4 = the ask (15-min call, one Q, etc.). No greeting. No signature.>

---

## 3. Discovery Call Talk Track — 5 Questions
1. <Open question to surface current state>
2. <Question that quantifies the pain>
3. <Question about what they've already tried>
4. <Question about decision process / who else is involved>
5. <Question about what success looks like in 90 days>

---

## 4. Objection Handling Card
**Objection 1:** <most common — usually price or "we have something">
- Response: <2-3 sentences, acknowledge then reframe>

**Objection 2:** <second most likely — usually time/bandwidth>
- Response: <2-3 sentences>

**Objection 3:** <third — usually "send me more info">
- Response: <2-3 sentences that move toward a meeting, not a deck>
```

## Steps

1. Read all 5 inputs. If any are missing, ask for them and stop.
2. Think about the prospect's actual day. What does this person Google at 9am? What gets their boss off their back? Anchor the pain there.
3. Write each section in the format above. Be specific to the prospect — generic copy loses.
4. Cold email: lead with something specific about THEIR business (not yours). The ask is small.
5. Discovery questions: open, not leading. Get them talking about their world before you talk about yours.
6. Objections: predict the real ones based on the prospect/product combo, not generic SaaS objections.
7. Output as one markdown block, copy-pasteable.

## Voice
- Direct, warm, contractions. Talk like a human who's done this before.
- No preamble, no fluff, no "exciting opportunity to partner."
- No banned words ("leverage", "seamlessly", "unlock", "delve", "robust", "in today's landscape", "game-changer", "synergy").
- No emojis.

## When NOT to use
- If the user just needs a cold email and nothing else — use the `cold-email` skill.
- If the user wants a full pitch deck — use `sales-enablement`.
- If the user doesn't have a specific prospect yet — they need positioning work, not collateral.
