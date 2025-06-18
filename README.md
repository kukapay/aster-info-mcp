# Aster Info MCP

An MCP server that provides structured access to [Aster DEX](https://www.asterdex.com/en) market data—covering candlesticks, order books, trades, and funding rates.

<a href="https://glama.ai/mcp/servers/@kukapay/aster-info-mcp">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/@kukapay/aster-info-mcp/badge" alt="aster-info-mcp MCP server" />
</a>

![GitHub License](https://img.shields.io/github/license/kukapay/aster-info-mcp) 
![Python Version](https://img.shields.io/badge/python-3.10%2B-blue)
![Status](https://img.shields.io/badge/status-active-brightgreen.svg)

## Features

- **13 Tools**: Access a variety of Aster Finance Futures API endpoints, including:
  - Candlestick data (`get_kline`, `get_index_price_kline`, `get_mark_price_kline`)
  - Price and ticker data (`get_latest_price`, `get_price_change_statistics_24h`, `get_order_book_ticker`)
  - Order book and trade data (`get_order_book`, `get_recent_trades`, `get_historical_trades`, `get_aggregated_trades`)
  - Funding and index data (`get_premium_index`, `get_funding_rate_history`)
- **Markdown Output**: All tools return data as formatted Markdown tables for easy readability and integration.
- **Robust Error Handling**: Handles HTTP errors (e.g., 400, 429) and data processing issues with clear exceptions.

## Installation

### Prerequisites

- Python 3.10 or higher
- [uv](https://github.com/astral-sh/uv) (recommended package manager)


### Steps

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/kukapay/aster-info-mcp.git
   cd aster-info-mcp
   ```

2. **Install Dependencies**:
   ```bash
   uv sync
   ```
   
3. **Installing to Claude Desktop**:

    Install the server as a Claude Desktop application:
    ```bash
    uv run mcp install main.py --name "Aster Info"
    ```

    Configuration file as a reference:

    ```json
    {
       "mcpServers": {
           "Aster Info": {
               "command": "uv",
               "args": [ "--directory", "/path/to/aster-info-mcp", "run", "main.py" ]
           }
       }
    }
    ```
    Replace `/path/to/aster-info-mcp` with your actual installation path.
   

## Usage

### Available Tools

| Tool Name                          | Description                                                                 | Parameters                                                                 |
|------------------------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------------------|
| `get_kline`                        | Fetch candlestick data for a symbol.                                        | `symbol`, `interval`, `startTime` (opt), `endTime` (opt), `limit` (opt)   |
| `get_index_price_kline`            | Fetch index price candlestick data for a pair.                              | `pair`, `interval`, `startTime` (opt), `endTime` (opt), `limit` (opt)     |
| `get_mark_price_kline`             | Fetch mark price candlestick data for a symbol.                             | `symbol`, `interval`, `startTime` (opt), `endTime` (opt), `limit` (opt)   |
| `get_premium_index`                | Fetch premium index data (mark price, funding rate).                        | `symbol` (opt)                                                            |
| `get_funding_rate_history`         | Fetch historical funding rate data for a symbol.                            | `symbol`, `startTime` (opt), `endTime` (opt), `limit` (opt)               |
| `get_price_change_statistics_24h`  | Fetch 24-hour price change statistics.                                      | `symbol` (opt)                                                            |
| `get_latest_price`                 | Fetch the latest price for a symbol or all symbols.                         | `symbol` (opt)                                                            |
| `get_order_book_ticker`            | Fetch order book ticker data (best bid/ask prices and quantities).          | `symbol` (opt)                                                            |
| `get_order_book`                   | Fetch order book data (bids and asks) for a symbol.                         | `symbol`, `limit` (opt)                                                   |
| `get_recent_trades`                | Fetch recent trades for a symbol.                                           | `symbol`, `limit` (opt)                                                   |
| `get_historical_trades`            | Fetch historical trades for a symbol.                                       | `symbol`, `limit` (opt), `fromId` (opt)                                   |
| `get_aggregated_trades`            | Fetch aggregated trades for a symbol.                                       | `symbol`, `fromId` (opt), `startTime` (opt), `endTime` (opt), `limit` (opt) |

**Notes**:
- All tools return data as Markdown tables.
- Parameters marked `(opt)` are optional.
- Timestamps are in milliseconds (Unix epoch); outputs are converted to readable datetime format.
- Numeric fields are rounded to 8 decimal places (except `priceChangePercent`, rounded to 2).


### Examples

Below are examples for each of the 13 tools.

#### Example: Fetching Candlestick Data (`get_kline`)

Prompt: 
```
Get me the latest 1-minute candlestick data for ETHUSDT, limited to the last 2 entries.
```

Expected response (Markdown table):
```markdown
| open_time           | open      | high      | low       | close     |
|---------------------|-----------|-----------|-----------|-----------|
| 2025-06-18 22:42:00 | 3500.1234 | 3510.5678 | 3490.4321 | 3505.6789 |
| 2025-06-18 22:43:00 | 3505.6789 | 3520.1234 | 3500.8765 | 3510.2345 |
```

#### Example: Fetching Index Price Candlestick Data (`get_index_price_kline`)

Prompt: 
```
Show me the 1-hour index price candlestick data for BTCUSD for the last 2 hours.
```

Expected response (Markdown table):
```markdown
| open_time           | open       | high       | low        | close      |
|---------------------|------------|------------|------------|------------|
| 2025-06-18 21:00:00 | 65000.1234 | 65200.5678 | 64900.4321 | 65100.6789 |
| 2025-06-18 22:00:00 | 65100.6789 | 65300.1234 | 65050.8765 | 65210.2345 |
```

#### Example: Fetching Mark Price Candlestick Data (`get_mark_price_kline`)

Prompt: 
```
Give me the 1-minute mark price candlestick data for BTCUSDT, limited to the last 2 entries.
```

Expected response (Markdown table):
```markdown
| open_time           | open       | high       | low        | close      |
|---------------------|------------|------------|------------|------------|
| 2025-06-18 22:42:00 | 65010.1234 | 65020.5678 | 65000.4321 | 65015.6789 |
| 2025-06-18 22:43:00 | 65015.6789 | 65030.1234 | 65010.8765 | 65025.2345 |
```

#### Example: Fetching Premium Index Data (`get_premium_index`)

Prompt: 
```
Show me the premium index data for ETHUSDT.
```

Expected response (Markdown table):
```markdown
| symbol  | markPrice  | indexPrice | lastFundingRate | nextFundingTime     |
|---------|------------|------------|-----------------|---------------------|
| ETHUSDT | 3505.1234  | 3500.5678  | 0.0001          | 2025-06-19 00:00:00 |
```

#### Example: Fetching Funding Rate History (`get_funding_rate_history`)

Prompt: 
```
Get the funding rate history for BTCUSDT, limited to the last 2 records.
```

Expected response (Markdown table):
```markdown
| symbol  | fundingTime         | fundingRate |
|---------|---------------------|-------------|
| BTCUSDT | 2025-06-18 16:00:00 | 0.00012     |
| BTCUSDT | 2025-06-18 20:00:00 | 0.00015     |
```

#### Example: Fetching 24-Hour Price Change Statistics (`get_price_change_statistics_24h`)

Prompt: 
```
Show me the 24-hour price change statistics for ETHUSDT.
```

Expected response (Markdown table):
```markdown
| symbol  | priceChange | priceChangePercent | lastPrice  | volume     |
|---------|-------------|--------------------|------------|------------|
| ETHUSDT | 50.1234     | 1.45               | 3505.6789  | 1000.4321  |
```

#### Example: Fetching Latest Price (`get_latest_price`)

Prompt: 
```
Show me the current price of BTCUSDT.
```

Expected response (Markdown table):
```markdown
| symbol  | price      |
|---------|------------|
| BTCUSDT | 65000.1234 |
```

#### Example: Fetching Order Book Ticker Data (`get_order_book_ticker`)

Prompt*: 
```
Get the best bid and ask prices for ETHUSDT.
```

Expected response (Markdown table):
```markdown
| symbol  | bidPrice  | bidQty    | askPrice  | askQty    |
|---------|-----------|-----------|-----------|-----------|
| ETHUSDT | 3500.1234 | 10.5678   | 3505.6789 | 15.4321   |
```

#### Example: Fetching Order Book Data (`get_order_book`)

Prompt: 
```
Show me the order book for BTCUSDT with 2 entries per side.
```

Expected response (Markdown table):
```markdown
| side | price      | quantity   |
|------|------------|------------|
| bid  | 65000.1234 | 0.5678     |
| bid  | 64995.6789 | 0.4321     |
| ask  | 65005.1234 | 0.8765     |
| ask  | 65010.6789 | 0.2345     |
```

#### Example: Fetching Recent Trades (`get_recent_trades`)

Prompt: 
```
Get the most recent trades for ETHUSDT, limited to 2 trades.
```

Expected response (Markdown table):
```markdown
| tradeId | price     | qty       | quoteQty  | time                | isBuyerMaker |
|---------|-----------|-----------|-----------|---------------------|--------------|
| 123456  | 3505.6789 | 1.2345    | 4321.1234 | 2025-06-18 22:43:00 | True         |
| 123457  | 3500.1234 | 0.8765    | 3067.5678 | 2025-06-18 22:42:00 | False        |
```

#### Example: Fetching Historical Trades (`get_historical_trades`)

Prompt: 
```
Show me historical trades for BTCUSDT starting from trade ID 1000, limited to 2 trades.
```

Expected response (Markdown table):
```markdown
| tradeId | price      | qty       | quoteQty   | time                | isBuyerMaker |
|---------|------------|-----------|------------|---------------------|--------------|
| 1000    | 65000.1234 | 0.1234    | 8025.6789  | 2025-06-18 20:00:00 | True         |
| 1001    | 64995.6789 | 0.2345    | 15245.1234 | 2025-06-18 20:01:00 | False        |
```

#### Example: Fetching Aggregated Trades (`get_aggregated_trades`)

Prompt: 
```
Get aggregated trades for ETHUSDT starting from aggregated trade ID 500, limited to 2 trades.
```

Expected response (Markdown table):
```markdown
| aggTradeId | price     | qty       | firstTradeId | lastTradeId | time                | isBuyerMaker |
|------------|-----------|-----------|--------------|-------------|---------------------|--------------|
| 500        | 3500.1234 | 5.6789    | 1000         | 1005        | 2025-06-18 22:40:00 | True         |
| 501        | 3505.6789 | 3.4321    | 1006         | 1010        | 2025-06-18 22:41:00 | False        |
```


## License

This project is licensed under the [MIT License](LICENSE).