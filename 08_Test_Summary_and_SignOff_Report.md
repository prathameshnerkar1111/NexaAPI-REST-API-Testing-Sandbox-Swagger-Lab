# 08. API Test Summary & Release Sign-Off Report — NexaAPI

## 1. Cycle Information

| Field | Value |
| :--- | :--- |
| **Application Under Test** | NexaAPI — REST API Testing Sandbox & Swagger Lab |
| **Execution Tool** | Postman Collection Runner / Newman CLI |
| **Test Cycle** | Cycle 1 — Full Functional, Contract, Negative & Chaos API Verification |
| **Prepared By** | Prathamesh Nerkar (Senior Quality Analyst) |
| **Report Date** | November 24, 2026 |

---

## 2. Planned Coverage Breakdown

| Priority Level | Planned TCs | Executed | Passed | Failed | Blocked | Pass Rate |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **P0 - Critical** | 19 | 0 | 0 | 0 | 0 | 0.0% |
| **P1 - High** | 21 | 0 | 0 | 0 | 0 | 0.0% |
| **P2 - Medium** | 5 | 0 | 0 | 0 | 0 | 0.0% |
| **TOTAL** | **45** | **0** | **0** | **0** | **0** | **0.0%** |

---

## 3. Open Review Findings
* **Major (1):** `BUG-401` — Idempotency-Key reuse with altered transfer amount returns cached `200` instead of `409 Conflict`.
* **Minor (2):** `BUG-402` (IFSC 5th character zero-check missing), `BUG-403` (`Retry-After` header missing on `429`).

---

## 4. Exit Criteria Checklist
* [ ] 100% of `P0 - Critical` API test cases executed and passed in Postman/Newman
* [x] Zero open Blocker or Critical severity defects
* [x] 100% Requirement-to-Test-Case traceability established (`10/10` Requirements)
* [ ] `BUG-401` resolved and regression-verified

---

## 5. Release Sign-Off
* **Recommendation:** `[ ] Go` / `[x] Conditional Hold (Pending Cycle 1 Newman Run & BUG-401 Fix)`
* **QA Lead:** ___________________________ **Date:** `2026-11-24`
