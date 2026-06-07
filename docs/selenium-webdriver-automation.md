# 🌐 Selenium WebDriver Automation

> Generate Java Selenium 4 test code with thread-safe Page Object Model, explicit waits, TestNG/JUnit 5 integration, and screenshot-on-failure.

---

## What It Does

The Selenium WebDriver skill acts as a Java automation architect who knows Selenium 4 inside out. It generates complete, thread-safe framework code — `DriverManager`, `BasePage`, `BaseTest`, `TestListener`, and your feature's Page Objects and test classes — following production standards that avoid the common pitfalls that make Selenium suites flaky and unmaintainable.

---

## Key Advantages

### 🔒 Thread-safe by design
Uses `ThreadLocal<WebDriver>` in `DriverManager` — every test gets its own isolated driver instance, enabling true parallel execution without interference.

### ⏱️ Explicit waits only
Every generated interaction uses `WebDriverWait` + `ExpectedConditions`. The skill never generates `Thread.sleep()` or `ImplicitWait` — the two most common causes of Selenium flakiness.

### 📸 Screenshot on every failure
Generates a `TestListener` implementing `ITestListener.onTestFailure` that captures screenshots, timestamps them, and attaches them to Allure reports automatically.

### 🗂️ Page Object Model with PageFactory
All locators declared with `@FindBy` annotations, initialized via `PageFactory.initElements`. POMs are clean — no WebDriver calls in test classes, ever.

### 📊 Data-driven out of the box
Generates TestNG `@DataProvider` with parallel test data, covering valid, invalid, boundary, and injection-attempt scenarios in a single parameterized test.

### ⚙️ WebDriverManager — zero manual setup
No manual ChromeDriver downloads. `WebDriverManager.chromedriver().setup()` handles all binary management, including CI environments.

---

## What to Provide

| Input | Required? | Example |
|---|---|---|
| UI flow / feature description | ✅ Required | "Admin creates a user, assigns a role, verifies in the user list" |
| Test runner preference | 🔶 Helpful | "TestNG" (default) or "JUnit 5" |
| Page HTML or locator hints | 🔶 Helpful | Paste form HTML or element IDs from DevTools |
| Parallel execution needs | 🔶 Helpful | "Need 4 parallel threads on CI" |
| Browser targets | 🔸 Nice to have | "Chrome + Firefox" (default: Chrome) |
| Reporting tool | 🔸 Nice to have | "Allure", "ExtentReports" (default: Allure) |

---

## Coverage Areas

```
✅ DriverManager — ThreadLocal, WebDriverManager, browser-switching factory
✅ BasePage — explicit waits, scroll-to-element, page-load wait, stale-element retry
✅ Page Object Model — @FindBy + PageFactory, no raw driver calls in tests
✅ BaseTest — @BeforeMethod setup, @AfterMethod screenshot + quit
✅ TestListener — ITestListener screenshot-on-failure + Allure attachment
✅ Explicit waits — visibilityOf, elementToBeClickable, invisibilityOf, textPresent, attributeContains
✅ FluentWait — custom polling, ignore StaleElementReferenceException
✅ Data-driven — TestNG @DataProvider (parallel=true), JUnit 5 @ParameterizedTest
✅ Actions class — hover, right-click, double-click, drag-and-drop, keyboard combos
✅ JavaScript executor — edge-case clicks, hidden input values, scroll, attribute reads
✅ Alert / iFrame / window handling — complete switch patterns
✅ Selector stability guide — data-testid > id > name > aria > CSS[attr] > XPath last resort
✅ Maven Surefire config — testng.xml, system properties, parallel thread count
✅ CI integration — headless Chrome flags, env variable injection
```

---

## Setup

### Claude (Cowork / Desktop)

1. Download `selenium-webdriver-automation.skill`
2. Open Claude desktop → **Plugins → Install from file**
3. Select `.skill` file

