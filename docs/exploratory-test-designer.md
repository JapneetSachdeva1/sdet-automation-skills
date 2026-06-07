# 🔍 Exploratory Test Designer

> Generate non-obvious, high-value functional test cases from requirements using 17 advanced testing techniques.

---

## What It Does

The Exploratory Test Designer is an AI skill that acts as a senior QA engineer with deep expertise in structured exploratory testing. When you feed it requirements, user stories, or acceptance criteria, it produces a comprehensive set of functional test cases that go far beyond basic happy-path and boundary-value tests.

It applies 17 techniques systematically — only the ones relevant to your feature — and flags every ambiguity before generating a single test case.

---

## Key Advantages

### 🎯 Non-obvious coverage
Finds the tests that humans consistently miss: state corruption during interruptions, cascading side effects after data changes, cross-role data leakage, async job failure recovery, and UI/API/cache inconsistencies.

### 🚫 No duplication
Always asks for your existing tests. Never regenerates what you already have.

### ❓ Blocks on ambiguity
If a business rule, system limit, or role definition is unclear, it lists blocking questions and waits — rather than generating tests based on wrong assumptions.

### 🏗️ Structured output
Every test case includes: Functionality, Test name, Steps, Data, Expected Outcome, and Criticality (High / Medium / Low). Ready to paste into TestRail, Jira, or your test management tool.

### 📋 Technique coverage summary
At the end of every session it tells you exactly which of the 17 techniques it applied, how many tests each produced, and which were skipped and why.

---

## The 17 Techniques

| # | Technique | When Applied |
|---|---|---|
| 1 | State-Based Testing | Multi-step flows, sessions, transient states |
| 2 | Advanced CRUD | Any data entity (not basic create/read/update/delete) |
| 3 | Zero / One / Many | Any list, collection, count, or relationship |
| 4 | Beginning / Middle / End | Ordered lists, position-sensitive operations |
| 5 | Follow the Data | Data flowing through multiple views/services |
| 6 | Decision Table | 2+ conditions producing different outcomes |
| 7 | Pairwise / Combinatorial | Forms or configs with 3+ independent parameters |
| 8 | Cross-Role | Multiple user roles or permission levels |
| 9 | Async / Background Jobs | Background processing, webhooks, scheduled tasks |
| 10 | Time-Based | Expiry, schedules, timezones, date-dependent behavior |
| 11 | Import / Export | Data import, export, or file-based transfer |
| 12 | Notification Side-Effects | Actions triggering email, SMS, push, or webhook |
| 13 | Data Flow & Propagation | Computed fields, caches, indexes, aggregates |
| 14 | Integration Failure | Third-party APIs, payment gateways, external services |
| 15 | Test Heuristics (Misuse) | Double-submit, multi-tab, replay, back button |
| 16 | CAROL-G | Error handling quality: Clarity, Actionability, Recovery, On-Time, Logging, Gracefulness |
| 17 | RCRCRC (Regression) | Only for modified or repaired functionality |

---

## What to Provide

| Input | Required? | Example |
|---|---|---|
| Requirements / user stories / acceptance criteria | ✅ Required | Jira ticket, PRD section, Gherkin scenarios |
| Existing test cases | 🔶 Helpful | Test suite export, TestRail cases |
| User roles in the system | 🔶 Helpful | Admin, User, Guest, Moderator |
| Business rules and limits | 🔶 Helpful | Max 500 items, orders expire after 24h |

---

## Setup

### Claude (Cowork / Desktop)

1. Download `exploratory-test-designer.skill`
2. Open Claude desktop app → **Plugins** → **Install from file**
3. Select the `.skill` file
4. Done — the skill activates automatically when you mention test cases, edge cases, or test design

**Trigger phrases:**
```
"generate test cases for this feature"
"what should we test here"
"find edge cases"
"create test scenarios"
"test this user story"
```

### Cursor

1. Extract the `.skill` file (it's a zip) → open `SKILL.md`
2. Copy the full content of `SKILL.md`
3. In Cursor: **Settings → Rules for AI** (or `.cursorrules` in project root)
4. Paste the content, or add as a project-level instruction
5. In chat: `@exploratory-test-designer generate tests for [paste requirements]`

**Alternatively** — add as a Cursor Doc:
1. **Cursor Settings → Features → Docs → Add new doc**
2. Paste the raw `SKILL.md` content
3. Reference with `@exploratory-test-designer` in any chat

### GitHub Copilot (VS Code)

1. Extract `SKILL.md` from the `.skill` zip
2. Create `.github/copilot-instructions.md` in your repo (or append to existing)
3. Paste the `SKILL.md` content
4. In Copilot Chat: `#file:.github/copilot-instructions.md` then describe your requirements

**Or use Copilot Workspace instructions:**
```
# In .github/copilot-instructions.md
[paste full SKILL.md content here]
```

---

## Example Session

**You:**
```
Generate test cases for this feature:
- Users can invite team members by email
- Invited users receive an email with a 7-day expiry link
- Admin can revoke pending invites
- Only admins can invite; regular users see the button greyed out
```

**Skill output:**
- Blocking question: "What happens if the same email is invited twice while the first invite is still pending?"
- After answer: 40+ test cases across State-Based, Cross-Role, Time-Based, Notification Side-Effects, CAROL-G, and Misuse techniques
- Technique coverage summary at end

---

## Contributing to This Skill

See [CONTRIBUTING.md](../CONTRIBUTING.md) for the full guide. For this skill specifically:

- **Add a new technique:** Add a numbered section following the existing pattern (name, when to apply, test idea table, code examples if applicable)
- **Improve CAROL-G:** Add more error-handling anti-patterns with test cases
- **Add domain-specific patterns:** e.g., e-commerce-specific CRUD patterns, healthcare-specific state machines
- **Improve the ambiguity protocol:** Add new gap types and their handling rules
- **Calibrate criticality:** Improve the High/Medium/Low assignment logic
