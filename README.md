# ASN ChatGPT Handoff

This repository is a **public, redacted handoff channel** for coordinating work between **Cursor** (product development) and **ChatGPT** (planning, review, and operator-facing summaries).

It is **not** the ASN product repository. Product source code, migrations, tests, and deploy artifacts belong in [LuckyEu/affidavit-support-network](https://github.com/LuckyEu/affidavit-support-network), not here.

## What belongs here

Only **sanitized Markdown and JSON** handoff files:

- Redacted status reports
- Scope summaries and next-step plans
- Instruction placeholders for operator-reviewed workflows

## What must never be committed here

- Secrets, API keys, cookies, auth headers, or `DATABASE_URL`
- Full flow tokens, magic links, or raw intake tokens
- Full provider IDs (Dropbox, Stripe, DocuSign, notary, etc.)
- Blob URLs, PDF bytes, or raw image/screenshot payloads
- Raw logs, raw script stdout, or full metadata dumps
- PII beyond minimal redacted labels (e.g. masked emails)

When in doubt, **redact or omit**.

## File conventions

| Path | Direction | Purpose |
|------|-----------|---------|
| `latest.md` | Cursor → ChatGPT | Most recent redacted handoff report for ChatGPT context |
| `instructions/latest-from-chatgpt.md` | ChatGPT → Cursor | Latest instructions for Cursor — **operator confirmation required** before execution |
| `manifest.json` | Metadata | Pointers and handoff version |
| `reports/` | Archive | Dated redacted reports (optional) |
| `archive/` | Archive | Retired handoff snapshots (optional) |

## Execution policy

- **`latest.md`** is safe to share with ChatGPT for planning context (redacted only).
- **`instructions/latest-from-chatgpt.md`** must **not** be executed automatically by Cursor. The operator must review and explicitly approve any actions derived from it.

## Related repos

- **Product:** https://github.com/LuckyEu/affidavit-support-network
- **Handoff (this repo):** https://github.com/LuckyEu/asn-chatgpt-handoff
