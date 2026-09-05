# DevDome MCP server

Ask Claude, Cursor, ChatGPT, Windsurf, VS Code or any MCP client about your website traffic, bots, realtime visitors and uptime. DevDome runs a first-party remote MCP server, so there is nothing to install: one URL and your API key.

- Server URL: `https://analytics.devdome.com/mcp` (streamable HTTP)
- Registry: [`com.devdome/analytics`](https://registry.modelcontextprotocol.io/v0/servers?search=devdome) in the official MCP Registry
- Docs: https://devdome.com/docs/mcp/
- REST alternative: https://devdome.com/docs/api/

## Get a key

1. Sign up at https://devdome.com (free plan works).
2. Add your site and install the WordPress plugin or the snippet.
3. Account, API, create key. Keys start with `ddk_`.

## Connect

**Claude Code**

```bash
claude mcp add devdome https://analytics.devdome.com/mcp \
  -t http -H "Authorization: Bearer YOUR_API_KEY"
```

**Claude Desktop, Cursor** (`claude_desktop_config.json` or `.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "devdome": {
      "url": "https://analytics.devdome.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

**VS Code** (`.vscode/mcp.json`)

```json
{
  "servers": {
    "devdome": {
      "type": "http",
      "url": "https://analytics.devdome.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

**Windsurf** uses `serverUrl` instead of `url`. **Zed** uses `context_servers` with `"source": "custom"`. Clients that cannot send headers can append `?key=YOUR_API_KEY` to the URL.

**curl**

```bash
curl https://analytics.devdome.com/mcp \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "content-type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Tools

| Tool | What it returns |
|---|---|
| `list_sites` | Websites on the account with verification status |
| `add_site` | Add a website, returns the tracking snippet |
| `verify_site` | Verify ownership of a pending site (snippet or DNS TXT) |
| `get_stats` | Visits, visitors, pageviews, outbound clicks, bots, CTR, bounce rate, session duration |
| `get_timeseries` | Daily series of visits, visitors, pageviews, clicks and bots |
| `get_breakdown` | Top N by pages, referrers, countries, devices, browsers, os, dates, keywords, products or redirects |
| `get_bot_report` | Bot traffic split into known bots and behavioural threats |
| `get_realtime` | Visitors active in the last 5 minutes with their current page |
| `get_visitors` | Recent individual visits: entry page, path, country, device, human or bot |
| `get_site_health` | Uptime, response time, TLS and domain checks, open incidents |

Every tool takes `site` (the bare domain) where relevant, plus optional `days` or `from` and `to`. No window means the full retained history of your plan.

## Example prompts

- "Which pages on example.com got the most human visitors this week?"
- "How much of my traffic yesterday was bots, and which ones?"
- "Who is on my site right now?"
- "Is example.com up, and when was the last incident?"

## Protocol

Stateless JSON-RPC over streamable HTTP. Supported MCP revisions: 2025-11-25, 2025-06-18, 2025-03-26, 2024-11-05. Rate limit 60 requests per minute per key. CORS is open for `/mcp` and `/api/v1`.

## About

DevDome is cookieless WordPress-first analytics with a human vs bot split, site health monitoring and a plugin suite on WordPress.org. Part of the [DevDome Family](https://github.com/DevDomeFamily) with [InboxDome](https://inboxdome.com) and [BrandBulb](https://brandbulb.com).

Issues and feature requests for the MCP server: open an issue here.
