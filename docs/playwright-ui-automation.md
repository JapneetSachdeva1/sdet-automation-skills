# 🎭 Playwright UI Automation

> Generate TypeScript Playwright E2E tests with Page Object Models, auth fixtures, network interception, and zero-flakiness guarantees.

---

## What It Does

The Playwright skill acts as a senior SDET who lives and breathes Playwright. It generates complete TypeScript test suites — `playwright.config.ts`, Page Object Models, auth fixtures, and test files — following Playwright's recommended patterns and locator priority rules.

Every generated test is engineered to be stable: no `waitForTimeout`, no CSS class selectors, no hardcoded delays. It uses Playwright's built-in auto-waiting and event-based synchronization throughout.

---

## Key Advantages

### 🚀 Zero `waitForTimeout`
All synchronization uses Playwright's event-driven APIs: `waitForResponse`, `waitForLoadState`, `waitForURL`, `waitForFunction`. The skill refuses to generate timing-based waits.

### 🔒 Auth state reuse
Generates a `globalSetup` that logs in once and saves `storageState` to disk. Every test reuses the saved session — no re-login overhead per test, no auth flakiness.

### 🌐 Network interception built-in
Every generated test suite includes patterns for: mocking API responses, asserting request payloads, simulating API failures (503, timeout), and verifying no unwanted requests were made.

### 📱 Cross-browser and mobile ready
`playwright.config.ts` generated with Chromium, Firefox, WebKit, Pixel 7, and iPhone 14 projects. Workers and retry counts tuned for CI automatically.

### 🎯 Locator hierarchy enforced
`getByRole` → `getByLabel` → `getByTestId` → `getByText` → CSS `[data-*]`. Never generated XPath or dynamic class selectors unless absolutely no alternative exists.

### 🖼️ Visual snapshot testing
Generates `toHaveScreenshot()` calls with dynamic region masking (timestamps, avatars, user-specific content) so snapshots don't break on every run.

---

## What to Provide

| Input | Required? | Example |
|---|---|---|
| User flow / feature description | ✅ Required | "User logs in, adds item to cart, applies coupon, checks out" |
| Auth mechanism | 🔶 Helpful | "Email/password form", "SSO via Google", "JWT cookie" |
| App tech stack | 🔶 Helpful | "React SPA", "Next.js", "Angular", "Vue" |
| Existing selectors or data-testid attributes | 🔶 Helpful | Paste component HTML or dev tools snapshot |
| Target browsers | 🔸 Nice to have | "Chrome + Firefox + mobile Safari" (default: Chromium only) |
| CI environment | 🔸 Nice to have | "GitHub Actions", "GitLab CI" (affects reporter config) |

---

## Coverage Areas

```
✅ Page Object Model — lazy locator getters, no stale element issues
✅ Auth fixtures — storageState per role (user, admin, guest)
✅ Network interception — mock responses, assert payloads, simulate failures
✅ State-based UI — loading states, optimistic UI, error state recovery
✅ Form testing — validation messages, tab order, paste behavior, autocomplete
✅ Multi-tab / popup handling — waitForEvent('page'), waitForEvent('popup')
✅ Drag and drop — dragTo API
✅ File upload / download — setInputFiles, waitForEvent('download')
✅ Visual snapshots — toHaveScreenshot with dynamic masking
✅ Accessibility — axe-core/playwright integration
✅ Cross-browser config — chromium + firefox + webkit + mobile viewports
✅ Anti-flakiness — no waitForTimeout, event-based sync throughout
✅ CI config — headless, retries, trace/screenshot/video on failure
```

---

## Setup

### Claude (Cowork / Desktop)

1. Download `playwright-ui-automation.skill`
2. Open Claude desktop → **Plugins → Install from file**
3. Select the `.skill` file

**Trigger phrases:**
```
"write Playwright tests for this flow"
"Playwright E2E automation"
"create Page Object Model with Playwright"
"automate this React UI with Playwright"
"cross-browser test setup Playwright"
```

### Cursor

1. Unzip `playwright-ui-automation.skill` → copy `playwright-ui-automation/SKILL.md`
2. For a Playwright project, add to `.cursorrules`:

```
[paste SKILL.md content]
```

3. In Cursor chat: `Write Playwright tests for [describe flow]`

**Recommended:** Create a `playwright/` folder first and open it in Cursor — the skill generates into the correct structure automatically.

**For Cursor Composer (multi-file generation):**
```
@SKILL.md
Generate the full Playwright test suite for our checkout flow.
Flow: [describe steps]
Auth: [describe login method]
Target browsers: Chrome + Firefox
```
Cursor Composer will create all files (config, fixtures, POMs, tests) in one shot.

### GitHub Copilot (VS Code)

1. Unzip → open `playwright-ui-automation/SKILL.md`
2. In `.github/copilot-instructions.md`:

```markdown
# Playwright Testing Standards
[paste SKILL.md content]
```

3. In Copilot Chat:
```
#file:.github/copilot-instructions.md

Generate Playwright tests for the following user flow:
[describe flow]
```

**Tip for Copilot:** After generating tests, use `@workspace /fix` with Copilot to fix any TypeScript type errors against your actual Playwright version.

---

## Generated File Structure

```
playwright/
├── playwright.config.ts          ← browsers, baseURL, retries, reporters
├── fixtures/
│   ├── auth.fixture.ts           ← per-role storageState fixtures
│   └── test-data.fixture.ts      ← seeded test data fixtures
├── pages/
│   ├── base.page.ts              ← BasePage with shared helpers
│   └── [Feature].page.ts         ← one POM per page/component
├── tests/
│   └── [feature]/
│       └── [feature].spec.ts     ← test file per feature
└── utils/
    ├── api-helper.ts             ← REST calls for setup/teardown
    └── assertions.ts             ← custom expect extensions
```

---

## Example Session

**You:**
```
Automate the checkout flow for our React e-commerce app.
Flow: User logs in → browses products → adds to cart → applies coupon → pays → sees confirmation.
Auth: Email/password form at /login, JWT stored in httpOnly cookie.
The payment form is inside an iframe (Stripe).
Target: Chrome + mobile Chrome.
```

**Skill output:**
- `playwright.config.ts` with Chrome + mobile Chrome projects
- `fixtures/auth.fixture.ts` with `storageState` reuse
- `pages/login.page.ts`, `pages/product.page.ts`, `pages/cart.page.ts`, `pages/checkout.page.ts`
- `tests/checkout/checkout.spec.ts` with network interception for Stripe, iframe switching, payment mock, confirmation assertion
- Anti-flakiness notes for Stripe iframe timing

---

## Contributing to This Skill

See [CONTRIBUTING.md](../CONTRIBUTING.md). For this skill specifically:

- **Add component testing patterns:** Playwright Component Test (CT) for React/Vue/Angular components
- **Add API testing integration:** `request` fixture patterns for API-level assertions alongside UI
- **Add trace viewer workflow:** guidance on reading Playwright traces for debugging failures
- **Add Allure/HTML reporter config:** extend the config section
- **Add Python variant:** generate Python Playwright tests when user specifies Python
- **Improve mobile patterns:** device emulation, touch events, orientation change testing
