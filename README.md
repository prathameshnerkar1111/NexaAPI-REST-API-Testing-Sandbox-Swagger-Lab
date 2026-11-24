# NexaAPI — Functional API Testing with Postman QA Documentation

Industry-level Functional API QA documentation suite (aligned with **IEEE 829 / ISO/IEC/IEEE 29119**) and importable **Postman Collection v2.1** for **NexaAPI — REST API Testing Sandbox & Swagger Lab**.

---

## Repository Structure

```text
├── docs/qa/
│   ├── README.md                              # QA Documentation Hub (this file)
│   ├── 01_API_Test_Plan.md                    # Master API Test Plan (Scope, Strategy, Entry/Exit, Risks)
│   ├── 02_API_Test_Scenarios.md               # 12 High-Level API Scenarios (TS-01 to TS-12)
│   ├── 03_Functional_API_Test_Cases.md        # 45 Detailed API Test Cases (TC-001 to TC-045)
│   ├── 04_Requirements_Traceability_Matrix.md # Bi-Directional RTM (REQ-API -> TS -> TC)
│   ├── 05_Test_Data_and_BVA_Matrix.md         # Personas, BVA & Negative/Security Payloads
│   ├── 06_Postman_Test_Scripts_Guide.md       # Reusable pm.test Scripts & Newman CLI Guide
│   ├── 07_Defect_Report_Template_and_Log.md   # Jira API Bug Template & Defect Log (BUG-401 to BUG-403)
│   └── 08_Test_Summary_and_SignOff_Report.md  # Test Closure Report & Go/No-Go Sign-Off
└── postman/
    ├── NexaAPI_Functional_Suite.postman_collection.json
    └── NexaAPI_Local.postman_environment.json
```

---

## Coverage Snapshot

| Metric | Count |
| :--- | :---: |
| **API Functional Requirements** | 10 (`REQ-API-01` – `REQ-API-10`) |
| **High-Level API Scenarios** | 12 (`TS-01` – `TS-12`) |
| **Total Functional API Test Cases** | 45 (`TC-001` – `TC-045`) |
| **Positive / Negative / Edge / Security** | 16 / 16 / 8 / 5 |
| **Priority Breakdown (P0 / P1 / P2)** | 19 / 21 / 5 |
| **Logged API Review Defects** | 3 (`BUG-401` – `BUG-403`) |

---

## Quick Start with Postman & Newman

1. Open **Postman** -> click **Import** -> select both JSON files from the `postman/` folder.
2. Select the **NexaAPI — Local / Sandbox** environment in the top-right dropdown and verify `baseUrl` (default: `http://localhost:3000`).
3. Run the entire collection via **Collection Runner**, or execute headlessly via **Newman**:

```bash
npm install -g newman newman-reporter-htmlextra
newman run postman/NexaAPI_Functional_Suite.postman_collection.json \
  -e postman/NexaAPI_Local.postman_environment.json \
  -r cli,htmlextra --reporter-htmlextra-export reports/nexaapi-report.html
```
