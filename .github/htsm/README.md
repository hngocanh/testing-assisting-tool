# HTSM Reference Library

Canonical reference content for the **Heuristic Test Strategy Model (HTSM)** by James Bach (v6.0). All toolkit prompts and the base assistant (`copilot-instructions.md` / `CLAUDE.md`) reference these files instead of duplicating content.

## Files

| File | HTSM dimension | Used by |
|---|---|---|
| [test-techniques.md](test-techniques.md) | 9 General Test Techniques | Base assistant, risk analysis, bug predictor, test ideas, API testing |
| [product-elements.md](product-elements.md) | 7 Product Elements | Base assistant, risk analysis, bug predictor, test ideas, API testing, test plan |
| [quality-criteria.md](quality-criteria.md) | 10 Quality Criteria | Base assistant, risk analysis, bug predictor, test ideas, API testing, test plan |
| [project-environment.md](project-environment.md) | 8 Project Environment areas | Base assistant |

## For AI tools

When a prompt instructs you to read these files, use the Read tool (Claude Code) or `#file:` / `@` references (Copilot) to load the full content before analysis. Do not rely on memory or paraphrase — read the canonical definitions.

## Attribution

Grounded in the **Heuristic Test Strategy Model (HTSM)** by James Bach, version 6.0 (2024).
Original model: [www.satisfice.com](https://www.satisfice.com) · Copyright 1996–2024 Satisfice, Inc.
