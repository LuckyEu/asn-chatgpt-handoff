# Latest — PDF delivery now requires effective attorney approval — PR #99

**Verdict:** `PR99_MERGE_PASS`  
**Date:** 2026-06-12  
**Archive:** reports/2026-06-12-1210-pr99-stale-signature-prep-merge.md  
**Product PR:** https://github.com/LuckyEu/affidavit-support-network/pull/99  
**Production commit:** `639a212e21f7c3b2e1061921c20ffca259d9de39`

## Summary

- PR #99 prevents firm-linked applicant PDF delivery before effective attorney approval.
- `prepare-for-signature` now returns attorney-pending instead of sending PDF when review is required.
- Revision and approval clear stale `signature_preparation` so future output regenerates with current code.
- Joe production artifact remains pre-PR97 and **should not be approved as-is**.
- **Next step:** controlled Joe revision/resubmit workflow.
- No Joe mutation, approval, emails, or provider calls occurred during merge.

## What shipped

| Guard | Behavior |
|-------|----------|
| Derived attorney-review policy | Firm-linked cases block PDF even when persisted `attorney_approval_required=false` |
| Before approval | 423 `ATTORNEY_APPROVAL_PENDING` — no PDF, email, or `pdfSentToRequesterAt` |
| Stale pre-approval prep | Not treated as `alreadySubmitted`; safe block message on re-access |
| Request revision | Clears `signature_preparation` |
| Attorney approval | Clears `signature_preparation` for fresh post-approval regen |
| Witness submit | Routes 423 to attorney-pending thank-you |
| Observability | Metadata-only `email_messages` for manual-signature PDF sends when tracking disabled |

## Production identity

- `GET https://www.affidavitsupport.net/api/build-info` → commit `639a212e…`, ref `main`, env `production`

## Prior milestones

- PR #97: BFM declaration quality
- PR #96/#98: firm-linked attorney review queue and review actions
- Diagnosis: applicant PDF-before-approval bug (Joe E2E)

Synthetic demo · Not legal advice.
