# HTSM Testing Toolkit

### A Testing Assistant for GitHub Copilot and Claude Code

This toolkit turns **GitHub Copilot Chat** (VS Code) or **Claude Code** into a structured, systematic testing assistant grounded in the **Heuristic Test Strategy Model (HTSM)** by James Bach (v6.0). It helps you analyse features, identify risks, predict bugs, generate test ideas, and plan API testing — all through a shared set of prompt files and slash commands.

---

## Prerequisites

**GitHub Copilot**

- **VS Code** with the **GitHub Copilot** extension installed and active
- A Copilot subscription (Individual, Business, or Enterprise)

**Claude Code**

- [Claude Code](https://code.claude.com/) installed and authenticated
- Run `claude` from your project root (where the toolkit files live)

**Both**

- Copy the toolkit folders into the **root** of your project (see Installation below)
- Add `.testing/context.template.md` to capture feature context once per session and speed up tool workflows.

---

## Installation

### GitHub Copilot (VS Code)

1. Copy the toolkit into the root of your project:

```
your-project/
├── .github/
│   ├── README.md
│   ├── WORKFLOW.md
│   ├── copilot-instructions.md         ← always-on base assistant
│   ├── htsm/                           ← shared HTSM reference (single source of truth)
│   │   ├── README.md
│   │   ├── test-techniques.md
│   │   ├── product-elements.md
│   │   ├── quality-criteria.md
│   │   └── project-environment.md
│   └── prompts/
│       ├── risk-analysis.prompt.md
│       ├── bug-predictor.prompt.md
│       ├── test-ideas.prompt.md
│       ├── api-testing.prompt.md
│       └── test-plan.prompt.md
```

2. Enable instruction files in VS Code settings:
   - Open Settings with `Ctrl+,` (Windows/Linux) or `Cmd+,` (Mac)
   - Search for **"instruction file"**
   - Check **Code Generation: Use Instruction Files**

3. Open Copilot Chat with `Ctrl+Alt+I` (Windows/Linux) or `Ctrl+Cmd+I` (Mac)

4. Verify the base assistant is active — you'll see `copilot-instructions.md` listed in the References section of any Copilot response.

### Claude Code

1. Copy the same `.github/` folder **plus** the Claude Code config:

```
your-project/
├── CLAUDE.md                           ← always-on base assistant (symlink or copy of copilot-instructions.md)
├── .claude/
│   └── skills/
│       ├── risk-analysis/SKILL.md
│       ├── bug-predictor/SKILL.md
│       ├── test-ideas/SKILL.md
│       ├── api-testing/SKILL.md
│       └── test-plan/SKILL.md
└── .github/
    └── ...                             ← same as Copilot install above
```

2. Start Claude Code from your project root:

```bash
cd your-project
claude
```

4. The toolkit includes dedicated Claude Code skill files under `.claude/skills/` for each prompt. Each `SKILL.md` wrapper reads the corresponding `.github/prompts/*.prompt.md` file, so Claude uses the same HTSM instructions as GitHub Copilot.

5. Pass saved session files as command arguments or @-mention them in chat for chained workflows. Example:

```bash
/bug-predictor .testing/risk-login.md
```

3. Verify the base assistant is active — `CLAUDE.md` loads automatically at session start. Type `/` to see the specialist skills (`/risk-analysis`, `/bug-predictor`, etc.).

> **Windows note:** If symlinks are not supported, copy `.github/copilot-instructions.md` to `CLAUDE.md` in the project root instead of using the symlink.

---

## The Toolkit at a Glance

| File                                    | Copilot          | Claude Code      | Purpose                                                                     |
| --------------------------------------- | ---------------- | ---------------- | --------------------------------------------------------------------------- |
| `copilot-instructions.md` / `CLAUDE.md` | Always active    | Always active    | Base testing assistant — general help, feature analysis, ad-hoc questions   |
| `prompts/risk-analysis.prompt.md`       | `/risk-analysis` | `/risk-analysis` | Identify and prioritise risks before testing begins                         |
| `prompts/bug-predictor.prompt.md`       | `/bug-predictor` | `/bug-predictor` | Generate specific, falsifiable bug hypotheses                               |
| `prompts/test-ideas.prompt.md`          | `/test-ideas`    | `/test-ideas`    | Generate a prioritised, technique-grounded list of test ideas               |
| `prompts/api-testing.prompt.md`         | `/api-testing`   | `/api-testing`   | Full API test plan covering functional, security, contract, and performance |
| `prompts/test-plan.prompt.md`           | `/test-plan`     | `/test-plan`     | Consolidate all session outputs into one clean, shareable testing brief     |

---

## The Foundation — HTSM

Every tool in this toolkit is grounded in the **Heuristic Test Strategy Model** by James Bach. HTSM organises testing thinking around four interconnected dimensions:

- **Project Environment** — resources, constraints, schedule, team, tools, and mission
- **Product Elements** — structure, function, data, interfaces, platform, operations, and time
- **Quality Criteria** — capability, reliability, usability, security, performance, scalability, compatibility, installability, charisma, and development
- **General Test Techniques** — function, claims, domain, user, stress, risk, flow, scenario, and tool-supported testing

All output from every tool is labelled with the HTSM dimension it comes from, so you always know _why_ something is being flagged, not just _what_ it is.

---

## How Each Tool Works

### 🔵 Base Assistant — `copilot-instructions.md`

**Always active.** No invocation needed — it loads automatically in every Copilot Chat session.

**What it does:**

- Answers general testing questions grounded in HTSM
- Analyses features and requirements using Product Elements and Quality Criteria
- Guides you through test planning at any stage of a project
- Suggests which prompt tool to use for deeper work

**Mandatory workflow:**
Every session follows three rounds of questions before any output is generated:

1. **Round 1** — Feature basics (name, description, users, testing goal)
2. **Round 2** — Product detail (platform, inputs/outputs, dependencies, specs)
3. **Round 3** — Context and risks (history, constraints, worst-case scenarios)

A **Readiness Check** summarises understanding before analysis begins. This cannot be skipped — the tool will politely decline to proceed without it.

**Use it for:** Ad-hoc questions, coverage analysis, feature breakdowns, and as a starting point before deciding which specialist tool to use.

---

### 🔴 Risk Analysis — `risk-analysis.prompt.md`

**Invoke with:** `/risk-analysis`

**What it does:**
Systematically analyses a feature or product area for risks _before_ testing begins. Uses all 10 HTSM Quality Criteria and all 7 Product Elements as lenses to identify where and what could fail.

**Output:**

- A structured **Risk Register** table with: risk description, Quality Criterion, Product Element, Likelihood, Impact, Priority (🔴🟡🟢), and suggested test approach
- A **Top Risks** summary paragraph naming the 3–5 most important risks to tackle first
- A **CHAIN OUTPUT block** for passing context forward to downstream tools

**Three question rounds cover:**

1. Feature name, description, users, platform
2. Inputs/outputs, dependencies, new vs existing, specs
3. Past bugs, release pressure, worst-case failures, constraints

**Best used as the first step** in any testing session — establish the risk landscape before predicting bugs or generating tests.

**Feeds into:** `/bug-predictor` or `/test-ideas`

---

### 🐛 Bug Predictor — `bug-predictor.prompt.md`

**Invoke with:**

- Copilot: `/bug-predictor` or `/bug-predictor #file:.testing/risk-[feature].md`
- Claude Code: `/bug-predictor .testing/risk-[feature].md` or `/bug-predictor` with `@.testing/risk-[feature].md`

**What it does:**
Generates specific, falsifiable bug hypotheses — concrete predictions of what _might_ be broken and under exactly what conditions. This is a different mental mode from risk analysis: risks are categories of concern, bug predictions are specific failure statements of the form _"If [condition], then [specific failure], producing [observable symptom]."_

**Output:**

- A **Bug Predictions table** split by confidence: 🔴 High, 🟡 Medium, 🟢 Low
  - Each row: Bug Hypothesis | Trigger Condition | Expected Symptom | HTSM Area | Confidence
- **Top 5 predictions** to investigate first, with justification
- **Patterns and Root Cause Hypotheses** — clusters of bugs that might share a common cause
- **What Would Prove These Wrong** — how to rule out each top prediction
- A **CHAIN OUTPUT block** for passing forward to `/test-ideas`

**Three question rounds cover:**

1. Feature name, description, users, platform
2. Inputs/outputs, dependencies, new vs existing/refactor, specs/code
3. Tech stack, past bugs, recent changes, worst-case failures, suspicious patterns

**CHAIN INPUT:** Pass a `/risk-analysis` output file via `#file:` (Copilot) or as a file path argument / `@` mention (Claude Code). The tool reads the CHAIN OUTPUT block, skips already-answered questions, and focuses predictions on the top-priority risks identified upstream.

**Feeds into:** `/test-ideas`

---

### 🧪 Test Ideas — `test-ideas.prompt.md`

**Invoke with:**

- Copilot: `/test-ideas` or `/test-ideas #file:.testing/bugs-[feature].md`
- Claude Code: `/test-ideas .testing/bugs-[feature].md` or `/test-ideas` with `@.testing/bugs-[feature].md`

**What it does:**
Generates a concrete, actionable, prioritised list of test ideas by applying all **9 HTSM General Test Techniques** systematically to the specific feature. Every test idea is labelled with its technique, Quality Criterion, and recommended execution mode (Manual / Automated / Either).

**The 9 techniques applied:**
| Technique | Core question |
|---|---|
| Function Testing | Does each function do what it should — and nothing it shouldn't? |
| Claims Testing | Does the product deliver on every explicit and tacit claim? |
| Domain Testing | What boundary, typical, invalid, and combined data values matter? |
| User Testing | What does each user type do, and what do they value? |
| Stress Testing | What breaks under overload, large data, or constrained resources? |
| Risk Testing | What tests would directly reveal the top known risks? |
| Flow Testing | What happens across multi-step journeys and repeated actions? |
| Scenario Testing | What compelling real-world stories could expose failures? |
| Tool-Supported Testing | Where would automation, data generators, or proxy tools extend coverage? |

**Output:**

- A **Test Ideas table** split by priority: 🔴 High, 🟡 Medium, 🟢 Low
  - Each row: Test Idea | Technique | Quality Criterion | Manual/Automated
- **2–3 full Scenario Tests** in narrative form for exploratory sessions
- A **Coverage Gaps** section identifying untested Product Elements and missing information
- A **CHAIN OUTPUT block** carrying the full chain lineage forward

**Three question rounds cover:**

1. Feature name, description, users, testing goal
2. Platform, inputs/outputs, dependencies, upstream risk/bug files, specs
3. Fragile areas, problematic usage patterns, constraints, worst-case outcomes

**CHAIN INPUT:** Pass a `/bug-predictor` or `/risk-analysis` output file via `#file:` (Copilot) or file path / `@` mention (Claude Code). The tool reads the CHAIN OUTPUT block, skips answered questions, and ensures every high-confidence bug prediction and high-priority risk is directly addressed by at least one high-priority test idea.

**This is the last specialist tool in the chain** — its output is ready to execute as a testing session.

---

### 🔌 API Testing — `api-testing.prompt.md`

**Invoke with:**

- Copilot: `/api-testing` or `/api-testing #file:.testing/risk-[api].md`
- Claude Code: `/api-testing .testing/risk-[api].md` or `/api-testing` with `@.testing/risk-[api].md`

**What it does:**
Produces a comprehensive, multi-section API test plan covering all four critical dimensions in a single session. Works for REST, GraphQL, gRPC, and WebSocket APIs.

**Output — 9 sections:**

| Section                                 | Contents                                                                        |
| --------------------------------------- | ------------------------------------------------------------------------------- |
| 1. Endpoint Coverage Matrix             | Per-endpoint table: auth, roles, happy path, negative, security, contract ✅/❌ |
| 2. Functional Test Ideas                | Request/response details, edge cases, error handling, chained calls             |
| 3. Security Test Ideas                  | Auth bypass, privilege escalation, injection, token abuse, sensitive data       |
| 4. Contract & Schema Test Ideas         | Response shape, field types, status codes, headers, versioning                  |
| 5. Performance & Reliability Test Ideas | Load, concurrency, dependency failures, idempotency, rate limiting              |
| 6. Bug Predictions                      | Top 5–8 specific bug hypotheses for this API                                    |
| 7. Suggested Test Execution Order       | Recommended sequence with justification                                         |
| 8. Coverage Gaps & Open Questions       | Untested areas, blocked tests, assumptions to verify                            |
| 9. Chain Output                         | Machine-readable block for downstream tools                                     |

**Three question rounds cover:**

1. API name, purpose, style, consumers, testing goal
2. Endpoints/operations/spec file, auth mechanism, user roles, dependencies, data formats
3. New vs existing, past bugs, recent changes, worst-case failures, constraints, upstream files

**CHAIN INPUT:** Accepts upstream output from `/risk-analysis` or `/bug-predictor` via `#file:` (Copilot) or file path / `@` mention (Claude Code). Maps top risks and bug hypotheses directly into the relevant test sections.

**Security is never optional** — Section 3 is always generated regardless of the stated testing goal.

---

### 📄 Session Report — `test-plan.prompt.md`

**Invoke with:**

- Copilot: `/test-plan #file:.testing/risk-[feature].md #file:.testing/bugs-[feature].md #file:.testing/tests-[feature].md`
- Claude Code: `/test-plan .testing/risk-[feature].md .testing/bugs-[feature].md .testing/tests-[feature].md`

**What it does:**
Reads all prior session files and consolidates everything into a single, clean testing brief. It does not perform new analysis — it synthesises and summarises what was already established across the chain. The output is a structured plain-text document designed to be saved, shared with the team, attached to a ticket, or referenced during test execution.

**Output — one clean document with these sections:**

| Section                   | Contents                                                                                   |
| ------------------------- | ------------------------------------------------------------------------------------------ |
| Overview                  | 2–3 sentence product description + platform, tech stack, users, goal, constraints, history |
| Product Elements in Scope | Only elements that appear in actual findings — one line each, product-specific             |
| Quality Criteria in Scope | Only criteria that appear in actual findings — one line each, product-specific             |
| Risks                     | Full list from risk-analysis, ordered by priority, two lines each                          |
| Predicted Bugs            | Full list from bug-predictor, ordered by confidence, with trigger and symptom              |
| Test Ideas                | Full list from test-ideas, ordered by priority, with technique and execution mode          |
| Coverage Gaps             | Untested areas identified across all sessions                                              |
| Traceability              | Risk → Bug → Test lineage threads (only if full chain was run)                             |
| Notes                     | Any additional tester observations                                                         |

**Design principles:**

- No markdown tables — indented lists and plain text only
- No HTSM definitions or framework explanations in the output
- Sections are omitted entirely if there is nothing to put in them
- Every finding is two lines maximum — no paragraphs in lists
- Product Elements and Quality Criteria are filtered to only what appeared in findings — never the full HTSM list

**Minimal questions:** After reading all session files, it asks only for what's missing — typically just tester name and date. Everything else comes from the CHAIN OUTPUT blocks.

**Use it as the final step** in any chain, once all analysis and test idea sessions are complete.

---

Every tool enforces a **mandatory three-round questioning protocol** before generating any output. This is the single most important design decision in the toolkit.

LLMs default to generating immediately — which produces shallow, generic output that wastes testing time and misses the most important risks. The questioning protocol exists to prevent this.

**If you ask the tool to skip the questions, it will politely decline.** This is intentional. The quality of every risk register, bug prediction, and test idea is determined entirely by the depth of context gathered first.

**If you provide context upfront** — by describing the feature in detail or by passing an upstream file (`#file:` in Copilot, file path argument or `@` mention in Claude Code) — the rounds will be shorter because fewer questions need asking. But the Readiness Check always runs.

**Fast-path mode for experienced users:** If you already maintain a reusable context file such as `.testing/context-login.md`, you can tell the tool: `I have context in .testing/context-login.md — skip to analysis.` The tool should still verify that the context is complete and correct before proceeding, but it will avoid re-asking questions that are already answered.

---

## Chaining Tools Together

The most powerful way to use this toolkit is as a pipeline, where each tool's output feeds the next.

### The Full Chain

```
/risk-analysis ──► /bug-predictor ──► /test-ideas
```

1. Run `/risk-analysis` — save the output as `.testing/risk-[feature].md`
2. Run `/bug-predictor` with the risk file — save as `.testing/bugs-[feature].md`
   - Copilot: `/bug-predictor #file:.testing/risk-[feature].md`
   - Claude Code: `/bug-predictor .testing/risk-[feature].md`
3. Run `/test-ideas` with the bugs file — save as `.testing/tests-[feature].md`
   - Copilot: `/test-ideas #file:.testing/bugs-[feature].md`
   - Claude Code: `/test-ideas .testing/bugs-[feature].md`

### How context carries forward

Each tool ends its output with a `<!-- CHAIN OUTPUT ... -->` block containing all the context established during that session. When you pass it to the next tool (via `#file:` in Copilot, or file path argument / `@` mention in Claude Code), the downstream tool:

- Reads the block automatically
- Skips questions already answered
- Focuses its analysis on the top findings from upstream
- Carries forward feature, platform, tech stack, user types, history, and constraints

This means by the time you reach `/test-ideas`, it already knows everything established in the risk analysis and bug prediction sessions — and the test ideas it generates are directly targeted at the specific risks and bugs identified earlier.

### Traceability

The chain preserves a full lineage:

```
Risk R2 (Reliability > Data Integrity)
  └── Bug B3 (persistent data overwritten on concurrent save)
        └── Test T7 (two users save the same record simultaneously)
```

**See `WORKFLOW.md`** for all four chain patterns, step-by-step chaining instructions, prompting tips, and a quick-reference table for which tool to start with in any situation.

---

## Saving Session Outputs

To enable chaining, save Copilot responses as markdown files in your project. Suggested structure:

```
your-project/
├── .github/          ← toolkit lives here
└── .testing/         ← your session outputs live here
    ├── risk-login.md
    ├── bugs-login.md
    ├── tests-login.md
    └── api-orders.md
```

Then reference them when chaining:

```
# Copilot
/bug-predictor #file:.testing/risk-login.md

# Claude Code
/bug-predictor .testing/risk-login.md
```

**Jira-friendly tip:**

- Use Jira ticket IDs in your filenames, e.g. `.testing/risk-JIRA-123.md`, `.testing/bugs-JIRA-123.md`, `.testing/tests-JIRA-123.md`.
- Include `jira_ticket:`, `epic:`, `user_story:`, and `acceptance_criteria:` in the context file.
- Save and attach these markdown files to the corresponding Jira issue so test output is traceable back to the story.
- Optionally add `jira_ticket:` and `epic:` metadata as a comment at the top of each saved session output so reviewers can confirm the issue linkage without opening the ticket.

---

## Effective Prompting

**Lead with your goal and context, not just the feature name:**

> ✅ `"I need a risk analysis for the payment checkout flow — we've had data integrity issues before and are releasing on Friday"`
> ❌ `"analyse the checkout feature"`

**Reference your actual files whenever possible:**

> Copilot: `/test-ideas #file:src/auth/login.ts #file:.testing/bugs-login.md`
> Claude Code: `/test-ideas .testing/bugs-login.md` (and @-mention `src/auth/login.ts`)

**Chain the tools in order for maximum value:**

> The test ideas generated from a bug prediction that came from a risk analysis are significantly more targeted than test ideas generated from scratch.

**Push back and refine:**

> `"These feel too generic — focus on the security predictions only"` or `"Expand the data boundary section, I'm worried about the cardinality edge cases"`

**The one thing to avoid:**

> Don't prompt with `"generate test cases for X"` — this bypasses the intent of the questioning protocol and produces flat, unhelpful lists. Always frame requests around a **goal** or **question**.

---

## Quick Reference

### Which tool to start with?

| Situation                                     | Tool                             |
| --------------------------------------------- | -------------------------------- |
| New feature — don't know the risks yet        | `/risk-analysis`                 |
| Know the risks — need specific bug hypotheses | `/bug-predictor`                 |
| Know the risks and bugs — need tests now      | `/test-ideas`                    |
| Testing an API                                | `/api-testing`                   |
| Ad-hoc question or general guidance           | Base assistant (no command)      |
| Have a risk analysis, need an API test plan   | `/api-testing` + risk file       |
| Sessions done — need a clean summary document | `/test-plan` + all session files |

### HTSM at a glance

**9 Test Techniques:** Function · Claims · Domain · User · Stress · Risk · Flow · Scenario · Tool-Supported

**7 Product Elements:** Structure · Function · Data · Interfaces · Platform · Operations · Time

**10 Quality Criteria:** Capability · Reliability · Usability · Security · Performance · Scalability · Compatibility · Installability · Charisma · Development

**4 Project Environment areas:** Mission · Information · Developer Relations · Test Team · Equipment & Tools · Schedule · Test Items · Deliverables

---

## File Reference

| File                                    | Role                                                                   | Read by                                            |
| --------------------------------------- | ---------------------------------------------------------------------- | -------------------------------------------------- |
| `htsm/*.md`                             | Canonical HTSM reference (techniques, elements, criteria, environment) | All tools when applying HTSM                       |
| `copilot-instructions.md` / `CLAUDE.md` | Base assistant, workflow + HTSM index                                  | Copilot Chat (automatic) / Claude Code (automatic) |
| `.claude/skills/*/SKILL.md`             | Claude Code slash command wrappers                                     | Claude Code (`/risk-analysis`, etc.)               |
| `prompts/risk-analysis.prompt.md`       | Risk analysis engine                                                   | `/risk-analysis` (both tools)                      |
| `prompts/bug-predictor.prompt.md`       | Bug hypothesis generator                                               | `/bug-predictor` (both tools)                      |
| `prompts/test-ideas.prompt.md`          | Test idea generator                                                    | `/test-ideas` (both tools)                         |
| `prompts/api-testing.prompt.md`         | API test plan generator                                                | `/api-testing` (both tools)                        |
| `prompts/test-plan.prompt.md`           | Testing brief generator                                                | `/test-plan` (both tools)                          |
| `WORKFLOW.md`                           | Chaining guide, step-by-step instructions                              | You                                                |
| `README.md`                             | This file — overview and reference                                     | You and your team                                  |

---

## Attribution

This toolkit is grounded in the **Heuristic Test Strategy Model (HTSM)** by James Bach, version 6.0 (2024).
Original model: [www.satisfice.com](https://www.satisfice.com) · Copyright 1996–2024 Satisfice, Inc.
