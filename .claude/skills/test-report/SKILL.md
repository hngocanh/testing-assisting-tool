---
description: HTSM test report generator — craft a concise stakeholder-facing test report from debrief and session files.
disable-model-invocation: true
argument-hint:
  [
    optional debrief file or session files,
    e.g. .testing/debrief/JIRA-123-debrief.md,
  ]
allowed-tools: Read Write Grep Glob
---

Read `.github/prompts/test-report.prompt.md` in full and follow every instruction exactly. Do not summarise or skip sections.

When the prompt references `.github/htsm/` files, read those shared reference files in full before analysis.

## Claude Code — session file input

If the user passed file paths in `$ARGUMENTS`, or @-mentioned files in this message, use the Read tool to load each file before asking any questions. Treat this as the equivalent of GitHub Copilot's `#file:` chain input.

Arguments: $ARGUMENTS
