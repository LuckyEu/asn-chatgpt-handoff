# First review-ready draft artifact spec

**Date:** 2026-06-14  
**Verdict:** `FIRST_REVIEW_READY_DRAFT_SPEC_READY`  
**Task:** Design/spec only — no product code, migrations, or production writes

---

## A) Artifact definition

**`first_review_ready_draft`** is the first **immutable** statement snapshot ready for attorney review — not autosave, not raw witness answers, not approved final text, not PDF.

Created after: witness completes answers → structured statement generated → spelling/syntax normalization → case-type semantic checks → unsupported-claim / personal-knowledge flags → author-control recorded.

**Before:** attorney approval and post-approval PDF prep.

**Related kinds:**
- `revision_review_ready_draft` — each resubmit after attorney revision (v2+)
- `approved_version` — at attorney approval

**Not:** one per autosave; exactly one `first_review_ready_draft` per request ever.

---

## B) Data model

Proposed table **`statement_review_snapshots`** with fields: id, affidavit_request_id, case_id, organization_id, affidavit_version_id, witness ref, case_type_code, support_statement_purpose, execution_requirement, version_number, artifact_kind, created_at, created_by, source_answer_refs (jsonb refs only), author_control_mode, ai_assistance_used, ai_acknowledgement_at, statement_text_snapshot, statement_text_hash, section_map, question_to_section_map, quality_flags, unsupported_claim_flags, personal_knowledge_flags, relationship_scope, reviewer_status, attorney_revision_reasons, metadata (safe jsonb).

**Must not store:** tokens, magic links, ID images, PDF bytes, provider secrets.

Unique constraint: `(affidavit_request_id, artifact_kind, version_number)`.

Current gap: only mutable `affidavit_versions.final_letter_text` on latest row; timeline derived, not snapshot-backed.

---

## C) UX placement

**Only** `/attorney/review/[requestId]` (PR #107 single workspace):

| Panel | Role |
|-------|------|
| Statement | Current review-ready text under review |
| Review process record | Author control, AI, source answer metadata |
| Statement history | First review-ready draft + revision drafts + approved version |
| Attorney decision | Approve / revision when pending |

Case detail, intake-links, tasks → link only. No disconnected report page.

---

## D) Revision / approval / PDF relationship

**Revision:** `first_review_ready_draft` immutable; new `revision_review_ready_draft` on resubmit; history shows revision cycle.

**Approval:** create `approved_version` snapshot; hash-align with approved text.

**PDF:** Phase 3 uses `approved_version` as source. **PR #99 guard unchanged** — no PDF before effective attorney approval. Do not generate PDF from pre-approval snapshots.

---

## E) B2B ledger relationship

| Event | Count? |
|-------|--------|
| Snapshot creation (first or revision) | No |
| Attorney approval | Yes (`statement_approved`) |
| PDF prep/send | No |

Optional ledger metadata: `approved_version_snapshot_id` (uuid only, no statement text). Production ledger rollout remains paused.

---

## F) Diagrams (summary)

1. **Artifact lifecycle** — raw answers → structured draft → first_review_ready_draft → review → revision snapshots → approved_version → PDF
2. **Attorney review workspace** — case/tasks/intake → review page (statement, process record, history, attorney decision)
3. **State machine** — waiting_on_witness → submitted_for_review → needs_revision → resubmitted_for_review → approved → pdf_prepared/sent

---

## G) Implementation phases

| Phase | Scope |
|-------|-------|
| **1** | Table/model; snapshot on submit/resubmit; display in statement history |
| **2** | Diff/compare versions; quality flag display; source answer section links |
| **3** | approved_version as PDF source; ledger references approved_version id |

---

## H) Safety / privacy rules

- No production writes, emails, approvals, signing, payments, Proof, Dropbox, or ledger rollout in spec pass
- No migrations or manual SQL
- No secrets, tokens, magic links, full statement text, ID contents, PDF bytes, or blob URLs in handoff
- Snapshot text access-controlled in DB when implemented; never in B2B event metadata or handoff

---

## I) Recommendation

**Proceed with Phase 1** after operator privacy/schema sign-off. This artifact is the highest-value workflow record between witness completion and attorney decision — more meaningful than "first draft saved." Integrate entirely into the PR #107 review workspace. Ledger touchpoint stays at approval only.

---

## J) Safety confirmation

| Check | Status |
|-------|--------|
| Product code changed | No |
| Migrations run | No |
| Production writes | No |
| Secrets/statement text in report | No |

---

## Verdict

**`FIRST_REVIEW_READY_DRAFT_SPEC_READY`**
