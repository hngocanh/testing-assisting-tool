# Testing Toolkit — Workflow Guide

This guide explains how to use the HTSM-grounded testing toolkit with **GitHub Copilot Chat** (VS Code) or **Claude Code**, including how to chain tools together so each session builds on the last.

---

## File references when chaining

| Tool                     | Copilot                                       | Claude Code                                                                              |
| ------------------------ | --------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Reference a session file | `#file:.testing/risk-login.md`                | `.testing/risk-login.md` as slash command argument, or `@.testing/risk-login.md` in chat |
| Example chain step       | `/bug-predictor #file:.testing/risk-login.md` | `/bug-predictor .testing/risk-login.md`                                                  |

Claude Code supports chain input via CLI arguments or @-mentions. The included `.claude/skills/*/SKILL.md` wrappers detect `$ARGUMENTS` and read any passed files before asking questions.

Both tools read the same `<!-- CHAIN OUTPUT -->` blocks from saved session files.

> Tip: create a reusable `.testing/context.template.md` for each feature and pass it into tools so they can skip already-answered questions.

---

## The Tools

| Command          | Copilot file                      | Claude Code file                                 | Purpose                                                                     |
| ---------------- | --------------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------- |
| Always active    | `copilot-instructions.md`         | `CLAUDE.md`                                      | Base testing assistant — general help, ad-hoc questions, feature analysis   |
| `/risk-analysis` | `prompts/risk-analysis.prompt.md` | `.claude/skills/risk-analysis/SKILL.md` → prompt | Identify and prioritise risks _before_ testing begins                       |
| `/bug-predictor` | `prompts/bug-predictor.prompt.md` | `.claude/skills/bug-predictor/SKILL.md` → prompt | Generate specific, falsifiable bug hypotheses                               |
| `/test-ideas`    | `prompts/test-ideas.prompt.md`    | `.claude/skills/test-ideas/SKILL.md` → prompt    | Generate a prioritised, technique-grounded list of test ideas               |
| `/api-testing`   | `prompts/api-testing.prompt.md`   | `.claude/skills/api-testing/SKILL.md` → prompt   | Full API test plan covering functional, security, contract, and performance |
| `/test-plan`     | `prompts/test-plan.prompt.md`     | `.claude/skills/test-plan/SKILL.md` → prompt     | Consolidate all session outputs into one clean testing brief                |

---

## The Workflow Chains

### Chain 1 — Full Feature Testing (Recommended)

```
/risk-analysis
      │
      │  save output as: risk-[feature].md
      ▼
/bug-predictor  ◄── risk file (Copilot: #file: / Claude Code: path arg or @)
      │
      │  save output as: bugs-[feature].md
      ▼
/test-ideas  ◄── bugs file
      │
      │  save output as: tests-[feature].md
      ▼
/test-plan  ◄── all session files
      │
      │  save output as: brief-[feature].txt
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
/test-ideas  ◄── risk file
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
/api-testing  ◄── risk file
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
/bug-predictor  ◄── api file
      │
      │  save output as: bugs-[name].md
      ▼
/test-ideas  ◄── bugs file
```

**When to use:** After an initial API test plan reveals areas that deserve deeper investigation — use bug prediction to hypothesise what's hiding there, then generate targeted tests.

---

## How Chaining Works — Step by Step

### Step 1 — Run the first tool

Invoke `/risk-analysis` (or whichever tool starts your chain). Answer all three rounds of questions fully. The tool will generate its output ending with a `CHAIN OUTPUT` block — a structured, machine-readable summary.

### Step 2 — Save the output

Copy the entire response into a markdown file in your project. Suggested naming convention:

```
.testing/
  risk-JIRA-123.md
  bugs-JIRA-123.md
  tests-JIRA-123.md
  api-orders.md
  debrief/
    debrief-template.md
```

Use the debrief folder for feature-level knowledge and report base material:

```
.testing/debrief/JIRA-123-debrief.md
```

For Jira-friendly workflows, include the ticket ID in the filename and also in the reusable context file, so the output is easy to attach to the issue and trace back to the story.

You can save these anywhere in your project that makes sense — the key is that your AI tool can read them (Copilot via `#file:`, Claude Code via Read tool / `@` mention).

### Step 3 — Invoke the next tool with the saved file

**Copilot:**

```
/bug-predictor #file:.testing/risk-login.md
```

**Claude Code:**

```
/bug-predictor .testing/risk-login.md
```

Or @-mention the file in chat alongside the slash command.

The tool will immediately read the file, find the `CHAIN OUTPUT` block, extract all context fields, and:

- Skip questions that were already answered in the upstream session
- Focus its analysis on the top risks identified upstream
- Use tech stack and history from the prior session

