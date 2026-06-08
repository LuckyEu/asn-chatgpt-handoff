# BFM demo screenshot & video asset pack — final report

**Completed:** 2026-06-08 (local)  
**Worktree:** `C:\Users\lukoe\asn-deploy-stabilize`  
**Source E2E:** `2026-06-08-1922-bfm-browser-two-witness-e2e-final-clean.md` (PASS)  
**Synthetic names only:** Daniel Reed, Elena Petrova, Clara Johnson, Michael Turner, Sample Immigration Law PLLC

---

## A) Screenshots captured

**Output folder:** `C:\Users\lukoe\Downloads\asn-reports\artifacts\bfm-demo-screenshots-2026-06-08-2036`

| # | Filename | Status | Notes |
|---|----------|--------|-------|
| A | `01-case-type-selector.png` | **Captured** | Attorney intake; Admin Settings labels (N-400, EOIR-42B, I-129F, I-130, I-751); no raw enum codes |
| B | `02-two-witness-case-workspace.png` | **Captured** | Synthetic couple case; two support statement rows |
| C | `03-gateway-b-relationship-questions.png` | **Captured** | Gateway-B relationship-focused questions |
| D | `04-relationship-heading-preview.png` | **Captured** | Heading **PERSONAL OBSERVATIONS OF THE RELATIONSHIP**; body redacted |
| E | `05-attorney-pending-thank-you.png` | **Captured (composite)** | Attorney-review pending message; marketing-safe mock (post-submit URL not reusable) |
| F | `06-attorney-tasks-two-witnesses.png` | **Captured** | Tasks with `includePreviewTestData=true`; Clara + Michael visible |
| G | `07-good-faith-review-panel.png` | **Captured** | Good-faith review panel on attorney review page |
| H | `08-statement-timeline.png` | **Captured** | Statement timeline expanded |
| I | `09-structured-revision-reasons.png` | **Captured** | Request revision modal/reasons shown; not submitted |

**Total:** 9/9 captured  
**Manifest:** `capture-manifest.json` in screenshot folder

---

## B) Missing screenshots

**None.** All nine required frames captured or safely composited.

**Operator note on 05:** If a live thank-you capture is preferred over the composite, re-run during a fresh witness submit on dev and replace only that PNG.

---

## C) Redaction summary

| Rule | Applied |
|------|---------|
| Tokens / magic links / auth headers | Not visible (no browser chrome; dev-only URLs not exported) |
| Real email addresses | Synthetic only; cropped where possible |
| Internal UUIDs / case IDs | Not shown in final PNGs |
| Full private statement text | Redacted on preview (04); attorney review pages height-capped |
| Blob URLs / PDF bytes / ID images | Not captured |
| Provider / payment / DocuSign / notary screens | Not captured |
| Browser address bar with tokens | Not included in viewport |

---

## D) Video scripts & assembly docs created

**Packet folder:** `docs/_agent/pilot-copy/outreach-public-attorneys/bfm-demo-packet-v1/`

| File | Purpose |
|------|---------|
| `bfm-screenshot-capture-report.md` | Per-screenshot capture notes and redaction log |
| `bfm-video-assembly-plan.md` | 90s + 3min slide order, screenshot mapping, timing, safe stop points |
| `bfm-90-second-teaser-script.md` | Short outreach narration (pilot CTA; no overclaims) |
| `bfm-3-minute-narration-polished.md` | Smoother 3-minute walkthrough (less technical than E2E report) |

**Companion (existing):** `bfm-video-ppt-addendum.md`, main ASN video-PPT package

---

## E) Email CTA draft created

| File | Status |
|------|--------|
| `bfm-email-with-video-cta-v1.md` | **DRAFT** — not send-ready |

**From:** Affidavit Support Network Pilot Team `<pilot@affidavitsupport.net>`  
**Primary CTA:** “Would it be useful if we sent a short video overview?”  
**No** scheduling link as first CTA; **no** “book a call” as primary ask; **no** no-reply sender.

---

## F) Remaining placeholders

| Placeholder | Location | Action before send |
|-------------|----------|-------------------|
| `[PERSONALIZED_FIRST_LINE]` | Email body | Replace per recipient |
| `[VIDEO_LINK]` | Email body | Replace after teaser is hosted |
| `[COMPLIANCE_MAILING_ADDRESS]` | Email footer | Replace with approved mailing address |
| `[ATTORNEY_EMAIL]` | Email header | Replace per recipient |
| Subject line `[pick one above]` | Email | Choose one subject variant |

Video assembly: title/CTA slides and hosted export URL still to be produced by operator.

---

## G) Safety confirmation

| Constraint | Status |
|------------|--------|
| No production writes | **Confirmed** — dev/local only |
| No real emails / invites sent | **Confirmed** |
| No payments / Stripe | **Confirmed** |
| No Dropbox / finalization | **Confirmed** |
| No DocuSign / notary / signature requests | **Confirmed** |
| No provider PDF retrieval | **Confirmed** |
| No real applicant/witness data | **Confirmed** — synthetic names only |
| No secrets in this report | **Confirmed** — no tokens, DB URLs, hashes, or full statement text |
| No git staging of screenshots or pilot-copy | **Confirmed** — artifacts remain local/gitignored |

---

## H) Recommended next action

1. **Operator review** — Visual QA all nine PNGs at 100% zoom; decide whether to re-capture `05` live vs composite.  
2. **Assemble PPT/video** — Follow `bfm-video-assembly-plan.md`; record 90s teaser first.  
3. **Replace `[VIDEO_LINK]`** — Host teaser; update email draft.  
4. **Compliance pass** — Replace mailing address; confirm boundaries language.  
5. **Manual first email** — Single opt-in recipient; do not bulk send until reply pattern is validated.

**Do not mark email send-ready until all placeholders are replaced.**
