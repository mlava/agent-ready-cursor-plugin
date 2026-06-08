# AGENTS.md — Agent Ready Cursor Plugin

This repository is a **Cursor plugin** that wires [Agent Ready](https://agent-ready.dev)
into Cursor. It bundles the published `agent-ready` MCP server plus a Cursor rule
and a skill — there is no application code to build here. Use this file to
understand what the repo provides and how an AI agent should use it.

## What Agent Ready does

Agent Ready scans any public URL for **AI agent-readability** — how well a site
exposes itself to LLMs, AI search engines, and autonomous agents — against the
**Vercel Agent Readability Spec**, the **llmstxt.org** standard
(`llms.txt` / `llms-full.txt`), and agent-protocol manifests (MCP server cards,
A2A agent cards, `agents.json`, `agent-permissions.json`, UCP, x402, NLWeb, plus
API Catalog, Web Bot Auth, Agent Skills Discovery, and agent-driven UI / A2UI).
A scan returns a Vercel readability score (0–100 + rating) and an llms.txt score
(0–100), with a per-check `howToFix` for every failing check. It does **not**
edit the target site — pair it with a code-editing tool to land fixes.

## What's in this repo

- `mcp.json` — the `agent-ready` MCP server (`npx -y agent-ready-mcp@latest`),
  exposing three tools — `scan_site`, `get_scan`, `ask` — and three prompts
  (`scan`, `interpret_scan`, `remediation_plan`).
- `rules/agent-ready.mdc` — a Cursor rule: which tool to use for which question,
  and the rules of engagement (pass the URL verbatim, never fabricate scan ids,
  don't dump raw JSON).
- `skills/agent-ready/` — a skill with "when and how to scan" guidance.

## Tools

- `ask` — natural-language search over Agent Ready's methodology, check
  registry, and validated specs. **Public; no API key.** Use it for questions
  *about* Agent Ready (how the score works, what a check does).
- `scan_site` — scan a live URL; returns scores + per-check findings. **Needs a
  Pro API key.**
- `get_scan` — fetch a previous scan by id (e.g. to poll one still running).
  **Needs a Pro API key.**

## Install

Install from the [Cursor Marketplace](https://cursor.com/marketplace) (search
"Agent Ready"), or locally with `./scripts/sync-local.sh`, then reload Cursor.

## Configuration

`scan_site` and `get_scan` require a Pro API key from
<https://agent-ready.dev/dashboard/api-keys>. Set `AGENT_READY_API_KEY` in your
shell, or paste the literal key into the installed
`~/.cursor/plugins/local/agent-ready/mcp.json` (never the repo copy). The `ask`
tool needs no key.

## Usage

In Cursor, ask the agent to "scan example.com for agent readiness" (or use the
bundled prompts). For questions about the methodology, the agent uses the public
`ask` tool. See the canonical docs at <https://agent-ready.dev/docs> and the
MCP details at <https://agent-ready.dev/mcp>.
