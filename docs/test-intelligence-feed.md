# 🧠 Test Intelligence Feed

> Transform raw application knowledge into a risk-based test strategy, prioritized test plan, exploratory charters, and coverage gap analysis — before writing a single test.

---

## What It Does

The Test Intelligence Feed is the strategic brain of your testing workflow. Instead of jumping straight to writing tests, it forces a structured intake: collect everything known about the app, risk-score every functional area, and produce a prioritized plan that tells you exactly where to focus limited testing effort.

It applies the **OODA loop** (Observe → Orient → Decide → Act) continuously — gathering signals, building a risk model, making coverage decisions, and producing actionable output. For unfamiliar domains, it runs a **deep research phase** to learn the domain's business rules, compliance constraints, and common failure modes before generating any plan.

---

## Key Advantages

### 🎯 Risk-first, not feature-first
Scores every area by `Business Impact × Defect Probability` (1–5 each). This gives you a numeric priority — P0 (≥16) down to P3 (1–5) — so you spend time where bugs hurt most, not where the feature list is longest.

### 🔬 Deep research for unknown domains
If you're testing a payments system, healthcare app, or regulated financial product and don't know the domain's rules, the skill researches API docs, compliance standards (PCI-DSS, HIPAA, GDPR), and common failure modes — then feeds that knowledge into the test plan.

### 📊 Coverage gap analysis
Feed in your existing test suite. Get a structured gap checklist: state transitions untested, concurrent user scenarios missing, auth edge cases not covered, async job failures not simulated. Separated into Critical / High / Automation candidates / Defer.

### 📋 Exploratory test charters
For every P0 and P1 area not covered by automation, generates a structured charter: Area, Risk, Mission, Target flows, Oracles (how to recognize a bug), Duration, Priority.

### 🤖 Automation ROI scoring
For every test, calculates `(Execution Frequency × Risk Score) / Implementation Cost`. High ROI = automate first. Low ROI = keep manual or defer. Recommends which framework (RestAssured/Playwright/Selenium) per scenario.

### ✅ Release checklist output
Ends every plan with a concrete pre-release checklist: which automated gates must pass, which charters must run, which areas need spot-checking.

---

## What to Provide

| Input | Required? | Example |
|---|---|---|
| App description + core user flows | ✅ Required | "B2B SaaS order management. Core: create → approve → fulfill" |
| Recent changes / sprint scope | ✅ Required | Jira sprint items, release notes, PR titles |
| Defect history | 🔶 Helpful | "Most bugs appear in checkout and payment retry" |
| Architecture overview | 🔶 Helpful | "Microservices: Order, Payment, Inventory, Notification. Kafka events between them." |
| Existing test suite / coverage report | 🔶 Helpful | Jest coverage output, TestRail case count per area |
| Business rules and SLAs | 🔸 Nice to have | "Orders process in <2s. GDPR applies. PCI-DSS scope for payment pages." |

The more context you provide, the more accurate the risk scoring and the fewer assumptions the skill needs to make.

---

## The OODA Loop Applied to Testing

```
OBSERVE  → Collect: requirements, flows, defect history, architecture, team input
ORIENT   → Build: domain map, risk surface, defect density heatmap, dependency graph
DECIDE   → Prioritize: P0→P3 assignment, pyramid allocation, technique selection
ACT      → Output: risk matrix, test plan, charters, gap analysis, release checklist
```

The skill loops through OODA for each sprint or release — it's not a one-time exercise.

---

## Risk Scoring Model

```
Business Impact (B):    1=cosmetic  2=UX degraded  3=data issue  4=revenue  5=legal/safety
Defect Probability (P): 1=very stable  2=minor history  3=some bugs  4=frequent  5=known fragile

Risk Score = B × P

≥16 → P0 (release blocker if broken)
12–15 → P1 (must test before release)
6–11 → P2 (regression suite)
1–5  → P3 (exploratory, low frequency)
```

---

## Setup

### Claude (Cowork / Desktop)

1. Download `test-intelligence-feed.skill`
2. Open Claude desktop → **Plugins → Install from file**
3. Select the `.skill` file

