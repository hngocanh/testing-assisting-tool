# Testing Toolkit — Workflow Guide

This guide explains how to use the HTSM-grounded testing toolkit in VS Code with GitHub Copilot Chat, including how to chain tools together so each session builds on the last.

---

## The Tools

| Command | File | Purpose |
|---|---|---|
| Always active | `copilot-instructions.md` | Base testing assistant — general help, ad-hoc questions, feature analysis |
| `/risk-analysis` | `prompts/risk-analysis.prompt.md` | Identify and prioritise risks *before* testing begins |
| `/bug-predictor` | `prompts/bug-predictor.prompt.md` | Generate specific, falsifiable bug hypotheses |
| `/test-ideas` | `prompts/test-ideas.prompt.md` | Generate a prioritised, technique-grounded list of test ideas |
| `/api-testing` | `prompts/api-testing.prompt.md` | Full API test plan covering functional, security, contract, and performance |

---

## The Workflow Chains

### Chain 1 — Full Feature Testing (Recommended)

```
/risk-analysis
      │
      │  save output as: risk-[feature].md
      ▼
/bug-predictor  ◄── #file:risk-[feature].md
      │
      │  save output as: bugs-[feature].md
      ▼
/test-ideas  ◄── #file:bugs-[feature].md
      │
      │  save output as: tests-[feature].md
      ▼
    Execute
```

**When to use:** New features, high-risk areas, release sign-off, anywhere you want maximum coverage depth.

---

### Chain 2 — Fast Risk-to-Tests (Skip Bug Prediction)

```
/risk-analysis
      │
      │  save output as: risk-[feature].md
      ▼
/test-ideas  ◄── #file:risk-[feature].md
      │
      │  save output as: tests-[feature].md
      ▼
    Execute
```

**When to use:** Time-pressured testing, well-understood features, regression sweeps where you know the risks and just need test ideas fast.

---

### Chain 3 — API Testing (Standalone or Chained)

**Standalone:**
```
/api-testing
      │
      │  save output as: api-[name].md
      ▼
    Execute
```

**Chained with Risk Analysis:**
```
/risk-analysis
      │
      │  save output as: risk-[api].md
      ▼
/api-testing  ◄── #file:risk-[api].md
      │
      │  save output as: api-[name].md
      ▼
    Execute
```

**When to use:** API testing standalone when starting fresh. Chained when you already have a risk analysis and want the API test plan to directly address those risks.

---

### Chain 4 — Bug Investigation (Deep Dive)

```
/api-testing  (or any prior session)
      │
      │  save output as: api-[name].md
      ▼
/bug-predictor  ◄── #file:api-[name].md
      │
      │  save output as: bugs-[name].md
      ▼
/test-ideas  ◄── #file:bugs-[name].md
```

**When to use:** After an initial API test plan reveals areas that deserve deeper investigation — use bug prediction to hypothesise what's hiding there, then generate targeted tests.

---

## How Chaining Works — Step by Step

### Step 1 — Run the first tool

Invoke `/risk-analysis` (or whichever tool starts your chain). Answer all three rounds of questions fully. The tool will generate its output ending with a `CHAIN OUTPUT` block — a structured, machine-readable summary.

### Step 2 — Save the output

Copy the entire Copilot response into a markdown file in your project. Suggested naming convention:

```
.testing/
  risk-login.md
  bugs-login.md
  tests-login.md
  api-orders.md
```

You can save these anywhere in your project that makes sense — the key is that VS Code can see them so you can reference them with `#file:`.

### Step 3 — Invoke the next tool with `#file:`

In Copilot Chat, type the next tool command followed by the file reference:

```
/bug-predictor #file:.testing/risk-login.md
```

The tool will immediately read the file, find the `CHAIN OUTPUT` block, extract all context fields, and:
- Skip questions that were already answered in the upstream session
- Focus its analysis on the top risks identified upstream
- Use tech stack and history from the prior session

### Step 4 — Confirm the readiness check

