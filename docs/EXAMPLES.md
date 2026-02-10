# Portfolio Optimization Examples

This document provides real-world examples and case studies using the Portfolio Optimization Tooling.

## Table of Contents
1. [Quick Reference Examples](#quick-reference-examples)
2. [Sector-Specific Portfolios](#sector-specific-portfolios)
3. [Risk Tolerance Scenarios](#risk-tolerance-scenarios)
4. [Market Condition Analysis](#market-condition-analysis)
5. [Case Studies](#case-studies)

## Quick Reference Examples

### Example 1: Classic 60/40 Portfolio Simulation

**Objective**: Simulate a traditional 60% stocks / 40% bonds allocation

```python
# Date range
date1 = '2018-01-01'
date2 = '2023-01-01'

# Tickers
SPY, AGG  # S&P 500 ETF, Aggregate Bond ETF

# Expected results
# - Moderate risk (~10-12% std dev)
# - Moderate return (~8-10% annual)
# - Strong Sharpe ratio (~0.7-1.0)
```

**Target Risk**: 0.12 (12% volatility)

**Interpretation**: This balanced approach has historically provided steady growth with moderate volatility, suitable for retirement accounts.

---

### Example 2: Tech Growth Portfolio

**Objective**: Capture growth in technology sector

```python
# Date range
date1 = '2020-01-01'
date2 = '2023-01-01'

# Tickers (FAANG + semiconductors)
AAPL, AMZN, NVDA, GOOGL, META, MSFT, AMD, INTC

# Expected results
# - High risk (25-35% std dev)
# - High return (20-40% annual)
# - Variable Sharpe ratio (0.5-1.5)
```

**Target Risk**: 0.30 (30% volatility)

**Sample Output**:
```
Optimal Weights:
NVDA: 28.5%    # AI/GPU leader
MSFT: 22.3%    # Cloud & AI
AAPL: 18.7%    # Consumer tech
GOOGL: 15.2%   # Search & Cloud
AMZN: 10.1%    # E-commerce & Cloud
META: 5.2%     # Social media
AMD: 0%        # Too correlated with NVDA
INTC: 0%       # Underperformer

Portfolio Return: 32.4% (annualized)
Portfolio Risk: 30.0% (annualized)
Sharpe Ratio: 1.05
```

**Key Insights**:
- Optimization favors NVDA (strong returns, reasonable volatility)
- MSFT provides stability
- AMD and INTC get zero allocation (dominated by other assets)

---

### Example 3: Dividend Income Portfolio

**Objective**: Generate stable income with capital preservation

```python
# Date range
date1 = '2019-01-01'
date2 = '2024-01-01'

# Tickers (dividend aristocrats)
JNJ, PG, KO, PEP, MCD, WMT, TGT, VZ, T, XOM

# Expected results
# - Low risk (12-18% std dev)
# - Moderate return (8-12% annual)
# - High Sharpe ratio (0.8-1.2)
```

**Target Risk**: 0.15 (15% volatility)

**Sample Output**:
```
Optimal Weights:
WMT: 18.2%     # Retail staples
PG: 16.5%      # Consumer goods
JNJ: 15.8%     # Healthcare
KO: 12.3%      # Beverages
XOM: 11.7%     # Energy (dividends)
PEP: 10.9%     # Beverages/snacks
MCD: 8.4%      # Quick service
TGT: 6.2%      # Retail
VZ: 0%         # Telecom weakness
T: 0%          # High debt concerns

Portfolio Return: 10.2% (annualized)
Portfolio Risk: 15.0% (annualized)
Sharpe Ratio: 0.55 (with 2% risk-free rate)
```

**Use Case**: Retirees seeking steady income with lower volatility.

---

## Sector-Specific Portfolios

### Technology Sector

**Pure Tech Play**:
```python
# Software & Cloud
MSFT, ORCL, CRM, ADBE, NOW

# Semiconductors
NVDA, AMD, TSM, AVGO

# Internet
GOOGL, META, AMZN
```

**Characteristics**:
- High growth potential
- High volatility (30-40% std dev)
- Cyclical with economy
- Rapid innovation and disruption

**Optimal for**: Young investors with long time horizons and high risk tolerance.

---

### Healthcare Sector

**Defensive Healthcare**:
```python
# Pharmaceuticals
JNJ, PFE, MRK, ABBV, LLY

# Medical Devices
MDT, ABT, SYK, TMO

# Healthcare Services
UNH, CVS, HCA
```

**Characteristics**:
- Defensive (less correlated with economy)
- Moderate volatility (15-20% std dev)
- Aging demographics tailwind
- Regulatory risks

**Optimal for**: Risk-averse investors seeking stability with growth.

---

### Financial Sector

**Banking & Finance**:
```python
# Large Banks
JPM, BAC, WFC, C

# Investment Banks
GS, MS

# Insurance
BRK.B, AIG

# Payments
V, MA, PYPL
```

**Characteristics**:
- Interest rate sensitive
- Economic cycle dependent
- Moderate-high volatility (20-25% std dev)
- Dividend income

**Optimal for**: Investors bullish on economic growth and rising rates.

---

### Energy Sector

**Energy Mix**:
```python
# Oil & Gas
XOM, CVX, COP, EOG

# Renewables
NEE, ENPH, FSLR

# Utilities
DUK, SO, D

# Energy Services
SLB, HAL
```

**Characteristics**:
- Commodity price exposure
- High volatility (25-30% std dev)
- Geopolitical risks
- Energy transition underway

**Optimal for**: Inflation hedgers, commodity cycle players.

---

### Consumer Sector

**Consumer Mix**:
```python
# Discretionary
AMZN, HD, NKE, SBUX, TGT

# Staples
PG, KO, PEP, WMT, COST

# Luxury
LVMH, TIF
```

**Characteristics**:
- Staples: Defensive, low volatility (10-15%)
- Discretionary: Cyclical, higher volatility (20-25%)
- Brand power important
- E-commerce disruption

**Optimal for**: Balanced exposure to consumer spending.

---

## Risk Tolerance Scenarios

### Conservative Investor Profile

**Demographics**: Retiree, 5-10 year time horizon, cannot afford losses

**Portfolio**:
```python
# 70% Bonds / 30% Stocks
AGG, BND, LQD, TIP     # 70% allocation
SPY, VTI               # 30% allocation

# Target Risk: 0.08 (8% volatility)
```

**Expected Outcomes**:
- Return: 5-7% annually
- Risk: 7-9% std dev
- Sharpe: 0.4-0.6
- Maximum drawdown: ~10-12%

**Rebalancing**: Quarterly

---

### Moderate Investor Profile

**Demographics**: Mid-career professional, 15-20 year horizon, some risk tolerance

**Portfolio**:
```python
# 60% Stocks / 30% Bonds / 10% Alternatives
SPY, VTI, VEA          # US & International stocks (60%)
AGG, LQD               # Bonds (30%)
GLD, VNQ               # Gold & Real Estate (10%)

# Target Risk: 0.15 (15% volatility)
```

**Expected Outcomes**:
- Return: 8-10% annually
- Risk: 14-16% std dev
- Sharpe: 0.5-0.7
- Maximum drawdown: ~18-22%

**Rebalancing**: Semi-annually

---

### Aggressive Investor Profile

**Demographics**: Young professional, 25+ year horizon, high risk tolerance

**Portfolio**:
```python
# 100% Stocks - Growth focused
QQQ, VTI, VWO, ARKK     # US Growth & Emerging (80%)
VEA, EEM                # International (20%)

# Target Risk: 0.25 (25% volatility)
```

**Expected Outcomes**:
- Return: 12-15% annually
- Risk: 23-27% std dev
- Sharpe: 0.5-0.8
- Maximum drawdown: ~30-40%

**Rebalancing**: Annually

---

### Very Aggressive Investor Profile

**Demographics**: High net worth, 30+ year horizon, seeking maximum growth

**Portfolio**:
```python
# 100% Stocks - High concentration
NVDA, TSLA, MSFT, AMZN, GOOGL, META
# Individual high-growth stocks

# Target Risk: 0.35+ (35%+ volatility)
```

**Expected Outcomes**:
- Return: 15-25% annually (highly variable)
- Risk: 35-45% std dev
- Sharpe: 0.4-0.7 (lower due to high vol)
- Maximum drawdown: ~50-60%

**Rebalancing**: Annually or opportunistically

---

## Market Condition Analysis

### Bull Market Strategy (2019-2020)

**Period**: January 2019 - February 2020 (pre-COVID)

**Characteristics**:
- Low volatility
- Strong tech leadership
- Low interest rates

**Optimal Portfolio** (Hindsight):
```python
date1 = '2019-01-01'
date2 = '2020-02-01'

# Tickers
AAPL, MSFT, NVDA, AMZN, GOOGL, TSLA

# Results
# Average return: 40%+ annually
# Risk: 20-25% std dev
# Sharpe: 1.5-2.0 (exceptional)
```

**Lesson**: Momentum and growth stocks dominate in bull markets with strong fundamentals.

---

### Crisis Period (COVID-19 Crash)

**Period**: February 2020 - March 2020

**Characteristics**:
- Extreme volatility spikes
- Flight to safety
- Correlations approach 1.0

**What Happened**:
```python
date1 = '2020-02-01'
date2 = '2020-04-01'

# Losses
SPY: -34%
QQQ: -30%
Most stocks: -20% to -50%

# Winners
GLD (Gold): +4%
TLT (Long bonds): +15%
ZM (Zoom): +25%
```

**Lesson**: Diversification fails during extreme crises. Cash and safe havens outperform.

---

### Recovery Period (2020-2021)

**Period**: April 2020 - December 2021

**Characteristics**:
- V-shaped recovery
- Massive fiscal stimulus
- Tech acceleration (work from home)

**Optimal Portfolio**:
```python
date1 = '2020-04-01'
date2 = '2021-12-31'

# Tickers
TSLA, NVDA, AAPL, MSFT, ZM, SHOP, SQ

# Results
# Average return: 80%+ annually
# Risk: 35-45% std dev
# Sharpe: 1.5-2.0
```

**Lesson**: Post-crisis recoveries favor risk assets and damaged sectors that recover.

---

### Rising Rate Environment (2022)

**Period**: January 2022 - December 2022

**Characteristics**:
- Fed hiking rates aggressively
- Tech de-rating (lower P/E ratios)
- Energy outperformance

**Optimal Portfolio**:
```python
date1 = '2022-01-01'
date2 = '2022-12-31'

# Winners
XOM, CVX, OXY (Energy): +40-60%
XLE (Energy ETF): +65%

# Losers
QQQ: -33%
ARKK: -67%
Most tech: -40% to -80%

# Optimal mix
XOM: 30%
CVX: 25%
SPY: 20%
UNH: 15%
WMT: 10%
```

**Lesson**: Rising rates favor value stocks, energy, and defensive sectors. Growth stocks underperform.

---

## Case Studies

### Case Study 1: The Magnificent Seven (2023)

**Question**: Should you invest equally in the "Magnificent Seven" tech stocks?

**Setup**:
```python
date1 = '2023-01-01'
date2 = '2023-12-31'

# Equal Weight
AAPL, MSFT, GOOGL, AMZN, NVDA, TSLA, META
# Each gets 14.3% allocation
```

**Equal Weight Results**:
- Return: 62% (exceptional)
- Risk: 28% std dev
- Sharpe: 2.1

**Optimized Results**:
```
Optimal Weights:
NVDA: 42%      # AI beneficiary
META: 23%      # Recovery story
MSFT: 18%      # AI + Cloud
GOOGL: 12%     # AI play
AMZN: 5%       # Lower beta
AAPL: 0%       # Mature, lower growth
TSLA: 0%       # High volatility, lower Sharpe

Portfolio Return: 95%
Portfolio Risk: 28%
Sharpe Ratio: 3.2
```

**Insights**:
- Optimization dramatically outperformed equal weight (+33% return)
- Concentrated in AI beneficiaries (NVDA, META, MSFT)
- Eliminated mature (AAPL) and volatile (TSLA) names
- Same risk, much higher return

**Takeaway**: Equal weighting is not optimal. Data-driven allocation matters.

---

### Case Study 2: Diversification Failure (2008 Crisis)

**Question**: Did diversification work during the 2008 financial crisis?

**Setup**:
```python
date1 = '2008-01-01'
date2 = '2009-03-01'

# Well-diversified portfolio
SPY, IWM, VEA, VWO, VNQ, GLD, TLT, AGG
```

**Results**:
```
Losses:
SPY (Large Cap): -51%
IWM (Small Cap): -54%
VEA (Int'l Developed): -50%
VWO (Emerging): -53%
VNQ (Real Estate): -68%
AGG (Bonds): -2%

Winners:
TLT (Long Bonds): +26%
GLD (Gold): +5%
```

**Optimized Portfolio** (Hindsight):
```
TLT: 60%
GLD: 30%
AGG: 10%

Portfolio Return: +15% (during crisis!)
Portfolio Risk: 12%
```

**Actual Typical Portfolio**:
```
60% Stocks / 40% Bonds
Return: -27%
Risk: 18%
```

**Insights**:
- Stock diversification (US vs Int'l, Large vs Small) didn't help
- Correlations approached 1.0 during crisis
- Only bonds and gold provided protection
- Real estate correlated with stocks due to credit crisis

**Takeaway**: 
- Diversification across stocks is not enough
- Need uncorrelated asset classes (bonds, gold, alternatives)
- Crisis periods break normal correlation structures

---

### Case Study 3: Value vs Growth (2000-2002 Dot-com Crash)

**Question**: How should you allocate between value and growth stocks?

**Setup**:
```python
date1 = '2000-03-01'
date2 = '2002-10-01'

# Growth
QQQ (Nasdaq)

# Value
IWD (Russell 1000 Value)
```

**Results**:
```
QQQ (Growth): -78%
IWD (Value): -20%

If optimized over 1995-2000 (before crash):
- Allocation: 100% QQQ (growth had been winning)
- Actual return: -78%

If diversified 50/50:
- Return: -49%
- Still painful, but much better
```

**Insight**:
- Optimization using recent data can be dangerous
- Past winners become future losers
- Style rotation (growth/value) is unpredictable
- Diversification across styles is prudent

**Takeaway**: Don't optimize solely on recent performance. Balance growth and value.

---

### Case Study 4: International Diversification

**Question**: Should US investors include international stocks?

**Test Period**: 2010-2020

**Portfolio A** (US Only):
```python
SPY: 100%

Return: 13.9% annually
Risk: 15.1% std dev
Sharpe: 0.85
```

**Portfolio B** (Globally Diversified):
```python
SPY: 60%   # US Large Cap
VEA: 25%   # Developed Int'l
VWO: 15%   # Emerging Markets

Return: 10.2% annually
Risk: 14.8% std dev
Sharpe: 0.62
```

**Results**:
- US-only portfolio outperformed
- International provided little diversification benefit
- US exceptionalism period

**Alternative Test Period**: 2000-2010

**Portfolio A** (US Only):
```python
SPY: 100%

Return: -0.9% annually (lost decade!)
Risk: 19.2% std dev
Sharpe: -0.10 (negative!)
```

**Portfolio B** (Globally Diversified):
```python
SPY: 60%   # US Large Cap
VEA: 25%   # Developed Int'l
VWO: 15%   # Emerging Markets

Return: 2.8% annually
Risk: 16.4% std dev
Sharpe: 0.12
```

**Results**:
- International diversification helped significantly
- Emerging markets strong during this period
- US underperformed

**Takeaway**: 
- International diversification benefits vary by period
- US has outperformed recently (2010-2020)
- But past US dominance doesn't guarantee future dominance
- Long-term investors should maintain some international exposure

---

### Case Study 5: Rebalancing Benefits

**Question**: Does rebalancing improve returns?

**Setup**:
```python
date1 = '2018-01-01'
date2 = '2023-01-01'

# Initial: 50% SPY / 50% AGG
```

**Strategy A** (Buy and Hold - No Rebalancing):
```
Initial (2018): 50% SPY / 50% AGG

After 5 years (2023): 73% SPY / 27% AGG
# Stock growth caused drift

Return: 8.2% annually
Risk: 13.1% std dev
Final balance: $148,000 (from $100,000)
```

**Strategy B** (Annual Rebalancing):
```
Rebalance to 50/50 every year

Return: 8.9% annually (+0.7% from rebalancing!)
Risk: 11.2% std dev (lower!)
Final balance: $152,700 (from $100,000)
```

**Benefits**:
- Higher return (+$4,700 over 5 years)
- Lower risk (maintained target allocation)
- Disciplined "sell high, buy low"

**Cost**:
- Transaction costs (minimal with modern brokers)
- Potential tax implications (use tax-advantaged accounts)

**Takeaway**: 
- Rebalancing is valuable
- Annual or semi-annual is sufficient
- Use 5% drift thresholds to minimize trading

---

## Practical Tips from Examples

1. **Start with the efficient frontier**: Always visualize before optimizing
2. **Consider multiple time periods**: Different periods favor different strategies
3. **Don't over-concentrate**: Even if optimal, high concentration is risky
4. **Rebalance regularly**: Maintain your risk profile
5. **Include bonds/cash**: Especially as you age
6. **Test your assumptions**: Use multiple historical periods
7. **Account for costs**: In real implementations
8. **Stay diversified**: Across sectors, geographies, and asset classes
9. **Update regularly**: Markets evolve; so should your portfolio
10. **Keep it simple**: Complex doesn't mean better

---

For more information, see:
- [README.md](../README.md) - Project overview
- [USAGE.md](USAGE.md) - Detailed usage instructions
- [ALGORITHMS.md](ALGORITHMS.md) - Technical details
