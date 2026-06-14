# PR #105 merge report — attorney case detail approved/PDF state fix

**Date:** 2026-06-14  
**PR:** https://github.com/LuckyEu/affidavit-support-network/pull/105  
**Squash merge commit:** `e7968c8b7387e1954adcb738c0f897016f3a3081`  
**Prior production:** `5c864ef9`

---

## Verdict

**PR105_MERGE_PASS**

---

## Problem fixed

For firm-linked cases with legacy `SUBMITTED` witness rows that already had attorney approval and PDF sent, `/attorney/cases/[caseId]` showed awaiting review while list, tasks, and review pages were correct. Root cause: case GET payload omitted approval/signature prep fields on some paths; case detail UI treated raw `SUBMITTED` before workflow state.

---

## Scope (10 files)

- Case GET canonical overlay + legacy SELECT attorney columns
- Shared `attorneyWorkflowFieldsFromCanonicalRow` mapper
- Case detail workspace row-action ordering
- Regression tests (legacy approved+PDF, pending review, needs revision, approved-no-PDF)

**Not included:** payments, providers, B2B ledger rollout, migrations, production data changes.

---

## CI / checks

- Build and Test — pass
- Unit, Functional, Regression — pass
- Vercel — pass
- Targeted tests (47) — pass

---

## Production deploy

| Field | Value |
|-------|-------|
| `commit` | `e7968c8b7387e1954adcb738c0f897016f3a3081` |
| `ref` | `main` |
| `env` | `production` |

Prior commit `5c864ef9` superseded for this fix.

---

## Safety

No production writes, emails, approvals, signing, payments, providers, or B2B ledger rollout during merge/deploy verification.
