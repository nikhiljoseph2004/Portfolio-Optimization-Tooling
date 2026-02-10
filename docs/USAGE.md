# Usage Guide - Portfolio Optimization Tooling

This guide provides detailed instructions on how to use the Portfolio Optimization Tooling effectively.

## Table of Contents
1. [Getting Started](#getting-started)
2. [Basic Workflow](#basic-workflow)
3. [Detailed Examples](#detailed-examples)
4. [Interpreting Results](#interpreting-results)
5. [Advanced Usage](#advanced-usage)
6. [Troubleshooting](#troubleshooting)

## Getting Started

### Prerequisites
- Python 3.8 or higher
- Jupyter Notebook
- Internet connection (for fetching market data)

### First Time Setup
```bash
# Clone the repository
git clone https://github.com/nikhiljoseph2004/Portfolio-Optimization-Tooling.git
cd Portfolio-Optimization-Tooling

# Install dependencies
pip install -r requirements.txt

# Start Jupyter
jupyter notebook
```

Open `testing.ipynb` in your browser.

## Basic Workflow

### Step 1: Run Imports (Cell 1)
Execute the first cell to import all necessary libraries:
```python
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import minimize
```

### Step 2: Run Function Definitions (Cell 2)
Execute the second cell to load all the portfolio optimization functions. This includes:
- `get_string_array()` - Input handler for stock tickers
- `fetch_stock_data()` - Downloads historical price data
- `calculate_statistics()` - Computes return statistics
- `calculate_efficient_frontier()` - Generates the efficient frontier
- `optimize_portfolio_max_return()` - Maximizes return for given risk
- `optimize_portfolio_min_risk()` - Minimizes risk for given return
- `plot_efficient_frontier()` - Visualizes results

### Step 3: Set Date Range and Risk-Free Rate (Cell 3)
```python
date1 = '2021-01-06'  # Start date
date2 = '2022-01-09'  # End date

# Fetch the risk-free rate
risk_free_rate = fetch_tbill_rate(date1, date2)
```

**Tips for Date Selection:**
- Use at least 6 months of data for reliable statistics
- More data (1-3 years) provides better covariance estimates
- Be aware of market regime changes (e.g., pre/post COVID-19)
- Format: 'YYYY-MM-DD'

### Step 4: Input Stock Tickers (Cell 5)
When prompted, enter your stock tickers separated by commas:
```
Enter stock tickers, separated by commas: AAPL, MSFT, GOOGL, AMZN
```

The system will:
1. Fetch historical data for each ticker
2. Display the closing prices DataFrame
3. Calculate and display daily statistics
4. Generate the efficient frontier (10,000 random portfolios)
5. Display a plot showing risk-return tradeoffs

### Step 5: Optimize Portfolio (Cell 6)
When prompted, enter your target risk (standard deviation):
```
Enter your desired portfolio risk (standard deviation): 0.25
```

The system will output:
- Optimal portfolio weights for each stock
- Expected annual return
- Expected risk (standard deviation)

## Detailed Examples

### Example 1: Tech-Heavy Portfolio

**Objective**: Create a high-growth, tech-focused portfolio

**Input:**
```python
# Date range
date1 = '2020-01-01'
date2 = '2023-01-01'

# Tickers
AAPL, MSFT, GOOGL, NVDA, TSLA, META
```

**Expected Output:**
- High returns (potentially 20-40% annualized)
- High risk (standard deviation 25-40%)
- Strong correlation between assets (tech sector)

**Use Case**: Aggressive growth investors with high risk tolerance

---

### Example 2: Diversified Portfolio

**Objective**: Balance growth and stability across sectors

**Input:**
```python
# Date range
date1 = '2021-01-01'
date2 = '2024-01-01'

# Tickers (8 assets across sectors)
DUK, XOM, CVX, PG, KO, BABA, SPY, VTI
```

This includes:
- **Utilities**: DUK (Duke Energy) - Stable, dividend-paying
- **Energy**: XOM, CVX (Exxon, Chevron) - Commodity exposure
- **Consumer Staples**: PG, KO (Procter & Gamble, Coca-Cola) - Defensive
- **International**: BABA (Alibaba) - Emerging markets
- **Broad Market**: SPY, VTI (S&P 500, Total Market) - Market exposure

**Expected Output:**
- Moderate returns (10-18% annualized)
- Lower risk (standard deviation 15-25%)
- Lower correlation between assets
- Smoother efficient frontier

**Use Case**: Balanced investors seeking diversification

---

### Example 3: Income-Focused Portfolio

**Objective**: Prioritize dividend income and capital preservation

**Input:**
```python
# Date range
date1 = '2019-01-01'
date2 = '2024-01-01'

# Tickers (dividend aristocrats and bonds)
JNJ, PG, KO, T, VZ, PFE
```

**Expected Output:**
- Lower returns (8-15% annualized)
- Lower risk (standard deviation 12-20%)
- High Sharpe ratio due to consistent returns

**Use Case**: Conservative investors, retirees

---

### Example 4: Market-Neutral Strategy

**Objective**: Equal-weight broad market exposure

**Input:**
```python
# Date range
date1 = '2018-01-01'
date2 = '2023-01-01'

# Tickers (index funds)
SPY, QQQ, IWM, VEA, VWO, AGG
```

Components:
- SPY: Large-cap US
- QQQ: Tech-heavy Nasdaq
- IWM: Small-cap US
- VEA: Developed international
- VWO: Emerging markets
- AGG: Bonds

**Target Risk**: 0.15 (15% standard deviation)

**Expected Output:**
- Well-diversified allocation
- Moderate returns (10-15%)
- Risk similar to 60/40 stock/bond portfolio

---

## Interpreting Results

### Understanding Statistics Output

```
Daily Statistics:
               Mean Return    Std Dev    Variance
AAPL            0.001234    0.021567    0.000465
MSFT            0.001456    0.019876    0.000395
GOOGL           0.001123    0.022345    0.000499
```

**Mean Return**: Average daily return
- Multiply by 252 for annualized return
- Example: 0.001234 × 252 = 31.1% annual return

**Std Dev**: Daily volatility
- Multiply by √252 ≈ 15.87 for annualized volatility
- Example: 0.021567 × 15.87 = 34.2% annual volatility

**Variance**: Squared standard deviation
- Less intuitive but used in optimization calculations

### Understanding the Efficient Frontier Plot

**Axes:**
- **X-axis (Risk)**: Portfolio standard deviation (annualized)
- **Y-axis (Return)**: Portfolio expected return (annualized)

**Color Scale:**
- Represents Sharpe Ratio: (Return - Risk-Free Rate) / Risk
- Warmer colors (yellow/green): Higher Sharpe Ratio = Better risk-adjusted returns
- Cooler colors (blue/purple): Lower Sharpe Ratio

**Key Features:**
- **Left edge**: Minimum variance portfolio (lowest risk)
- **Top edge**: Maximum return portfolios
- **Upper-left region**: Best Sharpe ratios (optimal portfolios)

### Understanding Optimal Weights Output

```
Optimal Portfolio Weights for Max Return at Target Risk:
AAPL: 25.3%
MSFT: 30.1%
GOOGL: 22.4%
AMZN: 22.2%

Portfolio Risk: 0.2500
Portfolio Return: 0.2845
```

**Weights**: Percentage to invest in each asset
- Should sum to 100%
- No negative values (no short selling)

**Portfolio Risk**: Your target standard deviation (annualized)

**Portfolio Return**: Expected annual return at that risk level

### Sharpe Ratio Interpretation

The Sharpe Ratio measures risk-adjusted performance:

- **< 1.0**: Poor risk-adjusted returns
- **1.0 - 2.0**: Good risk-adjusted returns
- **2.0 - 3.0**: Very good risk-adjusted returns
- **> 3.0**: Excellent risk-adjusted returns (rare in practice)

## Advanced Usage

### Custom Date Ranges for Different Market Conditions

**Bull Market Analysis** (2019-2020):
```python
date1 = '2019-01-01'
date2 = '2020-02-01'  # Before COVID crash
```

**Crisis Period** (COVID-19):
```python
date1 = '2020-02-01'
date2 = '2020-06-01'
```

**Recovery Period**:
```python
date1 = '2020-06-01'
date2 = '2021-06-01'
```

Compare how optimal allocations change across market regimes.

### Comparing Multiple Target Risks

Run the optimization cell multiple times with different target risks:
- Conservative: 0.10 (10% volatility)
- Moderate: 0.15 (15% volatility)
- Aggressive: 0.25 (25% volatility)
- Very Aggressive: 0.35 (35% volatility)

Track how portfolio composition changes with risk tolerance.

### Sector Rotation Analysis

Compare portfolios during different sectors' strong periods:
- **Tech dominance** (2019-2021): FAANG stocks
- **Energy surge** (2021-2022): XOM, CVX
- **Defensive rotation** (2022): PG, KO, JNJ

### Analyzing Individual Asset Contributions

After getting optimal weights, calculate:

```python
# Asset contribution to portfolio return
asset_contributions = optimal_weights * daily_returns_combined.mean() * 252

# Asset contribution to portfolio risk
asset_risk_contributions = optimal_weights * (daily_returns_combined.std() * np.sqrt(252))

print("Return Contributions:", asset_contributions)
print("Risk Contributions:", asset_risk_contributions)
```

## Troubleshooting

### Issue 1: "Stock with ticker symbol X does not exist"

**Cause**: Invalid ticker or delisted stock

**Solution**:
- Verify ticker on Yahoo Finance
- Check for ticker changes (e.g., FB → META)
- Ensure proper capitalization

### Issue 2: Empty DataFrame after fetch_stock_data

**Cause**: No data available for date range

**Solution**:
- Check if stock traded during that period
- Adjust date range (stock may have IPO'd later)
- Verify market was open (avoid weekends/holidays)

### Issue 3: Optimization returns None

**Cause**: Infeasible constraints (target risk too low/high)

**Solution**:
- Check efficient frontier plot for feasible range
- Minimum risk: Look at leftmost point on frontier
- Maximum risk: Look at rightmost point on frontier
- Enter a risk value within this range

### Issue 4: Singular covariance matrix

**Cause**: Highly correlated assets or insufficient data

**Solution**:
- Use longer date range (>6 months recommended)
- Remove duplicate/highly correlated assets
- Add assets from different sectors

### Issue 5: Poor diversification (one asset gets 90%+ weight)

**Cause**: One asset dominates the risk-return profile

**Solution**:
- This is mathematically optimal but not practical
- Add minimum/maximum weight constraints (advanced)
- Consider assets with different characteristics

### Issue 6: Risk-free rate fetch fails

**Cause**: Yahoo Finance API issues or date range problems

**Solution**:
```python
# Manual override
risk_free_rate = 0.02  # 2% annual risk-free rate
```

Use current Treasury rates from treasury.gov

## Best Practices

1. **Data Quality**:
   - Use at least 1 year of historical data
   - Be aware of outlier periods (market crashes)
   - Check for missing data

2. **Asset Selection**:
   - Include 5-15 assets for good diversification
   - Mix sectors and asset classes
   - Avoid highly correlated assets

3. **Risk Targeting**:
   - Start with efficient frontier visualization
   - Choose risk within the feasible range
   - Compare multiple risk levels

4. **Rebalancing**:
   - Re-run optimization quarterly or annually
   - Markets change; optimal allocations evolve
   - Update date ranges to use recent data

5. **Reality Checks**:
   - Historical returns ≠ future returns
   - Add buffer for transaction costs (1-2%)
   - Consider tax implications
   - Account for minimum investment amounts

## Next Steps

After mastering the basic usage:
1. Read [ALGORITHMS.md](ALGORITHMS.md) for technical details
2. Check [EXAMPLES.md](EXAMPLES.md) for more use cases
3. Experiment with different asset combinations
4. Try implementing portfolio constraints
5. Explore backtesting your optimized portfolios

---

For more information, see the [main README](../README.md) or the research paper included in the repository.
