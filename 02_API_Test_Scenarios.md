# 02. High-Level API Test Scenarios — NexaAPI

All **12** high-level API test scenarios mapped to functional requirements (`REQ-API-XX`) and linked test cases.

| Scenario ID | Req ID | Module | Scenario Description | Priority | Linked TCs |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TS-01** | `REQ-API-01` | Health & System | Verify unauthenticated health endpoint returns 200 OK, schema fields, and <500ms SLA. | P0 - Critical | 2 |
| **TS-02** | `REQ-API-02` | Authentication & RBAC | Validate JWT login for STANDARD, VIP, invalid password, missing fields, and LOCKED personas. | P0 - Critical | 6 |
| **TS-03** | `REQ-API-02` | Authentication & RBAC | Verify Bearer token enforcement on /auth/me (valid, missing, expired/tampered token). | P0 - Critical | 3 |
| **TS-04** | `REQ-API-03` | Accounts & Balances | Retrieve account list and individual account details; verify 404 for invalid account IDs. | P0 - Critical | 4 |
| **TS-05** | `REQ-API-04` | INR Fund Transfers | Execute valid INR transfers and validate exact source/destination balance updates. | P0 - Critical | 4 |
| **TS-06** | `REQ-API-04` | INR Fund Transfers | Validate transfer boundary and negative rules (₹0, negative, >balance, >daily tier limit, same-account). | P0 - Critical | 6 |
| **TS-07** | `REQ-API-05` | Idempotency Protection | Verify duplicate Idempotency-Key replays original transfer without double-debiting. | P0 - Critical | 3 |
| **TS-08** | `REQ-API-06` | Beneficiaries & IFSC | Create, list, and delete beneficiaries with strict 11-char IFSC regex and duplicate guards. | P1 - High | 5 |
| **TS-09** | `REQ-API-07` | Transactions & Pagination | Filter transaction ledger by type/accountId and validate page/limit pagination boundaries. | P1 - High | 3 |
| **TS-10** | `REQ-API-08` | Cards Management | Freeze and unfreeze virtual debit/credit cards and verify invalid state/ID handling. | P1 - High | 3 |
| **TS-11** | `REQ-API-09` | Chaos, Rate Limit & SLA | Validate HTTP 429 rate-limit handling, HTTP 500 chaos error schema, and latency SLA. | P1 - High | 3 |
| **TS-12** | `REQ-API-10` | Security & State Reset | Verify malformed JSON, XSS/SQLi input rejection, and seed state restoration via /admin/reset. | P2 - Medium | 3 |
