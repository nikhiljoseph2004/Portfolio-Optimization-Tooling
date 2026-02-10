# Usage Guide

Instructions for using the Portfolio Optimization Tooling.

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

### Setup
```bash
git clone https://github.com/nikhiljoseph2004/Portfolio-Optimization-Tooling.git
cd Portfolio-Optimization-Tooling
pip install -r requirements.txt
jupyter notebook
```

Open `main.ipynb` in your browser.

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
Execute the second cell to load the portfolio optimization functions.

### Step 3: Set Date Range and Risk-Free Rate (Cell 3)
```python
date1 = '2021-01-06'
date2 = '2022-01-09'
risk_free_rate = fetch_tbill_rate(date1, date2)
```

Date selection tips:
- Use at least 6 months of data
- 1-3 years provides better estimates
- Format: 'YYYY-MM-DD'

### Step 4: Input Stock Tickers (Cell 5)
Enter stock tickers separated by commas:
```
Enter stock tickers, separated by commas: AAPL, MSFT, GOOGL, AMZN
```

The program will fetch data, calculate statistics, and display the efficient frontier plot.

### Step 5: Optimize Portfolio (Cell 6)
Enter your target risk:
```
Enter your desired portfolio risk (standard deviation): 0.25
```

Output includes optimal weights, expected return, and risk.

## Detailed Examples

### Example 1: Tech Portfolio

**Input:**
```python
date1 = '2020-01-01'
date2 = '2023-01-01'

# Tickers
AAPL, MSFT, GOOGL, NVDA, TSLA, META
```

**Expected Results:**
- High returns (20-40% annual)
- High risk (25-40% std dev)
- Strong correlation

---

### Example 2: Diversified Portfolio

**Input:**
```python
date1 = '2021-01-01'
date2 = '2024-01-01'

# Tickers
DUK, XOM, CVX, PG, KO, BABA, SPY, VTI
```

Includes utilities, energy, consumer staples, international, and broad market exposure.

**Expected Results:**
- Moderate returns (10-18% annual)
- Lower risk (15-25% std dev)
- Better diversification

---

### Example 3: Income Portfolio

**Input:**
```python
date1 = '2019-01-01'
date2 = '2024-01-01'

# Tickers
JNJ, PG, KO, T, VZ, PFE
```

**Expected Results:**
- Lower returns (8-15% annual)
- Lower risk (12-20% std dev)

---

### Example 4: Index Fund Portfolio

**Input:**
```python
date1 = '2018-01-01'
date2 = '2023-01-01'

# Tickers
SPY, QQQ, IWM, VEA, VWO, AGG
```

Components: large-cap, tech, small-cap, international, emerging markets, bonds

**Target Risk**: 0.15 (15%)

---

## Interpreting Results

### Statistics Output

```
Daily Statistics:
               Mean Return    Std Dev    Variance
AAPL            0.001234    0.021567    0.000465
```

- **Mean Return**: Average daily return (multiply by 252 for annual)
- **Std Dev**: Daily volatility (multiply by √252 for annual)
- **Variance**: Squared standard deviation

### Efficient Frontier Plot

- **X-axis**: Portfolio risk (annualized)
- **Y-axis**: Portfolio return (annualized)
- **Color**: Sharpe ratio (higher is better)

### Optimal Weights Output

```
Optimal Portfolio Weights:
AAPL: 25.3%
MSFT: 30.1%

Portfolio Risk: 0.2500
Portfolio Return: 0.2845
```

Weights sum to 100%, no negative values (no short selling).

### Sharpe Ratio

Risk-adjusted performance measure:
- < 1.0: Poor
- 1.0 - 2.0: Good
- 2.0 - 3.0: Very good
- > 3.0: Excellent (rare)

## Advanced Usage

### Custom Date Ranges

Compare different market conditions:

**Bull Market** (2019-2020):
```python
date1 = '2019-01-01'
date2 = '2020-02-01'
```

**Crisis Period** (COVID-19):
```python
date1 = '2020-02-01'
date2 = '2020-06-01'
```

### Comparing Risk Levels

Run optimization with different target risks:
- Conservative: 0.10
- Moderate: 0.15
- Aggressive: 0.25

## Troubleshooting

### Invalid ticker
- Verify ticker on Yahoo Finance
- Check for ticker changes (e.g., FB → META)

### Empty data
- Check if stock traded during selected dates
- Adjust date range

### Optimization fails
- Target risk may be outside feasible range
- Check efficient frontier plot for valid range

### Singular covariance matrix
- Use longer date range (>6 months)
- Remove highly correlated assets

### Poor diversification
- One asset dominates? Mathematically optimal but impractical
- Consider adding minimum/maximum weight constraints

### Risk-free rate fetch fails
```python
risk_free_rate = 0.02  # Manual override with current rate
```

## Best Practices

1. Use at least 1 year of historical data
2. Include 5-15 assets for diversification
3. Mix sectors and asset classes
4. Choose risk within feasible range shown on plot
5. Re-run optimization quarterly or annually
6. Remember: past returns ≠ future returns

## Next Steps

See also:
- [ALGORITHMS.md](ALGORITHMS.md) for technical details
- [EXAMPLES.md](EXAMPLES.md) for more use cases
