# Joe Average — applicant declaration PDF email diagnosis

**Verdict:** APPLICANT_PDF_BEFORE_APPROVAL_BUG  
**Date:** 2026-06-12-0932  
**Mode:** Read-only production diagnosis (no writes, no emails, no approval)

---

## A) Environment

| Field | Value |
|---|---|
| commit | `08469d49a6f8b5e30436e43acfe143af054d8c3a` |
| ref | `main` |
| env | `production` |
| dbHost fingerprint | `ep-super-king-afqr3kxf` |

---

## B) Joe request status

| Field | Value |
|---|---|
| Request prefix | `f15df8e5` |
| Case prefix | `bc9e464e` |
| Case type | `I130_BONAFIDE_MARRIAGE` |
| Witness | Joe Average |
| Witness email domain | `gmail.com` |
| Inviter (requester) email | `dr.emily.dds@gmail.com` (Emily Dental — applicant) |
| Status | `SUBMITTED` |
| Witness submitted at | `2026-06-11T22:55:09.658Z` |
| `attorney_approval_required` (persisted) | `false` |
| `attorney_approved_at` | **null** (never approved) |
| Finalization / signing | `signature_preparation.status = ready_for_manual_signature` |
| PDF prepared at | `2026-06-11T22:55:09.629Z` |
| PDF sent to requester at | **`2026-06-11T22:55:09.629Z`** (same second as submit) |
| Provider e-sign / Dropbox | Not triggered (manual DocuSign prep only) |

**Timeline summary:** Witness final submit and full declaration PDF preparation/email occurred in the same second (~22:55:09 UTC on 2026-06-11), **before** any attorney approval and while `attorney_approved_at` remains null.

**`affidavit_request_events`:** No rows for this request (empty).

---

## C) Email audit (`email_messages`)

### Rows linked to Joe request (`affidavit_request_id = f15df8e5…`)

| sent_at (UTC) | recipient | template_key | subject (abbrev) | attachment in DB |
|---|---|---|---|---|
| 2026-06-11T20:09:35 | witness (`lueugene…@gmail.com`) | `firm_intake_witness_invite` | Emily Dental + Daniel Reed need your help… | n/a (not stored) |
| 2026-06-11T22:01:56 | witness (`lueugene…@gmail.com`) | `firm_intake_witness_invite` | …invited you to provide a relationship support statement | n/a (not stored) |

### Applicant inbox (`dr.emily.dds@gmail.com`) on submit day

| sent_at (UTC) | template_key | subject (abbrev) |
|---|---|---|
| 2026-06-11T19:53:46 | `firm_applicant_intake_invite` | ASN Demo Immigration Practice invited you to start… |

**No `email_messages` row** for:
- `prepare_for_manual_signature_pdf`
- Subject containing “Declaration ready for signature”
- Recipient `dr.emily.dds@gmail.com` at ~22:55 UTC

**Why the gap:** `prepare-for-signature` sends PDF email with `disableTracking: true` and plain text only (no HTML). `zohoMail.sendEmail` inserts `email_messages` **only** when tracking is enabled (`!disableTracking && htmlBody`). PDF declaration emails are therefore **not auditable via `email_messages`** even when successfully delivered.

---

## D) PDF send timing

| Event | Timestamp | Approval state at time |
|---|---|---|
| Witness submit + PDF prep | `2026-06-11T22:55:09.629Z` | `attorney_approved_at = null`; persisted flag `false` |
| DB `pdfSentToRequesterAt` set | Same timestamp | Proves full prepare-for-signature PDF path completed |
| Attorney approval | Never | Still null at diagnosis time |

**Conclusion:** Applicant/requester (`inviter_email`) received the full declaration PDF at witness submit time, **not** after attorney approval. Operator report of PDF in applicant inbox is **consistent with production DB state**.

Attachment filename (code-derived, not stored in DB): `witness-declaration-Joe_Average.pdf`  
Email subject (code-derived): `Declaration ready for signature — Joe Average`

---

## E) Code path

