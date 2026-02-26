---
title: "QuickSight Financial Dashboard Plan"
tags: [research, aws, quicksight, fintech]
created: 2025-12-18
---

# QuickSight Financial Advisor Dashboard Plan

**Date:** 2025-12-12  
**Purpose:** Design dashboards for financial advisors using Thai financial news knowledge base

---

## Dashboard Types

### 1. Portfolio Performance Dashboard
**Key Metrics:**
- Total portfolio value & YTD return
- Asset allocation (pie chart: stocks, bonds, cash, alternatives)
- Top 10 holdings performance
- Sector exposure breakdown
- Risk metrics (Sharpe ratio, volatility, beta)

**Visuals:**
- Line chart: Portfolio value over time
- Waterfall chart: Monthly/quarterly returns
- Heat map: Asset class performance

---

### 2. Market Overview Dashboard
**Key Metrics:**
- Major indices (SET, S&P 500, MSCI Asia)
- Currency rates (THB/USD, THB/EUR)
- Commodity prices (gold, oil)
- Bond yields (Thai govt bonds, US Treasury)

**Visuals:**
- KPI cards: Daily/weekly changes
- Sparklines: Intraday trends
- Comparison table: Regional market performance

---

### 3. Client Risk Profile Dashboard
**Key Metrics:**
- Risk tolerance score
- Current vs target allocation
- Concentration risk (single stock >10%)
- Liquidity ratio
- Time to retirement/goal

**Visuals:**
- Gauge chart: Risk alignment
- Stacked bar: Current vs recommended allocation
- Scatter plot: Risk vs return by holding

---

### 4. Income & Cash Flow Dashboard
**Key Metrics:**
- Dividend income (monthly/annual)
- Interest income
- Upcoming distributions
- Tax efficiency metrics
- Withdrawal sustainability

**Visuals:**
- Area chart: Projected income stream
- Calendar heat map: Distribution schedule
- Funnel: Income sources breakdown

---

### 5. Market Sentiment & News Dashboard
**Key Metrics:**
- News sentiment score by sector
- Analyst ratings changes
- Earnings calendar
- Economic indicators (GDP, inflation, rates)
- Social media sentiment

**Visuals:**
- Sentiment trend line
- Word cloud: Trending topics
- Timeline: Key events & market reactions

---

### 6. Goal Tracking Dashboard
**Key Metrics:**
- Progress to retirement goal (%)
- Education fund status
- Emergency fund adequacy
- Projected vs required savings rate

**Visuals:**
- Progress bars: Multiple goals
- Scenario analysis: Monte Carlo projections
- Gap analysis: Shortfall/surplus

---

### 7. Tax Optimization Dashboard
**Key Metrics:**
- Realized/unrealized gains
- Tax loss harvesting opportunities
- Dividend tax impact
- Asset location efficiency

**Visuals:**
- Sankey diagram: Tax-efficient placement
- Bar chart: Tax drag by account type

---

## Data Structure

### Portfolio Holdings
```csv
date,client_id,ticker,asset_class,sector,quantity,price,market_value,cost_basis,unrealized_gain_pct
2025-12-12,C001,PTT,Stock,Energy,1000,35.50,35500,32000,10.94
2025-12-12,C001,KBANK,Stock,Banking,500,145.00,72500,70000,3.57
```

### Market Data
```csv
date,index_name,value,change_pct,volume
2025-12-12,SET,1245.67,-0.85,45000000000
2025-12-12,SET50,850.23,-1.02,28000000000
```

### News Sentiment (from Knowledge Base)
```csv
date,source,headline,sentiment_score,category,related_tickers
2025-12-12,Bangkok Post,BoT holds rates,0.5,monetary_policy,"KBANK,SCB,BBL"
2025-12-12,Nation,DELTA sell-off,-0.7,stock_market,DELTA
```

---

## QuickSight Data Source Options

**For Financial Data:**
1. **Amazon RDS/Aurora** - Real-time portfolio data
2. **Amazon Redshift** - Historical analysis (>1 year data)
3. **S3 + Athena** - Cost-effective for daily snapshots
4. **Direct Upload** - CSV files for static reference data
5. **API Connector** - Live market data (Alpha Vantage, Yahoo Finance)

---

## Recommended Starting Dashboard

**Essential Dashboard for Thai Financial Advisors:**

```
┌─────────────────────────────────────────────┐
│ Portfolio Summary (Top KPIs)                │
│ Total Value | YTD Return | Risk Score      │
├─────────────────────────────────────────────┤
│ Asset Allocation (Pie) | Performance (Line)│
├─────────────────────────────────────────────┤
│ Top Holdings (Table) | Sector Exposure (Bar)│
├─────────────────────────────────────────────┤
│ Market News Sentiment (from Knowledge Base) │
│ Recent Headlines + Sentiment Scores         │
└─────────────────────────────────────────────┘
```

**Filters:**
- Date range
- Client ID
- Asset class
- Sector

---

## Thai Financial News Sources

**English Sources:**
- Bangkok Post (https://www.bangkokpost.com/business)
- The Nation Thailand (https://www.nationthailand.com/business)
- Thailand Business News (https://www.thailand-business-news.com)
- Thai Examiner (https://www.thaiexaminer.com)

**Thai Sources:**
- Kaohoon (https://www.kaohoon.com)
- Thansettakij
- Manager Online

---

## Current Thai Market Trends (Dec 2025)

**Positive Indicators:**
- Inflation returned to target range (1.23% YoY)
- Q3 corporate profits: 886.8B baht
- Tourism recovery ongoing
- GDP growth forecast: 2.9% for 2025

**Challenges:**
- Strong baht hurting exports (+8% vs USD)
- Political uncertainty from early parliament dissolution
- US-China trade tensions affecting exports
- DELTA stock sell-off pressure

---

## Implementation Steps

1. **Data Collection**
   - Connect to existing web crawler knowledge base
   - Set up market data feeds (API/database)
   - Create portfolio data structure

2. **QuickSight Setup**
   - Create datasets from data sources
   - Define calculated fields (returns, allocations)
   - Create topics for natural language queries

3. **Dashboard Development**
   - Start with Portfolio Performance dashboard
   - Add Market Sentiment from knowledge base
   - Iterate based on advisor feedback

4. **Integration**
   - Connect Amazon Q Business for natural language queries
   - Enable scheduled refreshes
   - Set up alerts for significant events

---

## Next Steps

- [ ] Define specific data source (RDS/S3/Redshift)
- [ ] Create sample data structure
- [ ] Design first dashboard layout
- [ ] Connect knowledge base to QuickSight
- [ ] Set up Amazon Q Business integration