### Step 4 — Confirm the readiness check

Each tool still performs a Readiness Check before generating output — but when chaining, it will be shorter because most context is already known. You'll see something like:

> "Based on your risk analysis, I understand we're looking at the **Login feature** on a **React + Node web app**, targeting **Security and Reliability** risks, with a release deadline on Friday. The top risks from your analysis are [R1, R2, R3]. Does this accurately capture the situation?"

Confirm or correct, then let it generate.

> Fast-path mode: If you already maintain `.testing/context-[feature].md`, pass it directly and say: `I have context in .testing/context-login.md — skip to analysis.` The tool should use that file to skip answered questions and only ask what is missing.

### Step 5 — Save and continue the chain

Save the output to a new file and continue to the next tool in your chosen chain.

---

## The CHAIN OUTPUT Block

Every tool ends its output with a `<!-- CHAIN OUTPUT ... -->` block. This is the contract between tools. Here's what each field means:

| Field                       | Description                                                      |
| --------------------------- | ---------------------------------------------------------------- |
| `source_tool`               | Which tool produced this output                                  |
| `feature` / `api_name`      | The feature or API being tested                                  |
| `platform`                  | Web, mobile, REST API, etc.                                      |
| `testing_goal`              | What the tester is trying to achieve                             |
| `tech_stack`                | Languages, frameworks, libraries, databases                      |
| `user_types` / `user_roles` | The user roles relevant to this feature                          |
| `known_history`             | Past bugs, fragile areas, recurring issues                       |
| `key_constraints`           | Testing constraints (time, environment, data)                    |
| `top_risks`                 | Prioritised risks (from `/risk-analysis`)                        |
| `top_bugs`                  | Specific bug hypotheses (from `/bug-predictor`)                  |
| `upstream_risks`            | Risk IDs carried forward from prior tools                        |
| `upstream_bugs`             | Bug IDs carried forward from prior tools                         |
| `answered_questions`        | Context fields already established — downstream tools skip these |
| `recommended_next_tool`     | The suggested next step in the chain                             |

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

> Copilot: `/bug-predictor #file:.testing/risk-login.md — please start`
> Claude Code: `/bug-predictor .testing/risk-login.md — please start`

**Do:** Tell the tool what you want to focus on if the prior session was broad:

> `/test-ideas .testing/bugs-login.md — focus on the security predictions only`

**Do:** Chain multiple files if you have both a risk analysis and bug predictions:

> Copilot: `/test-ideas #file:.testing/risk-login.md #file:.testing/bugs-login.md`
> Claude Code: `/test-ideas .testing/risk-login.md .testing/bugs-login.md`

**Don't:** Paste the entire prior output into the chat message — save it as a file and reference it instead. This is cleaner and lets the tool read the structured CHAIN OUTPUT block more reliably.

**Don't:** Start a new chain session without saving the prior output first. Once the conversation scrolls away, the output is gone.

---

## File Organisation Suggestion

```
your-project/
├── CLAUDE.md                         ← Claude Code base assistant
├── .claude/
│   └── skills/                       ← Claude Code slash commands
│       ├── risk-analysis/SKILL.md
│       ├── bug-predictor/SKILL.md
│       ├── test-ideas/SKILL.md
│       ├── api-testing/SKILL.md
│       └── test-plan/SKILL.md
├── .github/
│   ├── copilot-instructions.md       ← Copilot base assistant
│   ├── WORKFLOW.md                   ← this file
│   └── prompts/
│       ├── risk-analysis.prompt.md
│       ├── bug-predictor.prompt.md
│       ├── test-ideas.prompt.md
│       └── api-testing.prompt.md
└── .testing/                         ← your saved session outputs
    ├── risk-login.md
    ├── bugs-login.md
    ├── tests-login.md
    └── api-orders.md
```

The `.testing/` folder is a suggestion — you can name it anything. The important thing is keeping session files in the project so both Copilot (`#file:`) and Claude Code (Read / `@`) can access them.

---

## Quick Reference — Which Tool to Start With?

| Situation                                     | Start with                       |
| --------------------------------------------- | -------------------------------- |
| New feature, don't know the risks yet         | `/risk-analysis`                 |
| Know the risks, need specific bug hypotheses  | `/bug-predictor`                 |
| Know the risks and bugs, need tests now       | `/test-ideas`                    |
| Testing an API specifically                   | `/api-testing`                   |
| Ad-hoc question, quick help, general guidance | Base assistant (no command)      |
| Have a risk analysis, need an API test plan   | `/api-testing` + risk file       |
| Sessions done, need a clean summary document  | `/test-plan` + all session files |
