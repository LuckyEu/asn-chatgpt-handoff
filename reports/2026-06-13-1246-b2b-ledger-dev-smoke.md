# B2B pilot usage ledger — dev/staging smoke

**Verdict:** B2B_LEDGER_DEV_SMOKE_PASS  
**Date:** 2026-06-13  
**Prerequisite:** PR #102 merged (`3463a447`); production migration 118 not applied; production ledger flag off

---

## A) Environment and DB fingerprint

### Production build-info (reference only — no production writes)

| Field | Value |
|-------|-------|
| commit | `3463a4470d0f66b6fd9f10bd7c72d469110e0282` |
| ref | `main` |
| env | `production` |
| dbHost fingerprint | `ep-super-king-afqr3kxf` |

### Dev/staging target

| Check | Result |
|-------|--------|
| Dev DB fingerprint | `ep-wild-cloud-afbfdnz7` |
| `IS_DEV` | true |
| `IS_PROD` | false |
| Production DB used | **No** |

---

## B) Migration 118 result

| Check | Result |
|-------|--------|
| Migration file + MIGRATION_ORDER entry | present |
| Dev dry-run | 111/111 applied, 0 pending |
| Table `b2b_usage_events` | exists |
| Unique constraint `b2b_usage_events_request_event_unique` | exists |
| Indexes | 6 (pkey + unique + 4 partial/time indexes) |
| Production migration touched | **No** |

---

## C) Dev/staging env flag result

| Check | Result |
|-------|--------|
| `B2B_USAGE_LEDGER_ENABLED` (dev/local only) | `true` |
| Production env changed | **No** |

---

## D) Controlled approval fixture

Synthetic firm-linked I-130/BFM demo case on dev (attorney-demo supervision):

| Field | Value (redacted) |
|-------|------------------|
| Case | `19f9b0ed…` |
| Request | `bfab618b…` |
| Case type | `I130_BONAFIDE_MARRIAGE` |
| Couple | Emily Dental + Daniel Reed (synthetic metadata) |
| Witness | Joe Average Dev |
| Execution requirement | `DECLARATION_E_SIGN` |
| Supervision | Firm intake link `a14f0f21…` → attorney-demo |
| Marker | `demo_batch=b2b-ledger-dev-smoke-v2`, `smoke=true` |
| Email / provider / payment / signing | not used |

---

## E) Ledger event result

Exactly **one** row for request `bfab618b…`:

| Field | Value |
|-------|--------|
| `event_type` | `statement_approved` |
| `billable_status` | `non_billable_test` |
| `case_type_code` | `I130_BONAFIDE_MARRIAGE` |
| `execution_requirement` | `DECLARATION_E_SIGN` |
| Metadata | allowlisted keys only; witness first name `Joe` |
| Forbidden content | none |

---

## F) Idempotency result

| Check | Result |
|-------|--------|
| Retry approval | HTTP 400 `ALREADY_APPROVED` |
| Row count after retry | 1 |
| `ON CONFLICT DO NOTHING` | duplicate blocked |

---

## G) Negative event checks

Only attorney approval creates ledger events. Unit tests: **19/19 pass** (invite/submit/revision/PDF prep excluded; flag-off verified).

---

## H) Admin API result

`GET /api/admin/b2b/usage-events` — HTTP 200, sanitized events, no forbidden fields.

---

## I) Flag-off safety

Unit test “skips ledger insert when env flag is off” — **PASS**. Production flag not altered.

---

## J) Safety confirmation

Dev/staging only. No production writes, migration, env change, emails, Stripe, providers, backfill, or secrets in report.

**Verdict: B2B_LEDGER_DEV_SMOKE_PASS**

Synthetic demo · Not legal advice.
