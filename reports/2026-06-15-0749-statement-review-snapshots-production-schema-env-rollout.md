# Statement review snapshots — production schema/env rollout

**Verdict:** `STATEMENT_REVIEW_SNAPSHOTS_SCHEMA_ENV_ROLLOUT_PASS`  
**Date:** 2026-06-15  
**Approvals exercised:**
- `APPROVE_PRODUCTION_MIGRATION_118_B2B_USAGE_EVENTS`
- `APPROVE_PRODUCTION_MIGRATION_119_STATEMENT_REVIEW_SNAPSHOTS`
- `APPROVE_ENABLE_STATEMENT_REVIEW_SNAPSHOTS_PRODUCTION`

**Prerequisite dry-run:** `2026-06-14-1944-statement-review-snapshots-production-dry-run.md`

---

## A) Production preflight

| Field | Value |
|-------|-------|
| URL | https://www.affidavitsupport.net/api/build-info |
| commit | `a5aab1dd8874ef7f8de2be0a8ac4a527ea4acdbe` |
| ref | `main` |
| env | `production` |
| dbHost fingerprint | `ep-super-king-afqr3kxf` |

**Expectation check:** env=production, fingerprint match, commit includes PR #108 + PR #109 baseline. **PASS**

---

## B) Dry-run before apply

| Item | Result |
|------|--------|
| Command | `pnpm db:migrate:dry` (fingerprint guard: `ep-super-king-afqr3kxf`) |
| Mode | DRY RUN — no `--yes` |
| Applied before | 110 / 112 |
| Pending | 2 |

**Pending migrations (expected):**
1. `118_create_b2b_usage_events_neon.sql`
2. `119_create_statement_review_snapshots_neon.sql`

**Classification:** queue unchanged — proceed.

---

## C) Migration 118 apply result

| Check | Result |
|-------|--------|
| Approval | `APPROVE_PRODUCTION_MIGRATION_118_B2B_USAGE_EVENTS` |
| Method | `applyOneMigration` via project migration runner (6 statements) |
| `public.b2b_usage_events` exists | **Yes** |
| Unique constraint `b2b_usage_events_request_event_unique` | **Yes** |
| Indexes | **6** |
| Row count | **0** |
| `schema_migrations` entry | **Yes** |
| Backfill | **None** |

---

## D) Migration 119 apply result

| Check | Result |
|-------|--------|
| Approval | `APPROVE_PRODUCTION_MIGRATION_119_STATEMENT_REVIEW_SNAPSHOTS` |
| Method | `applyOneMigration` via project migration runner (7 statements) |
| `public.statement_review_snapshots` exists | **Yes** |
| Unique `statement_review_snapshots_request_version_unique` | **Yes** |
| Unique `statement_review_snapshots_first_draft_unique` | **Yes** |
| Indexes | **6** |
| Row count | **0** |
| `schema_migrations` entry | **Yes** |
| Backfill | **None** |

---

## E) Env enable result

| Item | Result |
|------|--------|
| Approval | `APPROVE_ENABLE_STATEMENT_REVIEW_SNAPSHOTS_PRODUCTION` |
| Variable set | `STATEMENT_REVIEW_SNAPSHOTS_ENABLED=true` (Production scope) |
| Redeploy | `vercel --prod --force` |
| Deployment status | **Ready** |
| Production alias | https://www.affidavitsupport.net |

**Not set:** `B2B_USAGE_LEDGER_ENABLED` (absent from Production env)

---

## F) Post-deploy build identity

| Field | Value |
|-------|-------|
| env | `production` |
| dbHost fingerprint | `ep-super-king-afqr3kxf` |
| App status | Active (deployment Ready, alias serving) |
| commit (build-info) | *(empty — CLI `--prod --force` redeploy from local worktree does not inject `VERCEL_GIT_COMMIT_SHA`)* |
| Preflight commit | `a5aab1dd…` (unchanged code path; snapshot feature present) |

**Note:** Post-redeploy `build-info.commit` is blank due to Vercel CLI local deploy metadata. Preflight confirmed production was on `a5aab1dd` before rollout; redeployed bundle is from worktree containing `a5aab1dd` as ancestor (includes PR #108 + PR #109 snapshot code).

---

## G) Row-count verification

| Table | Row count |
|-------|-----------|
| `public.b2b_usage_events` | **0** |
| `public.statement_review_snapshots` | **0** |
| `schema_migrations` total | **112 / 112** |

Both migrations recorded in `schema_migrations`. No unexpected pre-existing rows.

---

## H) B2B ledger disabled confirmation

| Variable | Production |
|----------|------------|
| `B2B_USAGE_LEDGER_ENABLED` | **Absent** (not enabled) |
| `STATEMENT_REVIEW_SNAPSHOTS_ENABLED` | **Present** (Production) |

Migration 118 created schema only; B2B ledger runtime remains disabled.

---

## I) Backfill status

**None.** No backfill scripts run. Both new tables empty at rollout completion.

---

## J) Safety confirmation

| Constraint | Status |
|------------|--------|
| Only migrations 118 + 119 applied | **Yes** |
| No B2B ledger enablement | **Yes** |
| No production case/invite/witness creation | **Yes** |
| No controlled production snapshot smoke | **Yes** |
| No emails/invites/payments/providers/signing/PDF | **Yes** |
| No backfill | **Yes** |
| Secrets / DATABASE_URL in report | **None** |
| Product repo staged/committed/pushed | **None** |

---

## Summary

Production schema and env rollout for statement review snapshots completed successfully. Migrations 118 (B2B usage events schema prerequisite) and 119 (statement review snapshots) applied in order with zero rows. Feature flag `STATEMENT_REVIEW_SNAPSHOTS_ENABLED=true` set on Production and redeployed. B2B ledger remains disabled.

**Next step (out of scope this pass):** controlled production snapshot smoke when separately approved.

**Verdict:** `STATEMENT_REVIEW_SNAPSHOTS_SCHEMA_ENV_ROLLOUT_PASS`
