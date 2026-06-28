---
description: HTSM test ideas — generate a prioritised, technique-grounded list of test ideas. Use when the user needs concrete tests or after risk/bug sessions.
disable-model-invocation: true
argument-hint: [optional upstream file, e.g. .testing/bugs-login.md]
allowed-tools: Read Write Grep Glob
---

Read `.github/prompts/test-ideas.prompt.md` in full and follow every instruction exactly. Do not summarise or skip sections.

When the prompt references `.github/htsm/` files, read those shared reference files in full before analysis.

## Claude Code — session file input

If the user passed file paths in `$ARGUMENTS`, or @-mentioned files in this message, use the Read tool to load each file **before** asking any questions. Treat this as the equivalent of GitHub Copilot's `#file:` chain input.

Arguments: $ARGUMENTS
