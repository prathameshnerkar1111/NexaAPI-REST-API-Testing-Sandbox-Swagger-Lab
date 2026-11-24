# 01. Master API Test Plan — NexaAPI REST Testing Sandbox & Swagger Lab

| Document Control | Details |
| :--- | :--- |
| **Application Under Test** | NexaAPI — REST API Testing Sandbox & Swagger Lab |
| **Primary Tooling** | Postman (Collection Runner), Newman CLI, Playwright `APIRequestContext` |
| **Version & Date** | v1.0 \| November 2026 |
| **QA Lead** | Prathamesh Nerkar (Senior Quality Analyst) |
| **Standard Followed** | IEEE 829 / ISO/IEC/IEEE 29119-3 |

---

## 1. Objective
Define the functional API testing scope, Postman automation strategy, test data boundaries, chaos/resilience checks, entry/exit criteria, and deliverables for **NexaAPI** across authentication, banking account queries, INR transfers, idempotency, beneficiary IFSC validation, ledger pagination, and card controls.

## 2. Scope of Testing

### 2.1 In-Scope API Modules
* **Health & System (`GET /api/v1/health`):** Liveness, schema, and baseline SLA (`< 500ms`).
* **Authentication & RBAC (`POST /auth/login`, `GET /auth/me`):** JWT issuance for `STANDARD` and `VIP` roles, `401` invalid credentials, `403` locked account, and Bearer token guards.
* **Accounts & Balances (`GET /accounts`, `GET /accounts/{id}`):** Balance accuracy in INR, `404` missing IDs, unauthorized access rejection.
* **INR Transfers & Idempotency (`POST /transfers`):** Positive debit/credit math, `₹1` minimum, `₹1,00,000` STANDARD / `₹5,00,000` VIP daily limits, insufficient funds (`422`), and `Idempotency-Key` replay protection (`200` vs `409`).
* **Beneficiaries (`GET/POST/DELETE /beneficiaries`):** 11-char RBI IFSC regex validation (`^[A-Z]{4}0[A-Z0-9]{6}$`) and duplicate account conflict (`409`).
* **Transactions & Cards (`GET /transactions`, `PATCH /cards/{id}/status`):** Ledger filtering, pagination boundaries, and `ACTIVE`/`FROZEN` card toggles.
* **Chaos, Security & Reset:** `HTTP 429` rate limits, `HTTP 500` chaos error contracts, malformed JSON (`400`), XSS/SQLi input hardening, and `/admin/reset`.

### 2.2 Out-of-Scope
* Real core banking settlement (NEFT/RTGS/IMPS clearing house integration).
* Production DDoS/penetration testing and multi-region load stress testing (>10,000 RPS).

## 3. API Testing Strategy

| Testing Type | Validation Focus in Postman | Execution Mode |
| :--- | :--- | :--- |
| **Contract & Schema** | Exact HTTP status codes, `Content-Type: application/json`, required JSON keys & data types | Postman `pm.test` |
| **Functional & State** | Token chaining (`authToken`), balance delta verification before/after transfer, CRUD lifecycle | Collection Runner |
| **Boundary & Negative** | `₹0`, `₹1`, `₹1,00,000`, `₹1,00,001`, 10/11/12-char IFSC, `400/401/403/404/409/422` | Data-Driven / Newman |
| **Chaos & SLA** | `< 500ms` baseline SLA, `1500ms` latency injection, `429` Retry-After, `500` error JSON | Chaos Bar + Postman |

## 4. Entry & Exit Criteria

### Entry Criteria
1. NexaAPI server / Swagger Lab is reachable at `{{baseUrl}}` and `GET /api/v1/health` returns `200 OK`.
2. `POST /api/v1/admin/reset` verified to restore clean seed balances and accounts.
3. Postman Collection (`v2.1`) and Environment JSON imported and reviewed.

### Exit Criteria
1. **100%** of `P0 - Critical` API test cases pass with **0 Open Blocker/Critical defects**.
2. **>= 95%** pass rate across `P1 - High` test cases.
3. Newman HTML report archived and QA Sign-Off Report approved.

## 5. Risks & Mitigations

| Risk | Impact | Mitigation Strategy |
| :--- | :--- | :--- |
| Dirty state across sequential Postman runs | High | Run `POST /api/v1/admin/reset` as Folder 0 / pre-suite step before every execution. |
| Expired or missing Bearer token during collection run | Medium | Auto-save `authToken` via Post-Response script in Login request and reference `{{authToken}}`. |
| Duplicate debit on network retry | Critical | Enforce and test `Idempotency-Key` header replay (`TC-026`–`TC-028`). |
