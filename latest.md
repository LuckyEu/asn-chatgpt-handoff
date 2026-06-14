# Latest — First review-ready draft artifact spec ready

**Verdict:** `FIRST_REVIEW_READY_DRAFT_SPEC_READY`  
**Date:** 2026-06-14  
**Archive:** reports/2026-06-14-1207-first-review-ready-draft-artifact-spec.md  
**Production commit (unchanged):** `f654fb06ec82d96d812fe308f84ff858e1c05095`

## Summary

- Spec defines `first_review_ready_draft` as the first immutable review-ready statement snapshot before attorney approval.
- It is not autosave, raw answers, approved text, or PDF.
- Planned artifact kinds: `first_review_ready_draft`, `revision_review_ready_draft`, `approved_version`.
- Intended UX placement is inside `/attorney/review/[requestId]`, not a separate report route.
- B2B ledger should count attorney approval only; snapshot creation does not count.
- PDF generation should later source from `approved_version`.
- No product code, migrations, production writes, emails, or artifacts were changed.
- **Next operational decision:** implement Phase 1 snapshots or defer and return to B2B ledger production rollout.

## Artifact definition (concise)

| Kind | When |
|------|------|
| `first_review_ready_draft` | First witness submit for attorney review (one per request, ever) |
| `revision_review_ready_draft` | Each resubmit after attorney revision (v2+) |
| `approved_version` | Attorney approval event |

Trigger: witness passes final review + DraftGate; normalized statement persisted; author-control and case-type checks recorded — **before** attorney decision and PDF prep.

## Current product gap

Today only mutable `affidavit_versions.final_letter_text` on the latest row exists. Statement history timeline is derived from DB timestamps; `firstReviewReadyDraftCreated` label is reserved but not yet backed by persisted snapshots.

## Implementation phases (planned)

| Phase | Scope |
|-------|-------|
| 1 | Table/model; snapshot on submit/resubmit; display in statement history |
| 2 | Diff/compare versions; quality flag display; source answer section links |
| 3 | `approved_version` as PDF source; ledger references approved_version id |

## Prior milestone (context)

- PR #107: attorney review single decision workspace — `/attorney/review/[requestId]` is the sole approve/revision surface; case detail and intake progress link out only.

## Out of scope (confirmed)

- Product implementation of snapshots (Phase 1 not started)
- B2B ledger production enablement (still paused)
- Production data mutation
- Payments / Stripe / providers / signing automation

## Safety

Design/spec pass only. No secrets, tokens, magic links, full statement text, ID data, or PDF bytes in handoff archives.
