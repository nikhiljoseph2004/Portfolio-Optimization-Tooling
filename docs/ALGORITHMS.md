# Portfolio Optimization Algorithms

Technical details of the algorithms used in this project.

## Table of Contents
1. [Modern Portfolio Theory Overview](#modern-portfolio-theory-overview)
2. [Mathematical Foundations](#mathematical-foundations)
3. [Implementation Details](#implementation-details)
4. [Optimization Methods](#optimization-methods)
5. [Computational Complexity](#computational-complexity)
6. [Limitations and Assumptions](#limitations-and-assumptions)

## Modern Portfolio Theory

Modern Portfolio Theory (MPT) by Harry Markowitz (1952) provides a framework for portfolio construction.

### Key Concepts

1. **Risk-Return Tradeoff**: Higher risk requires higher expected returns
2. **Diversification**: Combining assets reduces portfolio risk
3. **Efficient Frontier**: Set of portfolios with maximum return for each risk level
4. **Rational Investors**: Prefer higher returns or lower risk

## Mathematical Foundations

### Portfolio Return

Expected return is the weighted average:

```
R_p = Σ(w_i × r_i)
```

Where w_i = weight of asset i, r_i = expected return of asset i

Annualization: Multiply daily returns by 252 trading days

### Portfolio Risk

Portfolio variance accounts for individual variances and covariances:

```
σ²_p = w^T Σ w
```

Where Σ = covariance matrix

Portfolio standard deviation: σ_p = √(σ²_p)

Annualization: Multiply by √252 ≈ 15.87

### Sharpe Ratio

Risk-adjusted performance:

```
Sharpe = (R_p - R_f) / σ_p
```

Higher is better.

## Implementation

### 1. Data Retrieval

Fetches daily closing prices from Yahoo Finance for each ticker.

Time Complexity: O(N × D) where N = tickers, D = days

### 2. Return Calculation

Calculates percentage change between consecutive days: (P_t - P_{t-1}) / P_{t-1}

### 3. Efficient Frontier

Monte Carlo simulation:
1. Generate 10,000 random portfolio weights
2. Normalize to sum to 1
3. Calculate return, risk, and Sharpe ratio for each
4. Store for visualization

Time Complexity: O(M × N²) where M = 10,000 simulations

### 4. Optimization (SLSQP)

Maximizes return for target risk:

Objective: Maximize R_p = w^T μ × 252

Constraints:
- Σ w_i = 1 (fully invested)
- σ_p ≤ target_risk
- 0 ≤ w_i ≤ 1 (no short selling)

Time Complexity: O(N³ × K) where K = iterations (~10-100)

## Computational Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Data Fetch | O(N × D) | O(N × D) |
| Covariance | O(N² × D) | O(N²) |
| Monte Carlo | O(M × N²) | O(M × N) |
| Optimization | O(K × N³) | O(N²) |

Where N = assets, D = days, M = 10,000, K = ~10-100

Typical performance (8 assets, 1 year): ~5-10 seconds total

## Limitations

### 1. Normality Assumption
Assumes returns are normally distributed, but real returns have fat tails and skewness.

### 2. Historical Data
Past returns don't predict future. Market regimes change.

### 3. No Transaction Costs
Assumes free rebalancing, but trading has costs and taxes.

### 4. Correlation Changes
Correlations increase during crises when diversification is needed most.

### 5. Liquidity
Assumes assets can be traded instantly at market prices.

### 6. No Short Selling
Current implementation restricts weights ≥ 0.

See the research paper for details: `Financial Engineering_IEDA3330 Final Report_compressed.pdf`
