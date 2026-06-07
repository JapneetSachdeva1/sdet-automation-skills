# Contributing to SDET Automation Skills

Thank you for helping improve these skills. Contributions that sharpen the techniques, add new patterns, or improve AI trigger accuracy are highly valued.

---

## What Is a Skill?

Each skill is a `.skill` file — a zip archive containing one file: `SKILL.md`. The `SKILL.md` is a structured markdown document that an AI reads as its system-level instruction when the skill is active.

```
my-skill.skill  (zip)
└── my-skill/
    └── SKILL.md
```

The quality of the skill = the quality of the `SKILL.md`. Every improvement is a markdown edit.

---

## Contribution Types

### 1. Improve an Existing Skill

**Fix a technique:** Found a test pattern that the skill misses or gets wrong? Edit the relevant technique section in `SKILL.md`.

**Add a new technique:** Add a new numbered section following the existing structure:
- When to apply (condition)
- Test idea table or code examples
- Integration with existing output format

**Improve trigger accuracy:** The `description:` field in the SKILL.md frontmatter controls when the AI activates the skill. If it's triggering when it shouldn't (false positive) or missing obvious requests (false negative), edit the description and trigger phrase list.

**Improve code patterns:** For the automation skills (RestAssured, Playwright, Selenium), add new code blocks for patterns that come up frequently — new auth types, new wait strategies, new assertion patterns.

**Add examples:** Real before/after examples of input → output make skills dramatically more useful. Add them to the relevant skill README in `docs/`.

### 2. Add a New Skill

New skills are welcome if they cover a distinct SDET concern not handled by existing skills. Good candidates:
- Performance testing (k6, Gatling, JMeter)
- Contract testing (Pact)
- Mobile automation (Appium)
- Accessibility testing (axe-core, Pa11y)
- Security testing (OWASP ZAP, automated DAST)
- Visual regression (Percy, Applitools)
- Test data management

### 3. Improve Documentation

Each skill has a README in `docs/`. Improvements welcome:
- Better setup instructions for a specific platform version
- More realistic example sessions
- Troubleshooting common issues
- Video walkthroughs (link in README)

---

## How to Submit Changes

### Step 1: Fork and clone

```bash
git clone https://github.com/JapneetSachdeva1/sdet-automation-skills.git
cd sdet-automation-skills
```

### Step 2: Unpack the skill you want to edit

```bash
unzip skills/api-restassured.skill -d /tmp/edit-skill/
```

### Step 3: Edit `SKILL.md`

```bash
# Edit the extracted SKILL.md
code /tmp/edit-skill/api-restassured/SKILL.md
```

### Step 4: Repack

```bash
cd /tmp/edit-skill/
zip -r api-restassured.skill api-restassured/
cp api-restassured.skill /path/to/repo/skills/
```

### Step 5: Update the README

If you added a new technique or pattern, update the corresponding file in `docs/`.

### Step 6: Test your changes

Before submitting, test the updated skill:
1. Install the new `.skill` in Claude, Cursor, or Copilot
2. Run 3+ representative prompts — cover: a good input, a minimal input, and an edge-case input
3. Confirm the output quality improved and nothing regressed
4. Note your test prompts and outputs in the PR description

### Step 7: Open a Pull Request

```
Title: [skill-name] Brief description of change
Body:
- What was wrong or missing
- What you changed
- Test prompts used
- Before/after output comparison (paste or screenshot)
```

---

## SKILL.md Writing Standards

Follow these rules when editing or creating `SKILL.md` files:

### Frontmatter

```yaml
---
name: my-skill-name          # kebab-case, matches filename
description: >               # This is what the AI reads to decide whether to activate
  Trigger on [specific phrases]. Use when [specific situation].
  Do NOT trigger for [explicit exclusions].
---
```

The `description` is critical. Write it from the AI's perspective — it's the rule the AI uses to self-select the skill. Be specific about triggers. Include explicit exclusions to prevent false positives.

### Technique Sections

Each technique should have:
- **Name and when to apply** (1–2 sentences)
- **Test idea table** OR **code examples** (not both for small sections)
- **Common defects found** (the "why this matters")

### Code Examples

- All code must be syntactically correct and runnable
- Use `${PLACEHOLDER}` for values the user must supply
- Include imports if the pattern requires non-obvious dependencies
- No pseudocode — if it's in the skill, it must be real code

### Output Format Sections

Every skill must have a clear `## OUTPUT FORMAT` section specifying exactly what the AI should produce and in what order. This is what makes skill output consistent and predictable.

### Constraints Section

Every skill must have a `## CONSTRAINTS` section listing what the skill explicitly refuses to do. This prevents the AI from drifting into adjacent concerns.

---

## Skill Quality Checklist

Before submitting a new or updated skill, verify:

- [ ] Frontmatter `description` includes at least 5 specific trigger phrases
- [ ] Frontmatter `description` includes at least 1 explicit exclusion
- [ ] All code examples are syntactically correct
- [ ] `## CONSTRAINTS` section is present and specific
- [ ] `## OUTPUT FORMAT` section is present and structured
- [ ] `## INPUT REQUIREMENTS` section distinguishes required vs helpful inputs
- [ ] Tested with at least 3 representative prompts across Claude + (Cursor or Copilot)
- [ ] Corresponding `docs/` README updated

---

## Code of Conduct

- Be specific in reviews — "this pattern is wrong because X" not "this is bad"
- Test before claiming something is an improvement
- Respect that different teams have different tooling constraints — keep skills flexible
- Credit sources when a technique comes from a specific book, paper, or practitioner

---

## Questions?

Open a GitHub Discussion with the `question` label. For bugs in a specific skill's output, open an Issue with: the skill name, the prompt you used, and what was wrong with the output.
