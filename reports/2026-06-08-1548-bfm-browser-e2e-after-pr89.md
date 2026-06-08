# BFM browser two-witness E2E after PR #89 — operator report

**Verdict:** BFM BROWSER TWO-WITNESS E2E **PASS**

**Completed:** 2026-06-08 (local)  
**Prerequisite:** READY_FOR_DEV_E2E (2026-06-08-1519 check)  
**Worktree:** `clean-current` @ `69c663d7055ffc9853e33e04e825eced1419e6cc`  
**Mode:** FLOW_ONLY_RUN (synthetic provision; no invites)

---

## A) Environment

| Item | Value |
|------|-------|
| Base URL | `http://localhost:3000` |
| Dev DB fingerprint | `ep-wild-cloud-afbfdnz7-pooler…` |
| Production (read-only ref) | commit `69c663d7`, ref `main`, env `production` |
| Synthetic names | Daniel Reed, Elena Petrova, Clara Johnson, Michael Turner |
| Case type | I-130 Bona Fide Marriage (PAIR_PRO / gateway-B) |

---

## B) Process hygiene

| Check | Result |
|-------|--------|
| Stale dev server on :3000 | Stopped (prior PID); fresh restart |
| Single listener on :3000 | **Yes** (one process after restart) |
| Stale Playwright/MCP jobs | **None observed** |
| `git status` | **Clean** on `clean-current` |
| Driver staged | **No** (`scripts/_agent/` gitignored) |

---

## C) Gateway result

| Check | Witness 1 (Clara) | Witness 2 (Michael) |
|-------|-------------------|---------------------|
| Natural SPA → gateway-b1 | yes | yes |
| Hard-nav fallback | **no** | **no** |
| Scope chooser / “1 of 1” | **no** | **no** |
| Gateway-B only | yes | yes |

**Classification:** No stall observed this run.

---

## D) Witness submit results

| Check | Witness 1 | Witness 2 |
|-------|-----------|-----------|
| Preview reached | yes | yes |
| Continue / interlock path | yes | yes |
| Final Review | yes | yes |
| Final confirmation modal | yes | yes |
| prepare-for-signature | attorney-pending (423 expected) | attorney-pending (423 expected) |
| Thank-you attorney-pending | yes | yes |
| DB status SUBMITTED | yes | yes |
| attorney_approval_required | true (on create, no manual SQL) | true |
| signature_preparation | absent | absent |
| §3 relationship heading in preview | yes | yes |

---

## E) Attorney review result

| Check | Result |
|-------|--------|
| Dev attorney login | **ok** |
| `/attorney/tasks` with `includePreviewTestData=true` | **2/2** witnesses visible (Clara + Michael) |
| Review pages load | yes (both requests) |
| Good-faith panel | visible |
| Statement timeline | visible / expanded |
| Structured revision reasons | visible (9 codes; not submitted) |
| Approve / request revision | **not performed** |

Request IDs masked: `0ff6326a…`, `bfab618b…`

---

## F) Document quality

| Check | Clara | Michael |
|-------|-------|---------|
| PERSONAL OBSERVATIONS OF THE RELATIONSHIP | **PASS** | **PASS** |
| OBSERVATIONS OF CHARACTER AND CONDUCT | absent | absent |
| TBOMK | absent | absent |
| no-records boilerplate | absent | absent |
| personal-knowledge limitation | present | present |
| §1746 declaration | present | present |

---

## G) includePreviewTestData handling

Demo/smoke fixture case requires **`includePreviewTestData=true`** on attorney tasks API/UI so both witnesses appear in review queue. This is expected for synthetic demo data, not a product defect. Queue verified **2/2** with hide-by-default behavior noted in driver diagnostics.

---

## H) Remaining gaps

**None.** All pass-matrix checks green (19/19).

---

## I) Artifact paths

| Item | Path |
|------|------|
| Driver report | `C:\Users\lukoe\Downloads\asn-reports\2026-06-08-2227-bfm-browser-two-witness-e2e-final-driver.md` |
| Artifacts dir | `C:\Users\lukoe\Downloads\asn-reports\artifacts\bfm-browser-e2e-final-2026-06-08-2227` |
| This report | `C:\Users\lukoe\Downloads\asn-reports\2026-06-08-1548-bfm-browser-e2e-after-pr89.md` |

Artifacts not staged to git.

---

## J) Safety confirmation

| Constraint | Status |
|------------|--------|
| No production writes | **Confirmed** |
| No production E2E | **Confirmed** |
| No real emails / invites | **Confirmed** |
| No payments / Stripe | **Confirmed** |
| No Dropbox / finalization | **Confirmed** |
| No DocuSign / notary / signature requests | **Confirmed** |
| No provider PDF retrieval | **Confirmed** |
| No manual SQL UPDATE | **Confirmed** |
| No secrets / tokens / full statement text in report | **Confirmed** |
| Driver/artifacts not staged | **Confirmed** |

---

## Pass matrix (summary)

All **PASS:** w1GatewayB, w2GatewayB, noScopeChooser, noScopeModal, w1/w2 Submitted, w1/w2 Preview, attorneyTasksTwo, attorneyReviewLoads, goodFaithPanels, timelinePanels, revisionReasons, relationshipHeadings, noGmc, attorneyApprovalOnCreate, w1/w2 DbSubmitted, attorneyLoginOk.