**Trigger phrases:**
```
"build a test strategy for this sprint"
"prioritize my tests by risk"
"what should I test first before this release"
"analyze coverage gaps in my test suite"
"create test charters for exploratory sessions"
"feed app info for test planning"
"risk-based test plan"
```

**Deep research mode:**
```
"I need to test a PCI-DSS compliant payment flow — research the domain first then build the test plan"
```

### Cursor

1. Unzip `test-intelligence-feed.skill` → copy `test-intelligence-feed/SKILL.md`
2. In `.cursorrules` or a dedicated `TEST_STRATEGY.md` in your project:

```
[paste SKILL.md content]
```

3. In Cursor chat:
```
@TEST_STRATEGY.md
Here's our sprint scope: [paste Jira items]
Here's our app architecture: [describe]
Known defect-heavy areas: [describe]
Build a risk matrix and prioritized test plan.
```

**Tip:** Keep a `test-strategy/` folder in your repo and open it in Cursor when running planning sessions. The skill produces markdown output you can commit directly as your sprint test plan.

### GitHub Copilot (VS Code)

1. Unzip → open `test-intelligence-feed/SKILL.md`
2. Add to `.github/copilot-instructions.md`:

```markdown
# Test Strategy and Planning Standards
[paste SKILL.md content]
```

3. In Copilot Chat:
```
#file:.github/copilot-instructions.md

Sprint scope: [paste items]
Architecture: [describe]
What's our test priority for this release?
```

**Copilot tip:** After getting the risk matrix, use `@workspace` to ask Copilot to check which existing test files cover each P0 area — it will cross-reference your codebase.

---

## Output Structure

Every run produces these sections in order:

```
1. Intake Confirmation      — what was collected, what was assumed
2. Domain Intelligence      — (only if deep research ran) domain rules, compliance, failure modes
3. Risk Matrix              — scored table: area × B × P × priority
4. Test Pyramid Rec.        — % allocation per layer with rationale
5. Prioritized Test Cases   — P0→P3 table with technique and automation flag
6. Exploratory Charters     — one per P0/P1 area not covered by automation
7. Coverage Gap Analysis    — (only if existing suite provided) critical → high → defer
8. Release Checklist        — pre-release gates, charter requirements, spot-checks
```

---

## Example Session

**You:**
```
App: Multi-tenant SaaS project management tool. Teams create projects, assign tasks, track time.
Sprint changes: Added time tracking export (CSV/PDF), modified role permissions (new "Viewer" role), fixed bug in recurring task scheduler.
Defect history: Most bugs in permission checks and recurring task logic.
Architecture: Rails monolith + Sidekiq for background jobs + S3 for exports.
Existing tests: 847 unit tests, 120 integration tests, 45 E2E tests. No coverage of export or new Viewer role.
```

**Skill output:**
- Risk matrix: export (B=3, P=4, score=12→P1), permissions (B=5, P=5, score=25→P0), recurring tasks (B=4, P=4, score=16→P0)
- P0 charters: permissions regression, recurring task fix verification
- P1 test cases: CSV/PDF export correctness, encoding, large dataset
- Coverage gaps: Viewer role cross-data visibility untested (critical), Sidekiq job failure/retry untested (high)
- Release checklist: permissions E2E suite must pass, Viewer role charter must run, export integration tests must pass

---

## Contributing to This Skill

See [CONTRIBUTING.md](../CONTRIBUTING.md). For this skill specifically:

- **Improve risk scoring:** Add a third axis (e.g., User Frequency) for a 3D risk model
- **Add FMEA integration:** Formal Failure Mode and Effects Analysis table format
- **Add domain packs:** Pre-built domain intelligence for payments (PCI-DSS), healthcare (HIPAA), finance, e-commerce
- **Add defect prediction:** Patterns to identify defect-prone code from git blame, churn, and coupling metrics
- **Add sprint velocity integration:** Factor in team capacity when recommending test scope
- **Add test suite ROI analysis:** Given execution time and failure rate per test, recommend which tests to cut
