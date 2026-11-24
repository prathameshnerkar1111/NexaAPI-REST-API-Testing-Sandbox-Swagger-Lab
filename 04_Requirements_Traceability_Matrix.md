# 04. Requirements Traceability Matrix (RTM) — NexaAPI

Bi-directional traceability linking every API requirement (`REQ-API-XX`) to its Scenarios (`TS-XX`) and Test Cases (`TC-XXX`).

| Req ID | Module | Requirement Summary | Priority | Linked Scenarios | Linked Test Cases | Total TCs |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: |
| **REQ-API-01** | Health & System | GET /api/v1/health returns HTTP 200 with service status, version, uptime, and ISO timestamp without authentication. | P0 - Critical | TS-01 | TC-001, TC-002 | 2 |
| **REQ-API-02** | Authentication & RBAC | POST /api/v1/auth/login issues Bearer JWT for STANDARD and VIP roles, returns 401 for invalid credentials, and 403 for LOCKED accounts. GET /api/v1/auth/me requires valid Bearer token. | P0 - Critical | TS-02, TS-03 | TC-003, TC-004, TC-005, TC-006, TC-007, TC-008, TC-009, TC-010, TC-011 | 9 |
| **REQ-API-03** | Accounts & Balances | GET /api/v1/accounts and GET /api/v1/accounts/{id} return authenticated user's INR accounts with accurate balances; unknown IDs return 404 and unauthorized requests return 401. | P0 - Critical | TS-04 | TC-012, TC-013, TC-014, TC-015 | 4 |
| **REQ-API-04** | INR Fund Transfers | POST /api/v1/transfers debits source account, credits destination, enforces positive amount (>= ₹1), sufficient balance (422), and daily tier limits (STANDARD ₹1,00,000; VIP ₹5,00,000). | P0 - Critical | TS-05, TS-06 | TC-016, TC-017, TC-018, TC-019, TC-020, TC-021, TC-022, TC-023, TC-024, TC-025 | 10 |
| **REQ-API-05** | Idempotency Protection | POST /api/v1/transfers with duplicate Idempotency-Key header returns HTTP 200 OK with original transferId and idempotentReplay=true without double-debiting. | P0 - Critical | TS-07 | TC-026, TC-027, TC-028 | 3 |
| **REQ-API-06** | Beneficiaries & IFSC | CRUD on /api/v1/beneficiaries validates 11-character alphanumeric IFSC (^[A-Z]{4}0[A-Z0-9]{6}$), prevents duplicate account numbers (409), and supports deletion. | P1 - High | TS-08 | TC-029, TC-030, TC-031, TC-032, TC-033 | 5 |
| **REQ-API-07** | Transactions & Pagination | GET /api/v1/transactions supports filtering by accountId, type (DEBIT/CREDIT), and pagination (page, limit) with accurate total count metadata. | P1 - High | TS-09 | TC-034, TC-035, TC-036 | 3 |
| **REQ-API-08** | Cards Management | PATCH /api/v1/cards/{id}/status toggles ACTIVE/FROZEN states; invalid transitions or unknown card IDs return 400/404. | P1 - High | TS-10 | TC-037, TC-038, TC-039 | 3 |
| **REQ-API-09** | Chaos, Rate Limit & SLA | API enforces rate-limiting (HTTP 429 with Retry-After), gracefully surfaces simulated HTTP 500 errors when chaos mode is active, and responds within 500ms SLA under normal load. | P1 - High | TS-11 | TC-040, TC-041, TC-042 | 3 |
| **REQ-API-10** | Security & State Reset | API rejects malformed JSON (400), strips/blocks SQLi/XSS payloads, enforces Content-Type: application/json, and POST /api/v1/admin/reset restores seed balances cleanly. | P2 - Medium | TS-12 | TC-043, TC-044, TC-045 | 3 |
| **TOTAL** | — | **10 Requirements (100% Covered)** | — | **12 Scenarios** | **TC-001 – TC-045** | **45** |
