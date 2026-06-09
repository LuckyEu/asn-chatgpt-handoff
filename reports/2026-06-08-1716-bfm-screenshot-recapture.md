# BFM demo screenshot re-capture — final report

**Verdict:** BFM SCREENSHOT QA **PASS** — final video export **unblocked**

**Completed:** 2026-06-08 (local)  
**Output folder:** `C:\Users\lukoe\Downloads\asn-reports\artifacts\bfm-demo-screenshots-recapture-2026-06-09-0010`  
**Source E2E:** post-E2E PASS synthetic BFM two-witness fixture  
**QA rerun:** `2026-06-08-1716-bfm-screenshot-visual-qa-rerun.md`

---

## A) Screens re-captured

| File | Method |
|------|--------|
| `01-case-type-selector.png` | Intake label panel + dropdown composite (all five case types) |
| `02-two-witness-case-workspace.png` | Attorney case workspace with route-mocked case GET (two witness rows) |
| `04-relationship-heading-preview.png` | Witness preview with redacted relationship heading body |
| `06-attorney-tasks-two-witnesses.png` | Attorney tasks with Include Preview/test enabled |
| `08-statement-timeline.png` | Attorney review timeline panel (Clara Johnson; two events) |

---

## B) Screens reused (prior QA OK)

| File | Source |
|------|--------|
| `03-gateway-b-relationship-questions.png` | `bfm-demo-screenshots-2026-06-08-2036` |
| `05-attorney-pending-thank-you.png` | `bfm-demo-screenshots-2026-06-08-2036` |
| `07-good-faith-review-panel.png` | `bfm-demo-screenshots-2026-06-08-2036` |
| `09-structured-revision-reasons.png` | `bfm-demo-screenshots-2026-06-08-2036` |

---

## C) QA verdict

**BFM SCREENSHOT QA PASS** — all nine PNGs pass visual/privacy checks for attorney-facing BFM video assembly.

---

## D) Remaining blockers

None for screenshot QA or video-PPT asset assembly.

**Follow-up (product, not screenshot-blocking):** dev `GET /api/cases/{id}` recommender query fails when legacy columns (`notary_status`, `reactivation_count`, etc.) are absent; recommend fixing legacy SELECT fallbacks so attorney case workspace loads without capture mocks.

---

## E) Safety confirmation

- Dev/local only; no production writes or env changes
- No emails, invites, Stripe, DocuSign, notary, Dropbox, or publish actions
- Synthetic fixture names only in captures
- No tokens, magic links, DATABASE_URL, blob URLs, or full private statement text in PNGs or this report
- Screenshots and scripts not staged to git

---

## F) Asset map

Updated: `C:\Users\lukoe\Downloads\asn-reports\artifacts\bfm-video-v1-2026-06-08-1620\asset-map.json`  
(`finalExportBlocked: false`, folder pointer → recapture output)
