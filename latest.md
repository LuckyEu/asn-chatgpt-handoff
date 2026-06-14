# Latest — First review-ready snapshots merged; production enablement pending

**Verdict:** `FIRST_REVIEW_READY_SNAPSHOTS_MERGED`  
**Date:** 2026-06-14  
**Production commit:** `a5aab1dd8874ef7f8de2be0a8ac4a527ea4acdbe`  
**Archives:**
- reports/2026-06-14-1610-snapshot-timeline-sanitizer-merge.md (PR #109)
- reports/2026-06-14-1449-pr108-first-review-ready-snapshots-merge.md (PR #108)

## Summary

- PR #108 adds `first_review_ready_draft` and `revision_review_ready_draft` snapshot support behind `STATEMENT_REVIEW_SNAPSHOTS_ENABLED`.
- PR #109 fixes statement timeline sanitizer so snapshot timeline metadata does not expose or trip on `statementTextHash`.
- Production code is deployed, but migration 119 is not applied.
- `STATEMENT_REVIEW_SNAPSHOTS_ENABLED` remains off in production.
- No production snapshots are expected yet.
- Before production enablement: re-run dev/staging smoke on current main, then apply migration 119 and enable the flag only with operator approval.
- B2B ledger production rollout remains separate.

## Milestones

| Milestone | Verdict | Merge commit |
|-----------|---------|--------------|
| PR #108 — first/revision review-ready snapshots (Phase 1) | `PR108_MERGE_PASS` | `39e2ef22` |
| PR #109 — timeline sanitizer for snapshot metadata | `SNAPSHOT_TIMELINE_SANITIZER_MERGE_PASS` | `a5aab1dd` |

## Phase 1 scope (merged)

- Migration 119 + `MIGRATION_ORDER.md` entry (dev/staging applied during pre-merge smoke; **not** on production)
- Snapshot create on witness submit/resubmit when flag enabled (attorney-blocking prepare-for-signature path)
- Attorney snapshots API + Statement history UI integration
- Kill switch: `STATEMENT_REVIEW_SNAPSHOTS_ENABLED` — default **off**

## Production status (read-only)

| Item | Status |
|------|--------|
| Migration 119 on production | **Not applied** |
| `STATEMENT_REVIEW_SNAPSHOTS_ENABLED` | **Off** |
| Production snapshot rows | **None expected** |
| B2B ledger production | **Still separate / not enabled** |

## Next operational step

`first_review_ready_snapshots_dev_smoke_after_sanitizer` — re-run dev/staging smoke on current main, then coordinate production migration 119 + flag enablement with operator approval only.

## Out of scope (confirmed)

- Production migration apply or env changes (this handoff pass)
- B2B ledger production rollout
- Payments / Stripe / providers / signing automation
- PDF sourcing from snapshots / `approved_version` (later phase)

## Safety

Handoff archives are redacted Markdown only. No secrets, tokens, magic links, full statement text, ID data, or PDF bytes.
