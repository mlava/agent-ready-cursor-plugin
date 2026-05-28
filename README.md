# Agent Ready — Cursor Plugin

Scan any URL for **AI agent readability** directly inside Cursor — against the
Vercel Agent Readability Spec, the [llmstxt.org](https://llmstxt.org) standard,
and seven agent-protocol manifests (MCP server cards, A2A agent cards,
`agents.json`, `agent-permissions.json`, UCP, x402, NLWeb). 60 checks across
four spec families with per-check `howToFix` guidance.

Backed by [agent-ready.dev](https://agent-ready.dev) — the same engine behind
the REST API, the MCP server, and the browser extension.

## What's in the bundle

| Component | What it gives you |
|---|---|
| **MCP server** (`mcp.json`) | The `agent-ready` server with three native tools: `scan_site`, `get_scan`, `ask`. Plus three workflow prompts: `scan`, `interpret_scan`, `remediation_plan`. |
| **1 skill** (`skills/agent-ready/`) | "When and how to scan" guidance: tool selection, the `running` placeholder, summarisation rules, the four check families (S/P/L/C), and what *not* to use Agent Ready for. |
| **1 rule** (`rules/agent-ready.mdc`) | On-demand tool-selection guidance: which tool for which question, plus the rules of engagement (URL verbatim, no fabricated scan ids, don't dump raw JSON). |

## Install

Install from the [Cursor Marketplace](https://cursor.com/marketplace) (search
"Agent Ready"), or test locally with the included sync script:

```bash
./scripts/sync-local.sh
```

That copies the plugin into `~/.cursor/plugins/local/agent-ready/`. Then
**Cmd+Shift+P → Developer: Reload Window** in Cursor.

## Configure your API key

`scan_site` and `get_scan` need an Agent Ready Pro API key. The `ask` tool is
public — no key.

1. Issue a Pro key at https://agent-ready.dev/dashboard/api-keys.
2. Set `AGENT_READY_API_KEY` in your shell, **or** put the literal key in
   `~/.cursor/plugins/local/agent-ready/mcp.json` (never the repo copy).

**Dock-launched Cursor doesn't inherit shell env.** If `${AGENT_READY_API_KEY}`
resolves to empty when Cursor is launched from the Dock, either launch from a
terminal or paste the literal key into the installed `mcp.json`.

```json
{
  "mcpServers": {
    "agent-ready": {
      "command": "npx",
      "args": ["-y", "agent-ready-mcp@latest"],
      "env": { "AGENT_READY_API_KEY": "${AGENT_READY_API_KEY}" }
    }
  }
}
```

## Try it

Once installed, ask Cursor's agent things like:

- *Scan `https://example.com` for AI agent readability and summarise the top failures.*
- *What does check P11 measure, and how do I fix it?*
- *Fetch scan `abc1234567` and produce a remediation plan focused on agent protocols.*
- *Is my `/.well-known/mcp.json` valid?*
- *Search Agent Ready's methodology for "markdown mirror".* (`ask`, no key needed)

## Rate limits

- **Pro:** 10 scans/minute, 200/day. Read endpoints (`get_scan`) 120/minute.
- **Team:** 60 scans/minute, 2000/day.
- **Public `ask`:** rate-limited per IP, no key required.

## Links

- Website: https://agent-ready.dev
- Methodology: https://agent-ready.dev/methodology
- Check registry: https://agent-ready.dev/checks
- Agent guide: https://agent-ready.dev/AGENTS.md
- OpenAPI spec: https://agent-ready.dev/api/v1/openapi.json
- MCP server source: https://github.com/mlava/agent-ready-mcp
- Browser extension: https://agent-ready.dev (footer)

## License

MIT — see [LICENSE](LICENSE).