Each tool still performs a Readiness Check before generating output — but when chaining, it will be shorter because most context is already known. You'll see something like:

> "Based on your risk analysis, I understand we're looking at the **Login feature** on a **React + Node web app**, targeting **Security and Reliability** risks, with a release deadline on Friday. The top risks from your analysis are [R1, R2, R3]. Does this accurately capture the situation?"

Confirm or correct, then let it generate.

### Step 5 — Save and continue the chain

Save the output to a new file and continue to the next tool in your chosen chain.

---

## The CHAIN OUTPUT Block

Every tool ends its output with a `<!-- CHAIN OUTPUT ... -->` block. This is the contract between tools. Here's what each field means:

| Field | Description |
|---|---|
| `source_tool` | Which tool produced this output |
| `feature` / `api_name` | The feature or API being tested |
| `platform` | Web, mobile, REST API, etc. |
| `testing_goal` | What the tester is trying to achieve |
| `tech_stack` | Languages, frameworks, libraries, databases |
| `user_types` / `user_roles` | The user roles relevant to this feature |
| `known_history` | Past bugs, fragile areas, recurring issues |
| `key_constraints` | Testing constraints (time, environment, data) |
| `top_risks` | Prioritised risks (from `/risk-analysis`) |
| `top_bugs` | Specific bug hypotheses (from `/bug-predictor`) |
| `upstream_risks` | Risk IDs carried forward from prior tools |
| `upstream_bugs` | Bug IDs carried forward from prior tools |
| `answered_questions` | Context fields already established — downstream tools skip these |
| `recommended_next_tool` | The suggested next step in the chain |

---

## Traceability

The chain is designed so that every test idea can be traced back to its origin:

```
Risk R2 (Reliability > Data Integrity)
  └── Bug B3 (persistent data overwritten on concurrent save)
        └── Test T7 (two users save the same record simultaneously — verify last-write-wins)
```

The `upstream_risks` and `upstream_bugs` fields in each CHAIN OUTPUT block preserve this lineage. When you reach `/test-ideas`, the high-priority test ideas reference their source bug IDs (B1, B2…) which in turn reference risk IDs (R1, R2…) from the original risk analysis.

---

## Prompting Tips for Chained Sessions

**Do:** Reference the upstream file immediately in your opening message:
> `/bug-predictor #file:.testing/risk-login.md — please start`

**Do:** Tell the tool what you want to focus on if the prior session was broad:
> `/test-ideas #file:.testing/bugs-login.md — focus on the security predictions only`

**Do:** Chain multiple files if you have both a risk analysis and bug predictions:
> `/test-ideas #file:.testing/risk-login.md #file:.testing/bugs-login.md`

**Don't:** Paste the entire prior output into the chat message — save it as a file and use `#file:` instead. This is cleaner and lets Copilot read the structured block more reliably.

**Don't:** Start a new chain session without saving the prior output first. Once the Copilot conversation scrolls away, the output is gone.

---

## File Organisation Suggestion

```
your-project/
├── .github/
│   ├── copilot-instructions.md
│   ├── WORKFLOW.md                  ← this file
│   └── prompts/
│       ├── risk-analysis.prompt.md
│       ├── bug-predictor.prompt.md
│       ├── test-ideas.prompt.md
│       └── api-testing.prompt.md
└── .testing/                        ← your saved session outputs
    ├── risk-login.md
    ├── bugs-login.md
    ├── tests-login.md
    └── api-orders.md
```

The `.testing/` folder is a suggestion — you can name it anything. The important thing is keeping it in the project so `#file:` references work.

---

## Quick Reference — Which Tool to Start With?

| Situation | Start with |
|---|---|
| New feature, don't know the risks yet | `/risk-analysis` |
| Know the risks, need specific bug hypotheses | `/bug-predictor` |
| Know the risks and bugs, need tests now | `/test-ideas` |
| Testing an API specifically | `/api-testing` |
| Ad-hoc question, quick help, general guidance | Base assistant (no command) |
| Have a risk analysis, need an API test plan | `/api-testing #file:risk-[x].md` |