# PromptSense

A cost-management toolkit for AI usage: estimates token cost before you send
a prompt, flags bloated or duplicate context, tracks spend against a budget
with alerts, and actively reduces cost through caching, model routing, and
prompt optimization — across several client surfaces sharing one engine.

This repository documents the project for portfolio purposes. The
implementation is closed-source — see [Why no source code?](#why-no-source-code).

## What it does

- **Cost estimation** — heuristic or exact (via the provider's real
  tokenizer) token/cost counting, multi-model pricing awareness
- **Budget tracking** — set a monthly budget, log spend, get alerts as you
  approach the limit
- **Prompt optimization** — detects bloated or duplicate context, suggests
  or applies trims, recommends the cheapest model likely to handle a given
  prompt well, flags prompts likely to trigger disproportionately expensive
  output
- **Real-time cost enforcement** — a local reverse proxy sits in front of
  the real API and actively reduces cost on every request: cache-breakpoint
  injection, prompt normalization/dedup, adaptive model routing, log
  truncation, and in-flight request coalescing (two identical concurrent
  requests cost one real call, not two)
- **Local-first correction** — offline spelling/grammar correction, with an
  optional local-LLM polish pass that never touches a paid API

## Architecture

One shared core engine (token/cost estimation, budget and savings tracking,
prompt optimization, local correction) powers multiple independent client
surfaces:

- **CLI** — command-line access to every core capability
- **Claude Desktop extension** — an MCP server exposing the toolkit as
  callable tools directly inside Claude Desktop
- **Standalone GUI** — a desktop app for optimizing a prompt before pasting
  it elsewhere, with an auto-send mode
- **Local proxy** — sits between your own scripts/SDKs and the real API,
  enforcing budget and applying cost-saving request rewrites in real time
- **Browser extension / VS Code extension** — planned, not yet built

All surfaces read/write the same local state (budget, savings, dedupe
history), so usage in one surface is reflected in the others.

## Status

Core engine, CLI, Claude Desktop extension, local proxy, and standalone GUI
are built and covered by an extensive automated test suite (100+ tests
across engine logic, proxy request-rewriting behavior, and live checks
against a real local LLM). Browser extension and VS Code extension are
planned but not yet built. Not yet shipped as a public tool or extension
listing.

## Why no source code?

This is a substantial, working toolkit solving a real and growing pain
point (AI usage cost management) with no obvious dominant competitor yet —
the source stays private to preserve that option. This showcase exists so
the architecture and scope can still be evaluated for portfolio purposes.
