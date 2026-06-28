---
description: HTSM API testing — full API test plan covering functional, security, contract, and performance. Use for REST, GraphQL, gRPC, or WebSocket APIs.
disable-model-invocation: true
argument-hint: [optional upstream file, e.g. .testing/risk-orders-api.md]
allowed-tools: Read Write Grep Glob
---

Read `.github/prompts/api-testing.prompt.md` in full and follow every instruction exactly. Do not summarise or skip sections.

When the prompt references `.github/htsm/` files, read those shared reference files in full before analysis.

## Claude Code — session file input

If the user passed file paths in `$ARGUMENTS`, or @-mentioned files in this message, use the Read tool to load each file **before** asking any questions. Treat this as the equivalent of GitHub Copilot's `#file:` chain input.

Arguments: $ARGUMENTS
