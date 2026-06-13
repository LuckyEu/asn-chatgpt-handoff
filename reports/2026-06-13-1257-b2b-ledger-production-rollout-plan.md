# B2B usage ledger — production rollout plan

**Execution verdict:** B2B_LEDGER_PRODUCTION_ROLLOUT_BLOCKED  
**Plan status:** READY (awaiting operator approvals)  
**Date:** 2026-06-13  
**Prerequisite:** B2B_LEDGER_DEV_SMOKE_PASS

---

## A) Verdict summary

| Item | Status |
|------|--------|
| Production rollout executed | **No** — blocked on missing approvals |
| Production rollout plan prepared | **Yes** |
| Dev/staging smoke | PASS |
| Code on production | PR #102 merged and deployed (`3463a447`) |
| Migration 118 on production | **Not applied** |
| `B2B_USAGE_LEDGER_ENABLED` on production | **Not set** |

---

## B) Required approvals (gate)

| Approval token | Purpose |
|----------------|---------|
| `APPROVE_PRODUCTION_MIGRATION_118_B2B_USAGE_EVENTS` | Apply migration 118 to production Neon |
| `APPROVE_ENABLE_B2B_USAGE_LEDGER_PRODUCTION` | Set `B2B_USAGE_LEDGER_ENABLED=true` on production |

Historical backfill requires **separate** approval — default: do not run.

---

## C) Critical ordering

**Migration 118 before flag enable.** Flag-on without table → attorney approval **503** `SCHEMA_MISSING`.

---

## D) Phased runbook

1. **Dry-run** production migrations — expect exactly one pending: `118_create_b2b_usage_events_neon.sql`
2. **Apply** migration 118 (with migration approval)
3. **Verify** table, unique constraint, indexes; row count **0**
4. **Enable** `B2B_USAGE_LEDGER_ENABLED=true` on production (with env approval); redeploy
5. **Smoke** one demo/smoke-tagged firm-supervised approval → one `statement_approved`, `non_billable_test`
6. **Verify** idempotency, admin API (sanitized), no backfill

---

## E) Production smoke fixture

| Requirement | Detail |
|-------------|--------|
| Supervision | Firm-linked or attorney-owned |
| Status | SUBMITTED, not yet approved |
| Billable tag | smoke/demo → `non_billable_test` |
| Prohibited | real emails, Stripe, providers, signing, backfill |

---

## F) Rollback

Set `B2B_USAGE_LEDGER_ENABLED=false` and redeploy. Table may remain (append-only, unused while flag off).

---

## G) Safety (plan pass)

No production migration, env change, or smoke performed in plan-only pass.

**Execution verdict: B2B_LEDGER_PRODUCTION_ROLLOUT_BLOCKED**  
**Plan verdict: READY**

Synthetic demo · Not legal advice.
