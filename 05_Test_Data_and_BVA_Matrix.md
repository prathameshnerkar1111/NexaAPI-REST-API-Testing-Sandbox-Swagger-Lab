# 05. Test Data & Boundary Value Analysis (BVA) Matrix — NexaAPI

## 1. Seed Personas & Account Balances

| Persona | Email | Password | Role | Daily Limit | Status |
| :--- | :--- | :--- | :---: | :---: | :---: |
| **Standard Customer** | `standard@nexabank.demo` | `Pass@1234` | `STANDARD` | `₹1,00,000` | `ACTIVE` |
| **VIP Customer** | `vip@nexabank.demo` | `Pass@1234` | `VIP` | `₹5,00,000` | `ACTIVE` |
| **Locked Customer** | `locked@nexabank.demo` | `Pass@1234` | `STANDARD` | `₹0` | `LOCKED` |

## 2. Equivalence Partitioning & BVA Test Dataset

| Domain / Group | Field / Header | Valid Nominal & Boundaries | Invalid Boundaries & Formats | Expected API Behavior |
| :--- | :--- | :--- | :--- | :--- |
| **Auth Credentials** | `email / password` | `standard@nexabank.demo / Pass@1234` | `locked@nexabank.demo; wrong password; empty {}` | 200 JWT for active; 401 bad creds; 403 locked; 400 empty |
| **Transfer Amount (STANDARD)** | `amount (INR)` | `5000 (nominal), 1 (min), 100000 (max)` | `0 (min-1), -500, 100001 (max+1), 'abc', null` | 201 for 1..100000; 400 for <=0 or non-numeric; 422 for >100000 or >balance |
| **Transfer Amount (VIP)** | `amount (INR)` | `250000 (nominal), 1 (min), 500000 (max)` | `500001 (max+1), 0, -1` | 201 for 1..500000; 422 DAILY_LIMIT_EXCEEDED for >500000 |
| **Idempotency Header** | `Idempotency-Key` | `idem-tx-001 (unique UUID/string)` | `Duplicate key same payload; Duplicate key changed amount` | 201 first call; 200 replay same payload; 409 mismatch |
| **Beneficiary IFSC** | `ifsc (11 chars)` | `HDFC0001234, SBIN0004567` | `HDFC000123 (10 chars), HDFC00012345 (12 chars), HDFC1001234 (5th!=0)` | 201 for valid 11-char regex; 400 INVALID_IFSC otherwise |
| **Pagination Params** | `page / limit` | `page=1, limit=10` | `page=0, page=-1, limit=0, page=999` | 200 with bounded items; 400 for negative/zero or safe default; empty array for page 999 |
| **Card Status** | `status` | `ACTIVE, FROZEN` | `DELETED, BLOCKED, '' (empty), null` | 200 for ACTIVE/FROZEN; 400 INVALID_CARD_STATUS otherwise |
| **Security Payloads** | `name / notes` | `Rohan Deshmukh / Monthly Rent` | `' OR 1=1 -- ; <script>alert(1)</script> ; broken JSON` | 400 Bad Request or sanitized string; zero stack traces leaked |
