---
created: 2025-12-12
updated: 2025-12-12
tags: [research, aws, quicksight, fintech, thailand, dashboard, api]
status: reference
---

# Amazon QuickSight Financial Dashboard Integration - Thailand Market

## Overview

Research on building a financial/portfolio performance dashboard using Amazon QuickSight for customers based in Thailand, focusing on real-time and historical financial data sources.

## Key Finding

Amazon QuickSight doesn't natively connect to external financial APIs directly - you need an **ETL pipeline architecture** to ingest data into AWS-supported data stores.

## Recommended Architecture

```
[Data Sources] → [ETL Layer] → [Data Store] → [QuickSight]
     ↓               ↓              ↓
  APIs/MCP      Lambda/Glue    S3/Redshift/
                               Timestream
```

### QuickSight Connection Options

| Mode | Description | Use Case |
|------|-------------|----------|
| **SPICE** | Super-fast, Parallel, In-memory Calculation Engine | Cached data with hourly refresh |
| **Direct Query** | Real-time access to RDS, Redshift, Athena | Live data requirements |

---

## Thailand Financial Data Sources (Detailed)

### 1. SET SMART Marketplace

**Official API from Stock Exchange of Thailand**

| Attribute | Details |
|-----------|---------|
| **Provider** | Stock Exchange of Thailand (SET) |
| **Base URL** | `https://www.set.or.th/en/services/connectivity-and-data/data/smart-marketplace` |
| **Documentation** | [Market Data API Specification (PDF)](https://media.set.or.th/set/Documents/2023/May/Market_Data_API_Service_Specification.pdf) |
| **Authentication** | API Key (contact SET) |
| **Format** | REST API, JSON response |

**Available Data Types:**

| Category | Data |
|----------|------|
| Real-time/Delayed | Equity trading data, bids/offers, SET Index series |
| Historical EOD | Intraday trading, EOD prices, derivatives, investor type breakdown |
| Fundamentals | Company financials, ratios, historical prices |
| ESG | Environmental, Social, Governance metrics |
| Reference | Security profiles, corporate actions |
| One Report | Structured annual company data |

**Pricing:**
| Plan | Cost | Notes |
|------|------|-------|
| Trial (2024) | Free | One Report, ESG Data |
| SETSMART Members | Free | Fundamental Data |
| Production | Contact | infoproducts@set.or.th |

---

### 2. Settrade Open API

**Trading and Market Data API**

| Attribute | Details |
|-----------|---------|
| **Provider** | Settrade (SET subsidiary) |
| **Portal** | https://developer.settrade.com/open-api/ |
| **Authentication** | OAuth 2.0 via broker |
| **Sandbox** | Available for testing |

**API Endpoints:**

| Category | Endpoints |
|----------|-----------|
| User Info | Account, Portfolio, Deal information |
| Market Data | Real-time quotes, historical data |
| Trading | Order placement, Pre-Trade Risk Management (PTRM) |
| Reference | Broker list, symbol info |

**Order Types Supported:**
- Limit Price orders (all sessions)
- Note: MP, ATO, ATC order types not supported via API

**Requirements:**
- Account with participating broker
- Programming capability for algorithm implementation
- Broker approval for API access

**Pricing:** Free (through broker relationship)

---

### 3. Bank of Thailand (BOT) API

**Official Government Financial Data**

| Attribute | Details |
|-----------|---------|
| **Provider** | Bank of Thailand |
| **Current Portal** | https://apiportal.bot.or.th/bot/public/ |
| **New Portal** | https://portal.api.bot.or.th/ (migrate by Dec 31, 2025) |
| **Authentication** | Registration required |
| **Format** | REST API, JSON |
| **Cost** | Free |

**Available API Products:**

| API Product | Description | Available Since |
|-------------|-------------|-----------------|
| Exchange Rates | THB/USD weighted-average, THB/Foreign currency | July 2017 |
| Interest Rates | Policy rate, BIBOR, interbank, deposit/loan rates | July 2017 |
| Debt Securities | Auction results | July 2017 |
| Statistics | Economic, financial institutions, markets, payments | July 2018 |
| Reference | Bank holidays, branches, financial products | Various |

**Exchange Rate API (v2.0.2):**
```
GET /bot/public/Stat-ExchangeRate/...
Response: JSON with date, currency, buying/selling rates
```

**Interest Rate Coverage:**
- Policy Rate
- Interbank Transaction Rates
- Bangkok Interbank Offered Rate (BIBOR)
- Thai Baht Implied Interest Rate
- Deposit/Loan Interest Rates
- US rates, LIBOR, SIBOR
- Spot rate and swap points

**Important:** Current system terminates December 31, 2025. Migrate to new portal.

---

### 4. FundConnext

**Mutual Fund Data Platform**

| Attribute | Details |
|-----------|---------|
| **Provider** | SET Digital Access Platform (DAP) |
| **URL** | https://www.set.or.th/en/dap/services/fundconnext |
| **Regulator** | SEC Thailand (SorNor 55/2559) |

**Data Available:**
- Mutual fund NAV (since inception)
- Prospectus and factsheet
- Fund rules and holidays
- Buy/sell/switching order processing

**Access:** Through fund distributors and asset management companies

---

## Global Financial Data APIs (Detailed)

### 1. Alpha Vantage

**Budget-Friendly Stock Market API**

| Attribute | Details |
|-----------|---------|
| **Base URL** | `https://www.alphavantage.co/query` |
| **Documentation** | https://www.alphavantage.co/documentation/ |
| **Authentication** | API Key (`?apikey=YOUR_KEY`) |
| **Format** | JSON, CSV |

**Core Endpoints:**

| Endpoint | Function | Parameters | Premium |
|----------|----------|------------|---------|
| `TIME_SERIES_INTRADAY` | Intraday OHLCV | symbol, interval (1/5/15/30/60min), outputsize | Yes |
| `TIME_SERIES_DAILY` | Daily OHLCV | symbol, outputsize | No |
| `TIME_SERIES_DAILY_ADJUSTED` | Adjusted daily + splits/dividends | symbol, outputsize | Yes |
| `TIME_SERIES_WEEKLY` | Weekly OHLCV | symbol | No |
| `TIME_SERIES_MONTHLY` | Monthly OHLCV | symbol | No |
| `GLOBAL_QUOTE` | Latest price/volume | symbol | No |
| `SYMBOL_SEARCH` | Ticker lookup | keywords | No |
| `MARKET_STATUS` | Global market status | - | No |
| `NEWS_SENTIMENT` | News with sentiment | tickers, topics, time_from | No |
| `REALTIME_OPTIONS` | Options chains with greeks | symbol | Yes |

**Example Request:**
```
GET https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=AAPL&apikey=demo
```

**Example Response:**
```json
{
  "Meta Data": {
    "1. Information": "Daily Prices",
    "2. Symbol": "AAPL",
    "3. Last Refreshed": "2025-12-11"
  },
  "Time Series (Daily)": {
    "2025-12-11": {
      "1. open": "150.00",
      "2. high": "152.50",
      "3. low": "149.00",
      "4. close": "151.25",
      "5. volume": "50000000"
    }
  }
}
```

**Pricing:**

| Plan | Rate Limit | Cost (Monthly) | Cost (Annual) |
|------|------------|----------------|---------------|
| Free | 25 req/day | $0 | $0 |
| Plan 15 | 75 req/min | $49.99 | $499 |
| Plan 60 | 150 req/min | $99.99 | $999 |
| Plan 120 | 300 req/min | $149.99 | $1,499 |
| Plan 360 | 600 req/min | $199.99 | $1,999 |
| Plan 600 | 1,200 req/min | $249.99 | $2,499 |

**Notes:**
- Premium plans: No daily limits, all features unlocked
- Real-time US data requires premium (exchange regulated)
- `outputsize=full` (20+ years history) is premium only

---

### 2. Twelve Data

**Real-Time WebSocket & REST API**

| Attribute | Details |
|-----------|---------|
| **REST Base URL** | `https://api.twelvedata.com` |
| **WebSocket URL** | `wss://ws.twelvedata.com` |
| **Documentation** | https://twelvedata.com/docs |
| **Authentication** | Query: `?apikey=KEY` or Header: `Authorization: apikey KEY` |
| **Format** | JSON, CSV |

**Core Endpoints:**

| Endpoint | Function | Credits | Description |
|----------|----------|---------|-------------|
| `/time_series` | Historical OHLCV | 1/symbol | Customizable intervals |
| `/quote` | Real-time quote | 1 | 52-week metrics included |
| `/price` | Latest price only | 1 | Minimal response |
| `/eod` | End-of-day close | 1 | Previous day close |
| `/profile` | Company details | 10 | Fundamentals |
| `/dividends` | Dividend history | 20 | Historical payouts |
| `/income_statement` | Income statement | 100 | Financial statements |
| `/balance_sheet` | Balance sheet | 100 | Financial statements |
| `/cash_flow` | Cash flow statement | 100 | Financial statements |

**Technical Indicators:** 100+ available (MACD, RSI, Bollinger Bands, SMA, EMA, etc.)

**Example Request:**
```
GET https://api.twelvedata.com/time_series?symbol=AAPL&interval=1day&outputsize=30&apikey=YOUR_KEY
```

**Example Response:**
```json
{
  "meta": {
    "symbol": "AAPL",
    "interval": "1day",
    "currency": "USD",
    "exchange_timezone": "America/New_York"
  },
  "values": [
    {
      "datetime": "2025-12-11",
      "open": "150.00",
      "high": "152.50",
      "low": "149.00",
      "close": "151.25",
      "volume": "50000000"
    }
  ],
  "status": "ok"
}
```

**WebSocket Example:**
```javascript
const ws = new WebSocket('wss://ws.twelvedata.com/v1/quotes/price?apikey=YOUR_KEY');
ws.send(JSON.stringify({
  "action": "subscribe",
  "params": { "symbols": "AAPL,MSFT,GOOGL" }
}));
```

**Pricing:**

| Plan | API Credits/min | WebSocket Credits | Cost (Monthly) | Cost (Annual) |
|------|-----------------|-------------------|----------------|---------------|
| Basic (Free) | 8 (800/day max) | 8 trial | $0 | $0 |
| Grow | 377 | 8 trial | $79 | $66/mo |
| Pro | 1,597 | 1,500 | $229 | $191/mo |
| Ultra | 10,946 | 10,000 | $999 | $832/mo |
| Enterprise | 17,711+ | 15,000+ | $1,999 | $1,659/mo |

**Notes:**
- No daily limits on paid plans
- 20% student/startup discount available
- Non-profits get free access
- ~170ms WebSocket latency

---

### 3. Financial Datasets

**AI-Focused Stock Market API**

| Attribute | Details |
|-----------|---------|
| **Base URL** | `https://api.financialdatasets.ai` |
| **Documentation** | https://docs.financialdatasets.ai/ |
| **Authentication** | Header: `X-API-KEY: YOUR_KEY` |
| **Coverage** | 30,000+ tickers, 30+ years history |

**Core Endpoints:**

| Endpoint | Function | Pay-As-You-Go Cost |
|----------|----------|-------------------|
| `GET /financials/income-statements` | Income statements | $0.04 |
| `GET /financials/balance-sheets` | Balance sheets | $0.04 |
| `GET /financials/cash-flow-statements` | Cash flow | $0.04 |
| `GET /financials` | All statements bundle | $0.10 |
| `GET /prices` | Stock prices | $0.01 |
| `GET /prices/snapshot` | Current price | $0.01 |
| `GET /news` | Company news | $0.02 |
| `GET /metrics` | Financial metrics | $0.02 |
| `GET /insider-trades` | Insider transactions | $0.02 |
| `GET /institutional-holdings` | 13F holdings | $0.02 |
| `GET /sec-filings` | SEC filings | $0.02 |
| `GET /analyst-estimates` | Analyst estimates | $0.04 |
| `GET /company-facts` | Company info | Free |
| `GET /earnings-releases` | Earnings | Free |
| `GET /crypto/prices` | Crypto prices | $0.01 |
| `GET /interest-rates` | Interest rates | Free |

**Example Request:**
```bash
curl -X GET "https://api.financialdatasets.ai/financials/income-statements?ticker=AAPL&period=annual&limit=5" \
  -H "X-API-KEY: YOUR_API_KEY"
```

**Pricing:**

| Plan | API Requests | License | Cost |
|------|--------------|---------|------|
| Pay-As-You-Go | Per request | Individual | Starting $20 |
| Developer | 1,000/min | Individual only | $200/mo |
| Pro | Unlimited | Redistribution allowed | $2,000/mo |
| Enterprise | Custom | Custom | Contact |

---

### 4. EODHD (EOD Historical Data)

**Comprehensive Historical Data API**

| Attribute | Details |
|-----------|---------|
| **Base URL** | `https://eodhd.com/api` |
| **Documentation** | https://eodhd.com/financial-apis/ |
| **Authentication** | Query: `?api_token=YOUR_TOKEN` |
| **Coverage** | 150,000+ tickers, 60+ exchanges, 30+ years |

**Core Endpoints:**

| Endpoint | Function | Description |
|----------|----------|-------------|
| `/eod/{SYMBOL}.{EXCHANGE}` | End-of-day prices | Historical OHLCV |
| `/intraday/{SYMBOL}.{EXCHANGE}` | Intraday data | 1min to 1hour intervals |
| `/real-time/{SYMBOL}.{EXCHANGE}` | Real-time quotes | Live prices |
| `/fundamentals/{SYMBOL}.{EXCHANGE}` | Fundamentals | Financial statements, ratios |
| `/div/{SYMBOL}.{EXCHANGE}` | Dividends | Dividend history |
| `/splits/{SYMBOL}.{EXCHANGE}` | Stock splits | Split history |
| `/news` | Market news | Financial news feed |
| `/calendar/earnings` | Earnings calendar | Upcoming earnings |
| `/exchange-symbol-list/{EXCHANGE}` | Symbol list | All symbols for exchange |

**Example Request:**
```
GET https://eodhd.com/api/eod/AAPL.US?api_token=YOUR_TOKEN&fmt=json
```

**Example Response:**
```json
[
  {
    "date": "2025-12-11",
    "open": 150.00,
    "high": 152.50,
    "low": 149.00,
    "close": 151.25,
    "adjusted_close": 151.25,
    "volume": 50000000
  }
]
```

**Pricing:**

| Plan | API Calls/Day | Rate Limit | History | Cost (Monthly) | Cost (Annual) |
|------|---------------|------------|---------|----------------|---------------|
| Free | 20 | 1,000/min | 1 year | $0 | $0 |
| EOD Historical | 100,000 | 1,000/min | 30+ years | $19.99 | $199 |
| EOD+Intraday | 100,000 | 1,000/min | 30+ years | $29.99 | $299.90 |
| Fundamentals | 100,000 | 1,000/min | 30+ years | $59.99 | $599.90 |
| All-In-One | 100,000 | 1,000/min | 30+ years | $99.99 | $999.90 |

**Features by Plan:**
- EOD Historical: Splits, dividends, adjusted data, delisted stocks
- EOD+Intraday: + Real-time WebSocket, US ticks API
- Fundamentals: + Financials, insider trades, macro data, 40K logos
- All-In-One: + Calendar/news API, bonds data, priority support

---

### 5. Marketstack

**Simple REST Stock Data API**

| Attribute | Details |
|-----------|---------|
| **Base URL** | `http://api.marketstack.com/v2` |
| **Documentation** | https://marketstack.com/documentation_v2 |
| **Authentication** | Query: `?access_key=YOUR_KEY` |
| **Coverage** | 170,000+ tickers, 70 exchanges, 30 years |

**Core Endpoints:**

| Endpoint | Function |
|----------|----------|
| `/eod` | End-of-day data |
| `/eod/latest` | Latest EOD prices |
| `/intraday` | Intraday data (15min+) |
| `/intraday/latest` | Latest intraday prices |
| `/tickers` | Available symbols |
| `/exchanges` | Supported exchanges |
| `/currencies` | Currency list |
| `/timezones` | Timezone list |

**Example Request:**
```
GET http://api.marketstack.com/v2/eod?access_key=YOUR_KEY&symbols=AAPL
```

**Pricing:**

| Plan | Requests/Month | Features | Cost (Monthly) |
|------|----------------|----------|----------------|
| Free | 100 | EOD only, 12mo history | $0 |
| Basic | 10,000 | + Intraday, 10yr history, HTTPS | $9.99 |
| Professional | 100,000 | + Real-time, full history, support | $49.99 |
| Business | 500,000 | + Direct support, all tools | $149.99 |
| Enterprise | Custom | Custom pricing | Contact |

**Notes:**
- V1 API deprecated after June 30, 2025 - use V2
- 15% discount for annual billing
- Intraday <15min intervals require Professional+

---

### 6. Polygon.io (Massive)

**Institutional-Grade Market Data**

| Attribute | Details |
|-----------|---------|
| **Base URL** | `https://api.polygon.io` |
| **Documentation** | https://polygon.io/docs |
| **WebSocket** | `wss://socket.polygon.io` |
| **Authentication** | Header: `Authorization: Bearer YOUR_KEY` |

**Core Endpoints:**

| Endpoint | Function |
|----------|----------|
| `/v2/aggs/ticker/{ticker}/range/{mult}/{timespan}/{from}/{to}` | Aggregates |
| `/v2/aggs/ticker/{ticker}/prev` | Previous close |
| `/v3/quotes/{ticker}` | NBBO quotes |
| `/v3/trades/{ticker}` | Trades |
| `/v3/reference/tickers` | Ticker details |
| `/v2/snapshot/locale/us/markets/stocks/tickers` | Market snapshot |
| `/vX/reference/financials` | Financial statements |

**Pricing:**

| Plan | API Calls | Historical | Features | Cost (Monthly) |
|------|-----------|------------|----------|----------------|
| Stocks Basic | 5/min | 2 years | EOD, reference, indicators | Free |
| Stocks Starter | Unlimited | 5 years | + 15min delayed, WebSocket | $29 |
| Stocks Developer | Unlimited | 10 years | + Trades | $79 |
| Stocks Advanced | Unlimited | 20+ years | + Real-time, quotes, financials | $199 |

**Notes:**
- No rate limits on premium plans
- 20% discount for annual billing
- 50% startup discount available (first year)

---

### 7. Financial Modeling Prep (FMP)

**Comprehensive Fundamentals API**

| Attribute | Details |
|-----------|---------|
| **Base URL** | `https://financialmodelingprep.com/api/v3` |
| **Documentation** | https://site.financialmodelingprep.com/developer/docs |
| **Authentication** | Query: `?apikey=YOUR_KEY` |
| **Format** | JSON, CSV |

**Core Endpoints:**

| Category | Endpoints |
|----------|-----------|
| Quotes | `/quote/{symbol}`, `/quote-short/{symbol}` |
| Historical | `/historical-price-full/{symbol}`, `/historical-chart/{timeframe}/{symbol}` |
| Financials | `/income-statement/{symbol}`, `/balance-sheet-statement/{symbol}`, `/cash-flow-statement/{symbol}` |
| Ratios | `/ratios/{symbol}`, `/key-metrics/{symbol}` |
| Profile | `/profile/{symbol}`, `/company-outlook` |
| News | `/stock_news`, `/press-releases/{symbol}` |
| Calendar | `/earning_calendar`, `/ipo_calendar` |
| Crypto | `/cryptocurrency/{symbol}`, `/quotes/crypto` |
| Forex | `/fx/{pair}`, `/quotes/forex` |

**Pricing:**

| Plan | API Calls/Min | Bandwidth | History | Coverage | Cost |
|------|---------------|-----------|---------|----------|------|
| Basic (Free) | 250/day | 500MB | EOD only | Sample (87 symbols) | $0 |
| Starter | 300/min | 20GB | 5 years | US only | $22/mo (annual) |
| Premium | 750/min | 50GB | 30 years | US, UK, Canada | $59/mo (annual) |
| Ultimate | 3,000/min | 150GB | Full | Global | $149/mo (annual) |

---

## MCP Servers (Detailed)

### 1. Financial Datasets MCP Server

**AI-Native Stock Data Access**

| Attribute | Details |
|-----------|---------|
| **Repository** | https://github.com/financial-datasets/mcp-server |
| **Requirements** | Python 3.10+, UV package manager |
| **Data Source** | Financial Datasets API |

**Installation:**
```bash
git clone https://github.com/financial-datasets/mcp-server
cd mcp-server
uv venv && source .venv/bin/activate
uv add "mcp[cli]" httpx
cp .env.example .env  # Add FINANCIAL_DATASETS_API_KEY
uv run server.py
```

**Claude Desktop Configuration:**
```json
{
  "mcpServers": {
    "financial-datasets": {
      "command": "/path/to/uv",
      "args": ["--directory", "/path/to/mcp-server", "run", "server.py"],
      "env": {
        "FINANCIAL_DATASETS_API_KEY": "your-api-key"
      }
    }
  }
}
```

**Available Tools:**

| Tool | Function |
|------|----------|
| `get_income_statements` | Retrieve income statements |
| `get_balance_sheets` | Retrieve balance sheets |
| `get_cash_flow_statements` | Retrieve cash flow statements |
| `get_current_price` | Get current stock price |
| `get_historical_prices` | Get historical price data |
| `get_company_news` | Get company news |
| `get_available_crypto_tickers` | List crypto tickers |
| `get_crypto_price_snapshot` | Current crypto price |
| `get_crypto_historical_prices` | Historical crypto prices |

**Example Queries:**
- "What are Apple's recent income statements?"
- "Show me the current price of Tesla stock"
- "Get historical prices for MSFT from 2024-01-01 to 2024-12-31"

**Pricing:** Uses Financial Datasets API pricing (see above)

---

### 2. Alpha Vantage MCP Server

**Official Alpha Vantage MCP Integration**

| Attribute | Details |
|-----------|---------|
| **URL** | https://mcp.alphavantage.co/ |
| **Remote Server** | `https://mcp.alphavantage.co/mcp?apikey=YOUR_KEY` |
| **Local Command** | `uvx av-mcp YOUR_API_KEY` |

**Supported Platforms:**
- Claude Desktop/Pro
- ChatGPT (developer mode)
- VS Code
- Cursor
- Claude Code
- Gemini CLI
- OpenAI Agents

**Claude Desktop Configuration:**
```json
{
  "mcpServers": {
    "alpha-vantage": {
      "command": "uvx",
      "args": ["av-mcp", "YOUR_API_KEY"]
    }
  }
}
```

**Available Tool Categories:**

| Category | Tools |
|----------|-------|
| Core Stock APIs | TIME_SERIES_INTRADAY, TIME_SERIES_DAILY, GLOBAL_QUOTE, SYMBOL_SEARCH, MARKET_STATUS |
| Options Data | REALTIME_OPTIONS, HISTORICAL_OPTIONS |
| Intelligence | NEWS_SENTIMENT, EARNINGS_CALL_TRANSCRIPT, TOP_GAINERS_LOSERS, INSIDER_TRANSACTIONS |
| Fundamentals | COMPANY_OVERVIEW, INCOME_STATEMENT, BALANCE_SHEET, CASH_FLOW, EARNINGS |
| Forex | FX_INTRADAY, FX_DAILY, FX_WEEKLY, FX_MONTHLY |
| Crypto | DIGITAL_CURRENCY_DAILY, DIGITAL_CURRENCY_WEEKLY |
| Commodities | WTI, BRENT, COPPER, WHEAT, CORN, etc. |
| Economic | REAL_GDP, CPI, UNEMPLOYMENT, TREASURY_YIELD |
| Technical | SMA, EMA, MACD, RSI, BBANDS, ATR + 40 more |

**Category Filtering:**
```
?categories=core_stock_apis,alpha_intelligence,fundamentals
```

**Pricing:** Uses Alpha Vantage API pricing (see above)

---

### 3. Twelve Data MCP Server

**Real-Time Financial Data with AI Router**

| Attribute | Details |
|-----------|---------|
| **Repository** | https://github.com/twelvedata/mcp |
| **Package** | `mcp-server-twelve-data` |
| **Requirements** | Twelve Data API Key + OpenAI API Key |

**Installation:**
```bash
# UV (recommended)
uvx mcp-server-twelve-data --help

# Pip
pip install mcp-server-twelve-data
```

**Claude Desktop Configuration:**
```json
{
  "mcpServers": {
    "twelvedata": {
      "command": "uvx",
      "args": [
        "mcp-server-twelve-data@latest",
        "-k", "YOUR_TWELVEDATA_KEY",
        "-u", "YOUR_OPENAI_KEY"
      ]
    }
  }
}
```

**Features:**
- **u-tool**: AI-powered universal router using GPT-4o
- Natural language queries to 100+ Twelve Data endpoints
- Real-time WebSocket streaming
- Market data, quotes, fundamentals, technical indicators

**Example Queries:**
- "Show me Apple stock performance this week"
- "What are the latest earnings for Tesla?"
- "Calculate RSI for MSFT over 14 days"

**Data Coverage:**
- Market data & quotes
- 100+ technical indicators
- Fundamental data & financials
- Currencies & crypto
- Mutual funds & ETFs
- Economic calendars

**Pricing:**
- Twelve Data API subscription (see above)
- OpenAI API usage for u-tool

---

### 4. Financial Modeling Prep MCP Server

**Comprehensive Financial Data MCP**

| Attribute | Details |
|-----------|---------|
| **Repository** | https://github.com/imbenrabi/Financial-Modeling-Prep-MCP-Server |
| **Package** | `financial-modeling-prep-mcp-server` |
| **Tools** | 253+ financial data tools |

**Installation:**
```bash
# NPM
npm install financial-modeling-prep-mcp-server
npx fmp-mcp --fmp-token=YOUR_TOKEN

# Docker
docker run -p 8080:8080 -e FMP_ACCESS_TOKEN=YOUR_TOKEN \
  ghcr.io/imbenrabi/financial-modeling-prep-mcp-server:latest
```

**Configuration Modes:**

| Mode | Description | Config |
|------|-------------|--------|
| Dynamic | 3 meta-tools, load on-demand | `--dynamic-tool-discovery` |
| Static | Pre-load specified toolsets | `FMP_TOOL_SETS=search,company,quotes` |
| Legacy | Load all 253+ tools at start | Default |

**Tool Categories (20+):**

| Category | Tools |
|----------|-------|
| search | Symbol search, CIK lookup |
| company | Profile, executives, outlook |
| quotes | Real-time quotes, price changes |
| statements | Income, balance sheet, cash flow |
| calendar | Earnings, IPO, dividends |
| charts | Historical prices, intraday |
| news | Stock news, press releases |
| analyst | Estimates, recommendations |
| market-performance | Gainers, losers, sectors |
| insider-trades | Insider transactions |
| institutional | 13F filings, holdings |
| indexes | Index constituents |
| economics | GDP, CPI, treasury rates |
| crypto | Crypto quotes, prices |
| forex | FX rates, pairs |
| commodities | Commodity prices |
| etf-funds | ETF holdings, performance |
| esg | ESG scores, ratings |
| technical-indicators | SMA, EMA, RSI, MACD |
| senate | Senate trading data |
| sec-filings | 10-K, 10-Q, 8-K filings |
| dcf | Discounted cash flow |
| bulk | Bulk data downloads |

**Pricing:** Uses FMP API pricing (see above)

---

## MCP Server Pricing Summary

| MCP Server | API Provider | Free Tier | Paid Plans |
|------------|--------------|-----------|------------|
| Financial Datasets MCP | Financial Datasets | Pay-as-you-go from $0.01/req | $200-$2,000/mo |
| Alpha Vantage MCP | Alpha Vantage | 25 req/day | $49.99-$249.99/mo |
| Twelve Data MCP | Twelve Data + OpenAI | 8 API credits/min (800/day) | $79-$1,999/mo + OpenAI |
| FMP MCP | Financial Modeling Prep | 250 calls/day | $22-$149/mo |

---

## AWS Architecture Patterns

### Pattern 1: Real-Time/Near Real-Time

```
API (SET/Settrade)
    → AWS Lambda (scheduled/EventBridge)
    → Amazon Kinesis Data Firehose
    → Amazon Timestream / Aurora
    → QuickSight (Direct Query)
```

### Pattern 2: Historical/Batch Data

```
API (SET SMART/Alpha Vantage)
    → AWS Lambda / Glue ETL
    → Amazon S3 (Data Lake)
    → Amazon Athena
    → QuickSight (SPICE or Direct Query)
```

### AWS Services Overview

| Service | Use Case | Notes |
|---------|----------|-------|
| **Lambda** | ETL for smaller jobs, API polling | Cost-effective, max 15 min execution |
| **Glue** | Complex transformations, schema discovery | Serverless ETL |
| **Kinesis** | Real-time streaming ingestion | High throughput |
| **S3** | Durable storage with lifecycle policies | Data lake foundation |
| **Athena** | Serverless SQL queries on S3 | Pay per query |
| **Redshift** | Data warehouse | Large-scale analytics |
| **Timestream** | Time-series data | Optimized for IoT/metrics |
| **EventBridge** | Scheduled triggers | Cron-like scheduling |

---

## Recommended Implementation

### For Thailand Portfolio Dashboard

**Primary Data Sources:**
1. **SET SMART Marketplace** - Official Thai market data
2. **Settrade Open API** - Real-time trading data (sandbox for testing)
3. **FundConnext** - Mutual fund NAV data
4. **Bank of Thailand API** - Exchange rates, interest rates

**Supplementary Data:**
5. **Twelve Data** or **Alpha Vantage** - International holdings, forex
6. **Financial Datasets** - US fundamentals (if needed)

### Integration Steps

1. Use **AWS Lambda** (scheduled via EventBridge) to poll APIs
2. Store raw data in **S3** (cost-effective, compliant)
3. Use **Athena** for ad-hoc queries or **Redshift** for complex analytics
4. Connect **QuickSight** to Athena/Redshift for dashboards
5. Use **SPICE** for frequently accessed dashboards (hourly refresh)

---

## Complete Pricing Summary

### Thailand Data Sources

| Source | Cost | Notes |
|--------|------|-------|
| SET SMART API | Contact SET | Some data free in trial |
| Settrade Open API | Free | Via broker relationship |
| Bank of Thailand API | Free | Registration required |
| FundConnext | Via distributor | Through broker/AMC |

### Global APIs

| API | Free Tier | Entry Paid | Full Access |
|-----|-----------|------------|-------------|
| Alpha Vantage | 25 req/day | $49.99/mo | $249.99/mo |
| Twelve Data | 800 req/day | $79/mo | $999/mo |
| Financial Datasets | Pay-per-request | $200/mo | $2,000/mo |
| EODHD | 20 req/day | $19.99/mo | $99.99/mo |
| Marketstack | 100 req/mo | $9.99/mo | $149.99/mo |
| Polygon.io | 5 req/min | $29/mo | $199/mo |
| FMP | 250 req/day | $22/mo | $149/mo |

### AWS Services

| Service | Cost Model |
|---------|------------|
| QuickSight Author | $18-24/month |
| QuickSight Reader | $0.30/session |
| Lambda | ~$0.20 per 1M requests |
| S3 | ~$0.023/GB/month |
| Athena | $5 per TB scanned |
| Redshift Serverless | Pay for compute used |
| Kinesis | $0.015/shard hour + data |
| EventBridge | $1 per 1M events |

---

## Third-Party Libraries (Thailand)

| Library | Language | Source |
|---------|----------|--------|
| **ThaiStock** | Python | [GitHub](https://github.com/UncleEngineer/ThaiStock) |
| **Thai Stock 2D API** | REST | [thaistock2d.com](https://docs.thaistock2d.com/) |
| **trading-stock-thailand** | Python | [GitHub](https://github.com/adminho/trading-stock-thailand) |

---

## References

### Official Thailand Sources
- [SET SMART Marketplace](https://www.set.or.th/en/services/connectivity-and-data/data/smart-marketplace)
- [Settrade Open API](https://developer.settrade.com/open-api/)
- [FundConnext](https://www.set.or.th/en/dap/services/fundconnext)
- [SET Market Data API Specification (PDF)](https://media.set.or.th/set/Documents/2023/May/Market_Data_API_Service_Specification.pdf)
- [Bank of Thailand API Portal](https://apiportal.bot.or.th/bot/public/)
- [BOT API New Portal](https://portal.api.bot.or.th/)

### Global API Documentation
- [Alpha Vantage Documentation](https://www.alphavantage.co/documentation/)
- [Alpha Vantage Pricing](https://www.alphavantage.co/premium/)
- [Twelve Data Documentation](https://twelvedata.com/docs)
- [Twelve Data Pricing](https://twelvedata.com/pricing)
- [Financial Datasets Documentation](https://docs.financialdatasets.ai/)
- [Financial Datasets Pricing](https://www.financialdatasets.ai/pricing)
- [EODHD Documentation](https://eodhd.com/financial-apis/)
- [EODHD Pricing](https://eodhd.com/pricing)
- [Marketstack Documentation](https://marketstack.com/documentation_v2)
- [Marketstack Pricing](https://marketstack.com/pricing)
- [Polygon.io Documentation](https://polygon.io/docs)
- [Polygon.io/Massive Pricing](https://massive.com/pricing)
- [FMP Documentation](https://site.financialmodelingprep.com/developer/docs)
- [FMP Pricing](https://site.financialmodelingprep.com/pricing-plans)

### MCP Server Repositories
- [Financial Datasets MCP](https://github.com/financial-datasets/mcp-server)
- [Alpha Vantage MCP](https://mcp.alphavantage.co/)
- [Twelve Data MCP](https://github.com/twelvedata/mcp)
- [Financial Modeling Prep MCP](https://github.com/imbenrabi/Financial-Modeling-Prep-MCP-Server)

### AWS Documentation
- [Amazon QuickSight Data Sources](https://docs.aws.amazon.com/quicksight/latest/user/working-with-data.html)
- [Connect QuickSight to External API](https://repost.aws/questions/QU8W15WgMfRb-qYIJzPuzA-g/connect-quicksight-to-external-api)
- [AWS Lambda ETL for Asset Management](https://aws.amazon.com/blogs/industries/etl-ingest-architecture-for-asset-management-based-on-aws-lambda/)

---

## Related

- [[research/aws/_index|AWS Research MOC]]

---

## Log

| Date | Update |
|------|--------|
| 2025-12-12 | Initial research completed |
| 2025-12-12 | Added detailed API specifications, endpoints, request/response examples, and comprehensive pricing |
