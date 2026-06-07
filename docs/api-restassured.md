# ⚡ API Testing — RestAssured

> Generate production-ready Java RestAssured test code from API contracts, OpenAPI/Swagger specs, or endpoint descriptions.

---

## What It Does

The RestAssured skill acts as a senior API automation engineer who knows RestAssured, Hamcrest, JSON Schema validation, WireMock, and Java testing best practices deeply. Feed it an API contract or endpoint description and it produces complete, runnable test code — not pseudocode, not stubs, actual production-quality Java.

It covers the full breadth of API test concerns: contract validation, every meaningful HTTP status code, auth flows, pagination, concurrency, error response structure, rate limiting, microservice chaining, and integration failure simulation.

---

## Key Advantages

### 📄 Contract-first validation
Validates every response field against the contract: data type, presence/absence, null vs empty vs missing, enum membership, date format compliance. Uses `matchesJsonSchemaInClasspath()` for full schema validation.

### 🏗️ Production structure from day one
Generates `RequestSpecification` base configs, a `ApiTestConfig` class, JSON Schema files, and test classes — the complete structure your CI pipeline needs.

### 🔐 All auth patterns covered
Bearer token, Basic auth, OAuth2 client credentials (token refresh in `@BeforeAll`), API Key header. Generates token expiry tests and wrong-scope tests automatically.

### 🔗 Microservice chain tests
Models end-to-end flows across multiple APIs: Create in Service A → verify visible in Service B → trigger fulfillment → assert status propagated.

### ☠️ Integration failure with WireMock
Stubs external dependencies to return 500, timeout, or malformed responses — tests your error handling without needing real failure conditions.

### ⚡ Never leaks internals
Every error response test checks that stack traces, `SQLException`, `NullPointerException`, and internal paths are NOT present in the response body.

---

## What to Provide

| Input | Required? | Example |
|---|---|---|
| API contract / OpenAPI YAML / endpoint description | ✅ Required | Swagger URL, pasted OpenAPI YAML, endpoint docs |
| Auth mechanism | 🔶 Helpful | "Bearer token", "OAuth2 client credentials", "API Key in X-API-Key header" |
| Base URL / environment | 🔶 Helpful | `https://api.staging.myapp.com` |
| Test runner preference | 🔶 Helpful | "TestNG" or "JUnit 5" (default: TestNG) |
| Microservice dependencies | 🔸 Nice to have | "Order service calls Payment API and Inventory API" |
| Expected error codes and messages | 🔸 Nice to have | "Returns 409 DUPLICATE_ORDER on double submit" |

---

## Coverage Areas

```
✅ Contract validation (schema + field types + enum values + date formats)
✅ HTTP status code matrix (200/201/204/400/401/403/404/405/409/413/415/429/500)
✅ Request header tests (missing auth, expired token, wrong Content-Type, wrong Accept)
✅ Request body tests (missing required fields, null vs empty, extra unknown fields, injection)
✅ Idempotency (GET idempotent, DELETE→second 404 not 500, POST with Idempotency-Key)
✅ Pagination (no overlap between pages, last page partial, beyond-last empty, invalid params)
✅ Sorting and filtering (sort correctness, filter accuracy, compound filters, invalid params)
✅ Request chaining (multi-service end-to-end flows)
✅ Concurrency (race conditions, deterministic responses under load)
✅ Integration failure (WireMock stubs for 500/timeout/malformed from dependencies)
✅ Rate limiting (429 response, Retry-After header present)
✅ Response headers (Content-Type, X-Request-Id, Cache-Control, security headers)
✅ Error response structure (code + message + field + requestId, no leaked internals)
```

---

## Setup

### Claude (Cowork / Desktop)

1. Download `api-restassured.skill`
2. Open Claude desktop → **Plugins → Install from file**
3. Select the `.skill` file
4. Skill auto-activates on API testing requests

**Trigger phrases:**
```
"write RestAssured tests for this endpoint"
"generate API tests from this OpenAPI spec"
"test this microservice API"
"validate this API contract"
"automate this REST API"
```

### Cursor

1. Unzip `api-restassured.skill` → copy `api-restassured/SKILL.md` content
2. Create or open `.cursorrules` in your Java project root
3. Paste the content (or add to existing rules)
4. In Cursor chat: `Generate RestAssured tests for this endpoint: [paste endpoint details]`

**For project-specific use:**
```
# .cursorrules
[paste SKILL.md content — the AI will follow all code patterns and constraints]
```

**For one-off use:**
1. Open `SKILL.md` in Cursor editor
2. In chat: `@SKILL.md Generate RestAssured tests for [paste OpenAPI YAML]`

### GitHub Copilot (VS Code)

1. Unzip `api-restassured.skill` → open `api-restassured/SKILL.md`
2. Add to `.github/copilot-instructions.md`:

```markdown
# API Testing Standards
[paste SKILL.md content]
```

3. In Copilot Chat:
```
#file:.github/copilot-instructions.md

Here is our OpenAPI spec: [paste YAML]
Generate the full RestAssured test suite.
```

**Tip:** Pin `copilot-instructions.md` as a Copilot Workspace instruction so it applies to every chat in the repo.

---

## Generated File Structure

```
src/
├── test/java/
│   ├── config/
│   │   └── ApiTestConfig.java          ← base spec, auth setup, response spec
│   └── [Feature]ApiTest.java           ← complete test class
└── test/resources/
    └── schemas/
        └── [endpoint]-response.json    ← JSON Schema for contract validation
```

---

## Example Session

**You:**
```
Here's our OpenAPI spec for the Orders endpoint:
POST /orders - creates an order, requires Bearer auth
Body: { productId: string (required), qty: integer (required, min: 1), couponCode: string (optional) }
Response 201: { id: string, status: "PENDING"|"CONFIRMED", total: number, createdAt: ISO8601 }
Auth: Bearer token via OAuth2 client credentials
We use TestNG.
```

**Skill output:**
- `ApiTestConfig.java` with token refresh in `@BeforeAll`
- `schemas/create-order-response.json` — full JSON Schema
- `OrdersApiTest.java` — 25+ tests covering: schema validation, all status codes, missing/null fields, enum validation, date format, idempotency key, concurrent submissions, WireMock for payment failure

---

## Contributing to This Skill

See [CONTRIBUTING.md](../CONTRIBUTING.md). For this skill specifically:

- **Add new auth patterns:** SAML, mTLS, Digest auth — add to §AUTH TOKEN REFRESH PATTERN
- **Add new microservice patterns:** gRPC transcoding, GraphQL-over-REST, SSE — add under §REQUEST CHAINING
- **Improve WireMock patterns:** more fault scenarios (partial body, TCP disconnect, jitter)
- **Add Kotlin DSL variant:** generate Kotlin RestAssured tests when user specifies Kotlin
- **Add contract drift detection:** test to assert live API still matches saved OpenAPI spec
