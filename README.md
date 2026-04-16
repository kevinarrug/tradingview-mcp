# TradingView MCP

A Model Context Protocol (MCP) server that provides TradingView market data and technical analysis capabilities to AI assistants.

> **Fork of** [atilaahmettaner/tradingview-mcp](https://github.com/atilaahmettaner/tradingview-mcp) with additional features and improvements.

## Features

- 📈 **Real-time market data** — Fetch quotes, OHLCV data, and market overviews
- 🔍 **Technical analysis** — Access indicators, oscillators, and moving averages
- 🔎 **Symbol search** — Search for stocks, crypto, forex, and futures
- 📊 **Screener** — Filter markets by technical conditions
- 🤖 **MCP-compatible** — Works with Claude, Cursor, and any MCP-enabled client

## Requirements

- Python 3.10+
- [uv](https://docs.astral.sh/uv/) (recommended) or pip

## Installation

### Using uv (recommended)

```bash
git clone https://github.com/your-username/tradingview-mcp.git
cd tradingview-mcp
uv sync
```

### Using pip

```bash
git clone https://github.com/your-username/tradingview-mcp.git
cd tradingview-mcp
pip install -e .
```

## Configuration

Copy `.env.example` to `.env` and adjust settings:

```bash
cp .env.example .env
```

## Usage

### Run the MCP server

```bash
uv run python -m tradingview_mcp
```

### Docker

```bash
docker build -t tradingview-mcp .
docker run --env-file .env tradingview-mcp
```

## MCP Client Configuration

### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "tradingview": {
      "command": "uv",
      "args": ["run", "python", "-m", "tradingview_mcp"],
      "cwd": "/path/to/tradingview-mcp"
    }
  }
}
```

### Cursor

Add to `.cursor/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "tradingview": {
      "command": "uv",
      "args": ["run", "python", "-m", "tradingview_mcp"],
      "cwd": "/path/to/tradingview-mcp"
    }
  }
}
```

## Available Tools

| Tool | Description |
|------|-------------|
| `get_quote` | Get real-time quote for a symbol |
| `get_technical_analysis` | Get technical analysis summary (oscillators, MAs) |
| `search_symbols` | Search for trading symbols |
| `get_screener` | Screen markets by technical conditions |
| `get_multiple_quotes` | Fetch quotes for multiple symbols at once |

> **Note:** I primarily use `get_technical_analysis` and `get_multiple_quotes` with crypto symbols (e.g. `BINANCE:BTCUSDT`). If you're doing the same, make sure to prefix symbols with the exchange name.

## Symbols I Use

For quick reference, here are the symbols I track most often:

- `BINANCE:BTCUSDT` — Bitcoin
- `BINANCE:ETHUSDT` — Ethereum
- `BINANCE:SOLUSDT` — Solana
- `BINANCE:BNBUSDT` — BNB
- `BINANCE:ADAUSDT` — Cardano
- `BINANCE:DOTUSDT` — Polkadot
- `BINANCE:XRPUSDT` — XRP
- `BINANCE:LINKUSDT` — Chainlink
- `BINANCE:AVAXUSDT` — Avalanche
- `BINANCE:OPUSDT` — Optimism
- `BINANCE:ARBUSDT` — Arbitrum
- `NASDAQ:NVDA` — NVIDIA
- `NASDAQ:TSLA` — Tesla
- `NASDAQ:MSFT` — Microsoft
- `NASDAQ:AAPL` — Apple
- `NASDAQ:AMZN` — Amazon

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for relea