**Route:** `POST /api/affidavit/[token]/prepare-for-signature`  
**File:** `src/app/api/affidavit/[token]/prepare-for-signature/route.ts`

### Attorney gate (lines ~241–323)

When `isAttorneyApprovalBlockingSignaturePrep({ attorneyApprovalRequired, status })` is **true**:
- Sets status `SUBMITTED`, notifies attorney (`notifyAttorneyStatementSubmittedForReview`)
- Returns **423** `ATTORNEY_APPROVAL_PENDING`
- **Does not** generate PDF or email attachment

Blocking requires `attorneyApprovalRequired === true` (persisted flag only at submit time in pre–PR #96 behavior). PR #96+ also derives requirement via `shouldRequireAttorneyReviewForRequest()` from case supervision **before** this check.

### Full PDF path (lines ~379–586) — **what ran for Joe**

When attorney gate does **not** block:
1. `renderPreparedStatementPdf()` → PDF bytes
2. `sendEmail({ to: inviterEmail, … attachments: [witness-declaration-….pdf], templateKey: 'prepare_for_manual_signature_pdf', disableTracking: true })`
3. Sets `signature_preparation` including `pdfSentToRequesterAt`
4. Sets request status `SUBMITTED`

**Recipient:** `inviter_email` on the request row (= applicant Emily Dental for firm-linked intake, **not** the witness).

**Joe submit context:** Persisted `attorney_approval_required=false` on a firm-linked I-130 BFM case caused the gate to **not** block at submit time (2026-06-11, before PR #96 derived policy was deployed). Full PDF email to applicant proceeded.

### Other PDF attachment senders

Only other `witness-declaration-*.pdf` attachment path found: admin `cleanup-pdf` (operator download, not applicant notification). No alternate applicant PDF route identified.

---

## F) Classification

**APPLICANT_PDF_BEFORE_APPROVAL_BUG**

| Criterion | Met |
|---|---|
| Applicant received full declaration PDF | **Yes** (DB `pdfSentToRequesterAt` + `inviter_email` = applicant) |
| Before attorney approval | **Yes** (`attorney_approved_at` null) |
| While not attorney-approved workflow state | **Yes** (firm-linked case; attorney review expected post–PR #96) |
| Witness-only copy | **No** — witness emails were invites only |

Not `EXPECTED_AFTER_APPROVAL` (no approval timestamp).  
Not `WITNESS_COPY_ONLY` (PDF goes to inviter/applicant, not witness).  
Not `UNKNOWN_NEEDS_EML` — DB `signature_preparation` is sufficient proof; EML would only corroborate subject/attachment filename.

---

## G) Recommended fix

1. **Joe row (operational):** Treat applicant inbox PDF as sent under pre-fix behavior. Do **not** approve pre-PR97 statement. Prefer **request revision** or fresh controlled witness after PR #97 regen quality is confirmed.

2. **Already shipped (PR #96):** `prepare-for-signature` now derives `shouldRequireAttorneyReviewForRequest()` for firm-linked cases — **new** submits should hit 423 and **not** email PDF before approval.

3. **Stale row gap:** Joe still has `signature_preparation` populated while attorney review pending — consider a controlled repair pass to clear or freeze prep state on legacy firm-linked rows (separate approved migration; not done in this diagnosis).

4. **Observability:** Insert metadata-only `email_messages` row for `prepare_for_manual_signature_pdf` even when `disableTracking: true` (recipient, subject, template, request id, sent_at — no body/attachment bytes).

5. **Optional product guard:** Block applicant re-download or re-send of prepared PDF when derived attorney review is required and `attorney_approved_at` is null.

---

## H) Safety confirmation

- Read-only DB queries via Neon MCP; no production writes
- No emails sent, no approval, no providers, no mutations
- No full tokens, magic links, cookies, DATABASE_URL, statement text, or ID image contents in this report
- Witness invite `email_messages` rows cited by template/subject/time only

---

**APPLICANT_PDF_BEFORE_APPROVAL_BUG**
