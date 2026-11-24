# 06. Postman Test Scripts & Newman Automation Guide — NexaAPI

Reusable **Postman Pre-Request** and **Post-Response (`pm.test`)** scripts included in `postman/NexaAPI_Functional_Suite.postman_collection.json`.

---

## 1. Pre-Request Script — Dynamic Idempotency Key Generator
Attach to `POST /api/v1/transfers` (first request) to generate a fresh UUID per run:

```javascript
const key = "idem-nexa-" + Date.now() + "-" + Math.floor(Math.random() * 10000);
pm.environment.set("idempotencyKey", key);
```

## 2. Post-Response Script — Auth Login & Token Chaining (`TC-003`)

```javascript
pm.test("Status code is 200 OK", () => {
    pm.response.to.have.status(200);
});

pm.test("Response time is under 500ms SLA", () => {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

pm.test("Content-Type is application/json", () => {
    pm.expect(pm.response.headers.get("Content-Type")).to.include("application/json");
});

pm.test("JWT token returned and saved to environment", () => {
    const json = pm.response.json();
    pm.expect(json).to.have.property("token").that.is.a("string").and.not.empty;
    pm.expect(json.user.role).to.eql("STANDARD");
    pm.environment.set("authToken", json.token);
});
```

## 3. Post-Response Script — Idempotent Replay Verification (`TC-027`)

```javascript
pm.test("Duplicate Idempotency-Key returns HTTP 200 OK", () => {
    pm.response.to.have.status(200);
});

pm.test("Original transferId replayed with idempotentReplay=true", () => {
    const json = pm.response.json();
    pm.expect(json.transferId).to.eql(pm.environment.get("lastTransferId"));
    pm.expect(json.idempotentReplay).to.eql(true);
});
```

## 4. Post-Response Script — Negative Validation Guard (`400` / `401` / `403` / `422`)

```javascript
pm.test("Error status matches expected negative code", () => {
    pm.expect(pm.response.code).to.be.oneOf([400, 401, 403, 409, 422]);
});

pm.test("Structured JSON error contract returned", () => {
    const err = pm.response.json();
    pm.expect(err).to.have.property("error").that.is.a("string");
    pm.expect(err).to.have.property("message").that.is.a("string");
});
```

## 5. Run Headlessly with Newman (CI/CD Ready)

```bash
newman run postman/NexaAPI_Functional_Suite.postman_collection.json \
  -e postman/NexaAPI_Local.postman_environment.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/nexaapi-postman-report.html
```