**Trigger phrases:**
```
"write Selenium tests for this flow"
"Selenium WebDriver automation in Java"
"Page Object Model with Selenium"
"TestNG Selenium setup"
"parallel Selenium tests"
"Selenium Grid configuration"
```

### Cursor

1. Unzip `selenium-webdriver-automation.skill` → copy `selenium-webdriver-automation/SKILL.md`
2. In your Java project, create `.cursorrules`:

```
[paste SKILL.md content]
```

3. Open a test file or POM in Cursor, then in chat:
```
Generate a Page Object and TestNG test class for the login page.
Locators: email input id="email", password input id="password", submit button css="button[type='submit']"
```

**Cursor Composer for full framework generation:**
```
@SKILL.md
Generate the complete Selenium 4 framework:
- DriverManager with ThreadLocal
- BasePage with explicit waits
- BaseTest with TestNG setup/teardown
- TestListener with screenshot-on-failure
- LoginPage POM and LoginTest for [describe flow]
```

### GitHub Copilot (VS Code)

1. Unzip → open `selenium-webdriver-automation/SKILL.md`
2. Add to `.github/copilot-instructions.md`:

```markdown
# Selenium WebDriver Standards
[paste SKILL.md content]
```

3. In Copilot Chat with Java file open:
```
#file:.github/copilot-instructions.md

Generate a thread-safe Page Object for our checkout page.
The page has: [describe elements and locators]
```

**Copilot tip:** Open the generated test file and use `Ctrl+I` (inline chat) to ask Copilot to add a specific explicit wait or data provider — it will follow the skill's patterns from the instructions file.

---

## Generated File Structure

```
src/
├── main/java/framework/
│   ├── config/ConfigReader.java          ← reads test.properties
│   ├── driver/DriverManager.java         ← ThreadLocal WebDriver factory
│   ├── pages/
│   │   ├── BasePage.java                 ← shared wait helpers
│   │   └── [Feature]Page.java            ← POM per page
│   └── utils/
│       ├── WaitUtils.java                ← custom wait helpers
│       ├── ScreenshotUtils.java          ← capture + save
│       └── ApiTestDataHelper.java        ← REST calls for test data setup
├── test/java/tests/
│   ├── BaseTest.java                     ← @BeforeMethod / @AfterMethod
│   └── [Feature]Test.java                ← test class
└── test/resources/
    ├── test.properties                   ← base URL, timeouts, env
    └── testng.xml                        ← suite, parallel settings
```

---

## Example Session

**You:**
```
Generate Selenium tests for the user registration flow.
Flow: Visit /register → fill First Name, Last Name, Email, Password, Confirm Password → check Terms checkbox → click Register → verify dashboard URL.
Runner: TestNG
Also add data-driven tests covering: duplicate email, password mismatch, empty required fields, SQL injection in name fields.
```

**Skill output:**
- `RegistrationPage.java` — POM with all `@FindBy` locators, `register()` method returning `DashboardPage`
- `RegistrationTest.java` — TestNG class with `@DataProvider` covering all 4 negative cases + happy path
- `BaseTest.java` — `@BeforeMethod` opens browser, `@AfterMethod` screenshots on failure + quits
- `TestListener.java` — `ITestListener` with Allure attachment
- `testng.xml` — parallel="methods", thread-count=4

---

## Contributing to This Skill

See [CONTRIBUTING.md](../CONTRIBUTING.md). For this skill specifically:

- **Add JUnit 5 full example:** mirror the TestNG patterns with JUnit 5 `@ExtendWith`, `@MethodSource`, `TestWatcher`
- **Add Allure annotations:** `@Step`, `@Attachment`, `@Description` patterns throughout
- **Add Selenium Grid 4 config:** `RemoteWebDriver` setup with Docker Compose Grid
- **Add shadow DOM handling:** `JavascriptExecutor` patterns for shadow root traversal
- **Add Python/C# variants:** generate Python `pytest-selenium` or C# NUnit Selenium code
- **Improve selector guide:** add more examples of `data-testid` negotiation with dev teams
