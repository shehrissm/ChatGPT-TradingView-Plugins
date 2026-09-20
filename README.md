# ChatGPT TradingView Plugin

A Codex-compatible plugin that connects ChatGPT and Codex to TradingView's official Model Context Protocol (MCP) server.

The plugin contains no scraper, market-data proxy, credentials, or API keys. It only declares the official TradingView MCP endpoint:

```text
https://mcp.tradingview.com/mcp
```

Authentication is handled directly by TradingView through OAuth 2.1.

## What it can do

The available tools are controlled by TradingView and may change during the beta. Depending on the tools enabled for your account, you can use natural language to:

- Retrieve quotes, historical prices, and technical data.
- Screen stocks, crypto, forex, and other supported markets.
- Review fundamentals, analyst estimates, news, filings, and economic calendars.
- Compare instruments and market performance.
- Read and manage TradingView watchlists.
- Read, create, pause, resume, or inspect supported alerts.
- Export retrieved data into analyses, scripts, tables, and charts.

This integration is intended for market research and TradingView account operations exposed by the MCP server. It does **not** automatically place trades, access broker balances, or manage broker positions unless TradingView explicitly adds and exposes such tools.

## Requirements

- ChatGPT desktop, Codex desktop, Codex CLI, or another client that supports Streamable HTTP MCP servers.
- A TradingView **Essential plan or higher**. Trial plans currently do not include MCP access.
- A TradingView account that can complete OAuth authorization.
- Internet access to `https://mcp.tradingview.com/mcp`.

TradingView MCP is currently a public beta. Tool availability, limits, and behavior may change.

## Quick installation: connect the MCP server directly

This is the simplest installation method if you only need the TradingView tools.

### Codex CLI

Run:

```bash
codex mcp add tradingview --url https://mcp.tradingview.com/mcp
```

Then:

1. Complete the TradingView sign-in and OAuth approval in the browser.
2. Start a new Codex task.
3. Check the connection with:

```bash
codex mcp list
```

4. Ask Codex to use TradingView, for example: `Use TradingView to compare AAPL and MSFT year to date.`

### ChatGPT or Codex desktop

Menu names can vary slightly by application version:

1. Open **Settings**.
2. Open **Plugins** or **MCP Servers**.
3. Choose **Add MCP Server**.
4. Enter a recognizable name such as `TradingView`.
5. Select **Streamable HTTP**.
6. Enter `https://mcp.tradingview.com/mcp`.
7. Save the connection.
8. Select **Authenticate**, sign in to TradingView, and approve access.
9. Start a new chat or Codex task.

## Install the packaged plugin from this repository

This repository uses the supported Codex compatibility layout:

```text
ChatGPT-TradingView-Plugins/
├── .codex-plugin/
│   └── plugin.json
├── .mcp.json
└── README.md
```

To install it as a local plugin:

1. Clone the repository.

```bash
git clone https://github.com/shehrissm/ChatGPT-TradingView-Plugins.git
```

2. Note the absolute path of the cloned folder.
3. In Codex, invoke `$plugin-creator` and ask:

```text
Add the existing plugin at <absolute-path>/ChatGPT-TradingView-Plugins
to my personal marketplace, validate it, and install it.
```

4. Allow Codex to create or update the personal marketplace when prompted.
5. Open the Plugins directory and install or enable **TradingView** from the personal/local source.
6. Authenticate with TradingView when prompted.
7. Start a **new task** so Codex loads the plugin and its MCP tools.

If you are developing the plugin locally, use `$plugin-creator` again after changes so it can validate, update the cachebuster, and reinstall the package safely.

## Usage examples

After installation and authentication, try prompts such as:

```text
Use TradingView to summarize what is happening with NVDA today.
```

```text
Compare BTC, ETH, and SOL over the last 90 days.
Include return, maximum drawdown, and volatility in one table.
```

```text
Find US technology stocks breaking out on rising volume
and trading within 3% of their 52-week highs.
```

```text
Show my TradingView watchlists and summarize the largest movers.
```

```text
Which of my TradingView alerts fired during the last seven days?
Explain what happened after each alert.
```

```text
Create a TradingView watchlist named Dividend Income
with the symbols I approve.
```

For actions that change TradingView data, such as creating a watchlist or alert, review the proposed action before approving it.

## Authentication and security

- The plugin does not store TradingView passwords.
- OAuth authorization occurs directly between your MCP client and TradingView.
- No API key is required by this repository.
- The MCP server receives requests needed to perform the TradingView actions you ask for.
- Review write actions carefully. Read-only requests do not modify your TradingView account, while supported write tools can change watchlists, alerts, or other account data.
- Revoke the connection from your MCP client or TradingView account settings if you no longer want it authorized.

## Repository files

### `.codex-plugin/plugin.json`

Defines the plugin identity, description, display metadata, example prompts, and the reference to the MCP configuration.

### `.mcp.json`

Declares the official TradingView Streamable HTTP MCP server:

```json
{
  "mcpServers": {
    "tradingview": {
      "type": "http",
      "url": "https://mcp.tradingview.com/mcp"
    }
  }
}
```

## Troubleshooting

### Authentication is requested repeatedly

Remove or disable the existing TradingView MCP connection, add it again, and complete OAuth in the same browser session. Then start a new task.

### TradingView tools do not appear

- Confirm your TradingView subscription is Essential or higher.
- Run `codex mcp list` when using the CLI.
- Confirm the server URL is exactly `https://mcp.tradingview.com/mcp`.
- Restart the desktop application or start a new task after installing the plugin.
- Check that the plugin or MCP server is enabled.

### A requested capability is unavailable

TradingView controls the server's tool catalog. The beta may not expose every TradingView feature, and connected-broker trading functions are separate from the MCP server.

### Data appears delayed

TradingView states that the beta can provide delayed market data. Confirm time sensitivity and exchange status before relying on a result.

### Rate-limit errors

Reduce the number of repeated requests and try again later. TradingView documents an approximate limit of 100 tool calls per minute per user, with possible additional beta limits.

## Uninstall or disconnect

For a direct CLI connection:

```bash
codex mcp remove tradingview
```

For a desktop installation, disable or remove TradingView from **Plugins** or **MCP Servers**. You can also revoke the OAuth connection from TradingView.

## Important disclaimer

This project is an independent packaging of TradingView's official MCP connection and is not financial advice. Verify market data and orders independently. TradingView is a trademark of its respective owner.

## Documentation

- [TradingView MCP documentation](https://www.tradingview.com/mcp/docs)
- [TradingView MCP public beta announcement](https://www.tradingview.com/blog/en/tradingview-mcp-server-public-beta-60864/)
- [OpenAI plugin architecture](https://developers.openai.com/plugins/concepts/plugins)
- [OpenAI plugin packaging guide](https://developers.openai.com/plugins/build/plugins)
- [OpenAI MCP documentation](https://developers.openai.com/docs/extend/mcp)
