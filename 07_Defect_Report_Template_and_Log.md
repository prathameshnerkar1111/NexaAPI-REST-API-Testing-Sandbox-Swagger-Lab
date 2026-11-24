# 07. Defect Report Template & API Defect Log — NexaAPI

## 1. Jira-Ready API Defect Template

```markdown
**Defect ID:** BUG-XXX
**Summary:** [API Module] Concise one-line description of failure
**Environment:** Postman v11 / Newman / NexaAPI Base URL ({{baseUrl}})
**Severity & Priority:** Blocker|Critical|Major|Minor / P0-P3
**Linked Test Case / Req:** TC-XXX / REQ-API-XX
**Preconditions:** Persona role, seed account balance, or chaos toggle state
**Steps to Reproduce:**
1. Authenticate via POST /api/v1/auth/login...
2. Send request with payload / headers...
**Expected Result:** Exact HTTP status code, headers, and JSON contract
**Actual Result:** Observed HTTP status code and response body
**Evidence:** Postman Console Log / cURL command / Newman HTML Report
```

## 2. Active API Defect Log

| Defect ID | Linked TC | Module | Summary | Severity | Priority | Status | Steps to Reproduce | Expected vs. Actual |
| :--- | :--- | :--- | :--- | :---: | :--- | :---: | :--- | :--- |
| **BUG-401** | `TC-028` | Idempotency Protection | Reusing Idempotency-Key with modified transfer amount returns 200 replay instead of 409 Conflict | Major | P1 - High | `Open` | 1. POST /api/v1/transfers with Idempotency-Key: idem-001 and amount=1500.<br>2. Resend POST /api/v1/transfers with Idempotency-Key: idem-001 and amount=9999. | **Exp:** HTTP 409 Conflict with error='IDEMPOTENCY_PAYLOAD_MISMATCH'.<br>**Act:** HTTP 200 OK returned cached ₹1,500 transfer without validating payload hash. |
| **BUG-402** | `TC-031` | Beneficiaries & IFSC | IFSC validation checks length=11 but allows non-zero 5th character (e.g., HDFC1001234) | Minor | P2 - Medium | `Open` | 1. Authenticate as STANDARD user.<br>2. POST /api/v1/beneficiaries with ifsc='HDFC1001234'. | **Exp:** HTTP 400 Bad Request with error='INVALID_IFSC' (RBI IFSC 5th char must be 0).<br>**Act:** HTTP 201 Created accepted invalid IFSC string. |
| **BUG-403** | `TC-040` | Chaos, Rate Limit & SLA | HTTP 429 rate-limit response omits standard Retry-After response header | Minor | P2 - Medium | `Open` | 1. Enable Simulate 429 in QA Chaos Bar.<br>2. Send GET /api/v1/accounts in Postman and inspect Headers tab. | **Exp:** HTTP 429 Too Many Requests including 'Retry-After: 60' header.<br>**Act:** HTTP 429 returned with JSON body only; Retry-After header missing. |
