# Latest — BFM declaration regen PASS; applicant PDF-before-approval bug diagnosed

**Verdict:** BFM_DECLARATION_PREVIEW_REGEN_PASS + APPLICANT_PDF_BEFORE_APPROVAL_BUG  
**Date:** 2026-06-12  
**Archives:**
- reports/2026-06-12-0925-bfm-declaration-quality-preview-regen.md
- reports/2026-06-12-0932-joe-applicant-pdf-email-diagnosis.md

## Summary

- PR #97 future BFM declaration output passes preview/dev regeneration (synthetic fixture; all quality checklist items PASS).
- Existing production Joe row (`f15df8e5…`) remains a pre-PR97 artifact and **should not be approved**.
- Diagnosis confirmed the full declaration PDF was sent to the applicant (`dr.emily.dds@…`) at witness submit time, **before** attorney approval, under pre-fix stale `attorney_approval_required=false` behavior on a firm-linked I-130 BFM case.
- DB proof: `pdfSentToRequesterAt` set same second as submit while `attorney_approved_at` is null; `email_messages` has no row for `prepare_for_manual_signature_pdf` (tracking disabled on that send path).
- **Next product fix:** stale `signature_preparation` / PDF delivery guard before requesting revision or approval (`stale_signature_prep_review_policy`).
- **No production writes** in either source report.

## BFM regen (synthetic)

| Check | Result |
|---|---|
| Post-PR97 declaration quality checklist | **13/13 PASS** |
| Production Joe touched | **No** |
| Unit tests (`bfmWitnessDeclarationQuality`) | **9/9 PASS** |
| PDF artifact | Local-only under Downloads `artifacts/` (not in cloud repo) |

## Applicant PDF diagnosis (read-only production)

| Check | Result |
|---|---|
| Applicant received full PDF before approval | **Confirmed** |
| Witness-only copy | **No** (PDF to inviter/applicant) |
| Joe approval recommended | **No** |
| Recommended operator action | Request revision or fresh controlled witness |

## Operational guidance

1. Do **not** approve Joe's stored statement as-is.
2. Ship or merge stale signature-prep review policy fix before treating legacy rows as signing-ready.
3. After witness resubmit + attorney approval, regenerate PDF using post-PR97 code path.

## Prior milestones

- PR #97: BFM declaration quality merge
- PR #98: attorney review actions via derived policy (production `08469d49`)
- E2E2B: Joe visible in attorney review queue with Approve / Request revision

Synthetic demo · Not legal advice.
