# WebGlazer MCP server

Connect your AI agent to your [WebGlazer](https://webglazer.com) AI-visibility data. WebGlazer tracks how ChatGPT, Gemini, Perplexity, Copilot and Google AI Mode mention and cite your brand, which competitors they name instead, and what to fix.

The MCP server lets Claude, Claude Code, Cursor or another MCP client read your runs, prompts, AI answers, citations, sentiment, trends, products and your To-do list. You ask questions in plain language, and the agent answers from the same data the WebGlazer dashboard shows. One connection covers every brand you have access to.

This repository holds documentation only. The server is hosted by WebGlazer, so there is nothing to install or run.

Full guide: [webglazer.com/docs/integrations/mcp](https://webglazer.com/docs/integrations/mcp)

## Connection details

| Property | Value |
| - | - |
| Server URL | `https://webglazer-mcp-56836240603.europe-west1.run.app/mcp` |
| Transport | Streamable HTTP |
| Authentication | OAuth sign-in, or a token you generate in WebGlazer |

Always copy the current server URL from the WebGlazer dashboard: **Settings** > **Integrations** > **Your AI agent** > **Setup guide** > **Server URL**. If it differs from the one above, use the dashboard's.

You need a WebGlazer account with at least one brand and an active subscription on the brands you want the agent to read.

## Connect your client

### Claude.ai and Claude Desktop (OAuth)

1. In Claude, go to **Customize** > **Connectors**, click **+ Add**, then **Add custom connector**.
2. Name it WebGlazer, paste the server URL and click **Continue**.
3. Under **Authentication**, choose **Sign in now** or **Sign in when needed**, keep **Use Claude's published identity**, and click **Add**.
4. Your browser opens the WebGlazer approval screen. Check the brand list and click **Allow**.

### Claude Code (OAuth)

```bash
claude mcp add --transport http webglazer https://webglazer-mcp-56836240603.europe-west1.run.app/mcp
```

Then type `/mcp` in Claude Code, select `webglazer` and choose **Authenticate**.

### Cursor (token)

Generate a token in WebGlazer under **Settings** > **Integrations** > **Your AI agent** > **Generate token**, then add this to `.cursor/mcp.json` or `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "webglazer": {
      "url": "https://webglazer-mcp-56836240603.europe-west1.run.app/mcp",
      "headers": {
        "Authorization": "Bearer <your token>"
      }
    }
  }
}
```

### Clients that only launch a command

Bridge to the server with `mcp-remote`:

```json
{
  "mcpServers": {
    "webglazer": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://webglazer-mcp-56836240603.europe-west1.run.app/mcp",
        "--header",
        "Authorization: Bearer <your token>"
      ]
    }
  }
}
```

Tokens expire after 30 days. Treat a token like a password: anyone who has it can read every brand you have access to.

## Check that it works

Ask your agent: *"List my WebGlazer brands."* It calls `list_projects` and shows each brand with its name, domain and your role.

## Example prompts

- "How did our AI visibility change week by week over the last three months?"
- "Which sources does AI cite most when it answers our prompts? Are any of them sites where we are not mentioned?"
- "How does AI describe our brand? Summarize the sentiment for the latest run."
- "Which prompts have the lowest visibility in the last run?"
- "Compare our visibility in Google AI Mode and ChatGPT for the last 30 days."
- "What is on our To-do list? Pick the three items that affect the most prompts and mark them as todo."

## Tools

15 tools. All of them read data except `mark_opportunity`, which changes the status of an item on your To-do list.

| Tool | What it returns |
| - | - |
| `list_projects` | The brands this connection can read |
| `list_runs` | The brand's runs that have results, newest first |
| `get_run_summary` | Summary numbers for one run or a date range |
| `get_prompts` | The prompts in a run, with each prompt's visibility numbers |
| `get_prompt_answers` | The AI answers recorded for one prompt across engines |
| `get_citations` | The sources AI cited, with citation totals |
| `get_sources` | The same data as `get_citations`, grouped by host |
| `get_sentiment` | How AI describes the brand |
| `get_trend` | Visibility over time, per run, day, week or month |
| `get_filter_options` | The topics, intents and engines available as filters |
| `list_opportunities` | Cited sources where your brand is absent, ranked by how many prompts they affect |
| `get_opportunity` | One item with its evidence and an instruction for fixing it |
| `mark_opportunity` | Sets an item to `open`, `todo`, `done` or `dismissed` (write) |
| `list_products` | Product visibility for a connected Shopify store |
| `get_product` | One product's visibility, merchants and competing products |

## Revoke access

In WebGlazer, go to **Settings** > **Integrations** > **Your AI agent** > **Clients and tokens** and click **Revoke**. Access ends at once.

## Support

[support@webglazer.com](mailto:support@webglazer.com)
