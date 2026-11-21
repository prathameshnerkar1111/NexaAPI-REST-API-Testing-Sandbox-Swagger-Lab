# NexaAPI — REST API Testing Sandbox & Swagger Lab

A standalone, zero-dependency REST API testing sandbox and interactive Swagger-style lab built for manual API testing and automation practice with **Playwright (`APIRequestContext`)**, **REST Assured**, and **Postman**.

🔗 **Live Demo:** `https://prathameshnerkar1111.github.io/nexaapi-qa-sandbox/`

---

## ✨ Key Features

* **Swagger-Style Endpoint Explorer:** 13 pre-configured endpoints across **Auth**, **Users CRUD**, **Orders CRUD**, and **Status & Chaos** simulators with `localStorage` persistence.
* **Postman-Style Request Builder:** Edit query parameters, custom headers (including a 1-click `Idempotency-Key` generator), and JSON request bodies.
* **Response Inspector & Assertions:** View color-coded HTTP status codes, response latency, payload size, syntax-highlighted JSON, response headers, and automated assertion checks.
* **1-Click JWT Auth Injection:** Authenticate once and click **Use Token** to automatically attach the Bearer JWT to subsequent requests.
* **Live Code Generator:** Dynamically generates ready-to-run **Playwright TypeScript**, **REST Assured (Java)**, and **cURL** snippets for any active request.
* **Real Local HTTP Server (`server.js`):** Download a zero-dependency Node.js server directly from the UI to run real HTTP requests against `http://localhost:3000`.
* **Postman Collection v2.1 Export:** Download a pre-built `.json` collection and import it straight into Postman.
* **QA Chaos Bar:** Inject global latency (`0ms`–`3000ms`), force `429 Rate Limit` or `500 Server Error` faults, and monitor the active JWT role (`None` / `USER` / `ADMIN`).

---

## 🚀 Quick Start

### Option 1: Use Online (GitHub Pages)
1. Open the live URL in your browser.
2. Explore endpoints, send requests, and inspect responses using the built-in browser mock engine.

### Option 2: Run Real HTTP Server Locally (for Postman, REST Assured & Playwright)
1. Open the NexaAPI web app and click **Download `server.js`**.
2. Start the zero-dependency Node.js server in your terminal:
   ```bash
   node server.js
