# 🧪 SDET Automation Skills Suite

> Five AI-powered skills for SDETs and QA Engineers — from exploratory test design to production-ready automation code.

[![Skills](https://img.shields.io/badge/Skills-5-6c63ff?style=flat-square)](.)
[![Platforms](https://img.shields.io/badge/Platforms-Claude%20%7C%20Cursor%20%7C%20Copilot-00d4aa?style=flat-square)](.)
[![License](https://img.shields.io/badge/License-MIT-ffa94d?style=flat-square)](LICENSE)

---

## What Is This?

This repository contains **five AI skill files** that supercharge your testing workflow inside any AI coding assistant that supports custom skills or instructions. Each skill is a pre-loaded expert that knows exactly what questions to ask, what patterns to apply, and what code to generate — so you don't have to re-explain your standards every session.

---

## The 5 Skills

| Skill | File | Primary Use |
|---|---|---|
| 🔍 Exploratory Test Designer | `exploratory-test-designer.skill` | Generate non-obvious functional test cases from requirements |
| ⚡ API Testing — RestAssured | `api-restassured.skill` | Generate Java RestAssured test code from API contracts |
| 🎭 Playwright UI Automation | `playwright-ui-automation.skill` | Generate TypeScript Playwright E2E tests with POM |
| 🌐 Selenium WebDriver Automation | `selenium-webdriver-automation.skill` | Generate Java Selenium 4 tests with TestNG/JUnit 5 |
| 🧠 Test Intelligence Feed | `test-intelligence-feed.skill` | Risk-based test strategy, prioritization, and coverage gap analysis |

---

## Quick Start

Each skill ships as a `.skill` file (a zip archive containing a `SKILL.md`). See each skill's README for platform-specific setup:

- [🔍 Exploratory Test Designer](docs/exploratory-test-designer.md)
- [⚡ API Testing — RestAssured](docs/api-restassured.md)
- [🎭 Playwright UI Automation](docs/playwright-ui-automation.md)
- [🌐 Selenium WebDriver Automation](docs/selenium-webdriver-automation.md)
- [🧠 Test Intelligence Feed](docs/test-intelligence-feed.md)

---

## Recommended Workflow

```
1. 🧠 Test Intelligence Feed   → Risk map, P0–P3 priority, test pyramid allocation
2. 🔍 Exploratory Designer     → Edge cases and functional test cases for P0/P1 areas
3. ⚡ RestAssured              → Automate API layer (contract + chains + error handling)
4. 🎭 Playwright / 🌐 Selenium → Automate UI happy paths for P0 flows only
5. 🧠 Test Intelligence Feed   → Re-run gap analysis after automation is written
```

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) to improve existing skills or add new ones.

---

## License

MIT — use freely, improve openly.
