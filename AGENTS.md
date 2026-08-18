# Akasi Skills instructions

## What this repo is

`skills` is the organization-level reusable skills surface. Preserve compatibility and validate instructions before publishing reusable changes.

## Routing rules

- Thinking, architecture, strategy, and personal notes belong in the iCloud vault; agents write there only in `vault/agent-notes/`.
- System build/configuration belongs in `akasi-systems`; reusable IP belongs here and in `akasi-ip` as applicable.
- Client facts, decisions, configuration, and delivery state stay only in the relevant client repo. Never pool client memory.
- Client deliverables belong in client `/deliverables`, then Google Drive for delivery.

## Durable memory

- Read relevant records at session start. Append durable facts using `YYYY-MM-DD`; never delete, rewrite, or overwrite history.
- State what changed, why, source/evidence, and unresolved follow-up. Keep durable agent state in repos, not the vault except `agent-notes/`.

## Safety

- Never commit, paste, log, or echo secret values, private keys, tokens, credentials, or `.env` contents. Record locations, owners, scopes, and rotation steps only.
- Check `.gitignore` before additions. If a secret is exposed, stop and report rotation is required.
- Do not broaden access, deploy, send, spend, or delete without sign-off.

## Voice and formatting

- Write concise, plain, complete sentences. Lead with outcome; record evidence, assumptions, and concrete blockers.
- Preserve conventions and make focused, reviewable changes.

## Dennis sign-off required

- Repository lifecycle, GitHub permissions, client/cross-client access, vault changes outside `agent-notes/`.
- Production changes, purchases, billing, external messages, deletion, client-data moves, scope changes, or publication.
