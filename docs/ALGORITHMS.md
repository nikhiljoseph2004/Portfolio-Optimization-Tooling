# Portfolio Optimization Algorithms

This document provides a technical deep-dive into the algorithms and mathematical foundations used in the Portfolio Optimization Tooling.

## Table of Contents
1. [Modern Portfolio Theory Overview](#modern-portfolio-theory-overview)
2. [Mathematical Foundations](#mathematical-foundations)
3. [Implementation Details](#implementation-details)
4. [Optimization Methods](#optimization-methods)
5. [Computational Complexity](#computational-complexity)
6. [Limitations and Assumptions](#limitations-and-assumptions)

## Modern Portfolio Theory Overview

Modern Portfolio Theory (MPT), introduced by Harry Markowitz in 1952, revolutionized investment management by providing a mathematical framework for portfolio construction.

### Core Principles

1. **Risk-Return Tradeoff**: Investors must be compensated with higher expected returns for taking on additional risk.

2. **Diversification Benefits**: Combining assets can reduce portfolio risk below the weighted average of individual asset risks, due to less-than-perfect correlation.

3. **Efficient Frontier**: The set of portfolios that offer the maximum expected return for each level of risk, or equivalently, the minimum risk for each level of expected return.

4. **Rational Investors**: Investors prefer higher returns for the same risk, or lower risk for the same return.

## Mathematical Foundations

### Portfolio Return

The expected return of a portfolio is the weighted average of individual asset returns:

```
R_p = Σ(w_i × r_i)    for i = 1 to N
```

Where:
- `R_p` = Portfolio expected return
- `w_i` = Weight of asset i in the portfolio
- `r_i` = Expected return of asset i
- `N` = Number of assets

**Annualization**: Daily returns are annualized by multiplying by 252 (trading days per year):
```
R_annual = R_daily × 252
```

### Portfolio Risk (Variance)

Portfolio variance accounts for both individual asset variances and covariances between assets:

```
σ²_p = Σ Σ w_i w_j Cov(r_i, r_j)    for i,j = 1 to N
```

In matrix notation:
```
σ²_p = w^T Σ w
```

Where:
- `σ²_p` = Portfolio variance
- `w` = Vector of portfolio weights
- `Σ` = Covariance matrix of asset returns
- `^T` = Transpose operation

Portfolio standard deviation (risk):
```
σ_p = √(σ²_p)
```

**Annualization**: Daily variance is annualized by multiplying by 252:
```
σ²_annual = σ²_daily × 252
σ_annual = σ_daily × √252 ≈ σ_daily × 15.87
```

### Covariance Matrix

The covariance between two assets measures how they move together:

```
Cov(r_i, r_j) = E[(r_i - μ_i)(r_j - μ_j)]
```

For a portfolio with N assets, the covariance matrix Σ is an N×N matrix where:
- Diagonal elements: `Σ_ii = Var(r_i)` (variance of asset i)
- Off-diagonal elements: `Σ_ij = Cov(r_i, r_j)` (covariance between assets i and j)

Properties:
- Symmetric: `Σ_ij = Σ_ji`
- Positive semi-definite

### Correlation Coefficient

Correlation normalizes covariance to [-1, 1]:

```
ρ_ij = Cov(r_i, r_j) / (σ_i × σ_j)
```

Where:
- `ρ_ij = 1`: Perfect positive correlation
- `ρ_ij = 0`: No correlation (diversification benefit)
- `ρ_ij = -1`: Perfect negative correlation (maximum diversification)

### Sharpe Ratio

Measures risk-adjusted performance:

```
Sharpe = (R_p - R_f) / σ_p
```

Where:
- `R_p` = Portfolio expected return
- `R_f` = Risk-free rate
- `σ_p` = Portfolio standard deviation

**Interpretation**: Return earned per unit of risk taken, above the risk-free rate.

Higher Sharpe ratios indicate better risk-adjusted returns.

## Implementation Details

### 1. Data Retrieval (`fetch_stock_data`)

```python
def fetch_stock_data(tickers, start_date, end_date):
    closing_prices = {}
    for ticker in tickers:
        stock_data = yf.Ticker(ticker).history(
            start=start_date, 
            end=end_date, 
            interval='1d'
        )
        closing_prices[ticker] = stock_data['Close']
    return pd.DataFrame(closing_prices)
```

**Algorithm**:
1. Iterate through each ticker symbol
2. Fetch daily OHLCV data from Yahoo Finance
3. Extract closing prices
4. Combine into a single DataFrame with dates as index

**Time Complexity**: O(N × D) where N = number of tickers, D = number of days

**Error Handling**:
- Checks for empty data (invalid/delisted tickers)
- Continues with valid tickers, reports invalid ones

### 2. Return Calculation (`calculate_returns_and_stats`)

```python
def calculate_returns_and_stats(df):
    daily_returns = df.pct_change().dropna()
    return calculate_statistics(daily_returns)
```

**Algorithm**:
1. Calculate percentage change between consecutive days: `(P_t - P_{t-1}) / P_{t-1}`
2. Drop first row (NaN from pct_change)
3. Calculate mean, std dev, and variance for each asset

**Simple Return Formula**:
```
r_t = (P_t - P_{t-1}) / P_{t-1}
```

**Note**: Uses simple returns rather than log returns for consistency with standard MPT implementation.

### 3. Efficient Frontier Generation (`calculate_efficient_frontier`)

```python
def calculate_efficient_frontier(returns, risk_free_rate=0.0, num_portfolios=10000):
    num_assets = returns.shape[1]
    results = np.zeros((3, num_portfolios))
    weights_record = []
    
    for i in range(num_portfolios):
        # Generate random weights
        weights = np.random.random(num_assets)
        weights /= np.sum(weights)  # Normalize to sum to 1
        
        # Calculate portfolio metrics
        portfolio_return = np.sum(returns.mean() * weights) * 252
        portfolio_std_dev = np.sqrt(
            np.dot(weights.T, np.dot(returns.cov() * 252, weights))
        )
        sharpe_ratio = (portfolio_return - risk_free_rate) / portfolio_std_dev
        
        results[0, i] = portfolio_return
        results[1, i] = portfolio_std_dev
        results[2, i] = sharpe_ratio
        
    return results, weights_record
```

**Algorithm** (Monte Carlo Simulation):
1. Generate 10,000 random portfolio weight combinations
2. Normalize weights to sum to 1 (fully invested constraint)
3. For each portfolio:
   - Calculate expected return: `w^T μ × 252`
   - Calculate risk: `√(w^T Σ w × 252)`
   - Calculate Sharpe ratio
4. Store all metrics for visualization

**Time Complexity**: O(M × N²) where M = number of simulations (10,000), N = number of assets

**Space Complexity**: O(M × N) for storing weights

**Advantages**:
- Simple to implement
- Provides good coverage of feasible region
- Computationally efficient for moderate N

**Limitations**:
- Not exhaustive (doesn't guarantee finding absolute optimal)
- Points may not lie exactly on efficient frontier
- Random sampling may miss certain regions

### 4. Constrained Optimization (`optimize_portfolio_max_return`)

Maximizes return for a given risk target using Sequential Least Squares Programming (SLSQP).

```python
def optimize_portfolio_max_return(returns, target_std_dev):
    num_assets = returns.shape[1]
    
    def portfolio_performance(weights):
        portfolio_return = np.sum(returns.mean() * weights) * 252
        portfolio_std_dev = np.sqrt(
            np.dot(weights.T, np.dot(returns.cov() * 252, weights))
        )
        return portfolio_std_dev, portfolio_return
    
    def objective_function(weights):
        return -portfolio_performance(weights)[1]  # Negative for maximization
    
    constraints = [
        {'type': 'eq', 'fun': lambda w: np.sum(w) - 1},  # Weights sum to 1
        {'type': 'ineq', 'fun': lambda w: target_std_dev - portfolio_performance(w)[0]}  # Risk <= target
    ]
    
    bounds = tuple((0, 1) for _ in range(num_assets))  # No short selling
    initial_weights = num_assets * [1. / num_assets]  # Start with equal weights
    
    result = minimize(
        objective_function, 
        initial_weights, 
        bounds=bounds, 
        constraints=constraints,
        method='SLSQP'
    )
    
    return result.x if result.success else None
```

**Mathematical Formulation**:

Maximize: `R_p = w^T μ × 252`

Subject to:
- `Σ w_i = 1` (fully invested)
- `σ_p ≤ target_risk` (risk constraint)
- `0 ≤ w_i ≤ 1` (no short selling, no leverage)

**Algorithm** (SLSQP):
1. Start with equal weights (1/N for each asset)
2. Iteratively adjust weights to improve objective
3. Respect all constraints at each step
4. Stop when convergence criteria met

**Time Complexity**: O(N³ × K) where K = number of iterations (typically < 100)

**Convergence**: Typically reaches optimal solution in 10-50 iterations for well-conditioned problems.

### 5. Minimum Risk Optimization (`optimize_portfolio_min_risk`)

Minimizes risk for a given return target.

```python
def optimize_portfolio_min_risk(returns, target_return):
    # ... (similar structure to max return)
    
    def objective_function(weights):
        return portfolio_performance(weights)[0]  # Minimize risk
    
    constraints = [
        {'type': 'eq', 'fun': lambda w: np.sum(w) - 1},
        {'type': 'eq', 'fun': lambda w: portfolio_performance(w)[1] - target_return}
    ]
```

**Mathematical Formulation**:

Minimize: `σ_p = √(w^T Σ w × 252)`

Subject to:
- `Σ w_i = 1` (fully invested)
- `R_p = target_return` (return constraint - equality)
- `0 ≤ w_i ≤ 1` (no short selling)

**Key Difference**: Return constraint is equality (`=`) rather than inequality (`≤`), making this problem more constrained.

### 6. Risk-Free Rate Fetching (`fetch_tbill_rate`)

```python
def fetch_tbill_rate(start_date, end_date, ticker='^IRX'):
    tbill_data = yf.Ticker(ticker)
    tbill_history = tbill_data.history(start=start_date, end=end_date)
    tbill_rate = tbill_history['Close'].mean()
    return tbill_rate / 100  # Convert percentage to decimal
```

**Algorithm**:
1. Fetch 13-week Treasury Bill yields (^IRX) from Yahoo Finance
2. Calculate average yield over the period
3. Convert from percentage (e.g., 2.5) to decimal (0.025)

**Note**: Uses historical average as proxy for expected risk-free rate.

## Optimization Methods

### Method Comparison

| Method | Advantages | Disadvantages | Use Case |
|--------|-----------|---------------|----------|
| Monte Carlo | Simple, visualizes frontier | Approximate, not exact | Initial exploration |
| SLSQP (Sequential Least Squares) | Exact, handles constraints | Slower, may get stuck in local minima | Finding specific optimal portfolios |
| Quadratic Programming | Very fast, global optimum | Requires QP solver, less flexible | High-frequency rebalancing |
| Genetic Algorithms | Global search, handles discrete constraints | Slow, approximate | Complex constraint scenarios |

### When to Use Each Optimization

**Monte Carlo** (`calculate_efficient_frontier`):
- Visualizing the entire efficient frontier
- Understanding the feasible range of portfolios
- Initial exploration and education

**SLSQP Optimization** (`optimize_portfolio_*`):
- Finding precise optimal weights
- Given specific risk or return targets
- Real portfolio construction
- When exact solutions needed

## Computational Complexity

### Asymptotic Analysis

| Operation | Time Complexity | Space Complexity |
|-----------|-----------------|------------------|
| Data Fetch | O(N × D) | O(N × D) |
| Return Calculation | O(N × D) | O(N × D) |
| Covariance Matrix | O(N² × D) | O(N²) |
| Monte Carlo (10k portfolios) | O(M × N²) | O(M × N) |
| SLSQP Optimization | O(K × N³) | O(N²) |
| Plot Generation | O(M) | O(M) |

Where:
- N = number of assets
- D = number of days of historical data
- M = number of Monte Carlo simulations (10,000)
- K = number of optimization iterations (~10-100)

### Practical Performance

For typical use cases:
- **N = 8 assets, D = 252 days (1 year)**:
  - Data fetch: ~2-5 seconds (network dependent)
  - Efficient frontier: ~1-2 seconds
  - Optimization: <1 second
  - Total: ~5-10 seconds

- **N = 20 assets, D = 756 days (3 years)**:
  - Data fetch: ~5-10 seconds
  - Efficient frontier: ~3-5 seconds
  - Optimization: ~2-3 seconds
  - Total: ~15-20 seconds

### Scalability Limits

- **Practical limit**: ~50 assets
  - Beyond this, covariance matrix becomes very large (N²)
  - Optimization becomes slower (N³)
  - Multicollinearity issues increase

- **Memory considerations**:
  - 100 assets: ~40 MB for covariance matrix (float64)
  - Generally not a constraint for typical portfolios

## Limitations and Assumptions

### 1. Normality Assumption

**Assumption**: Returns are normally distributed

**Reality**: 
- Returns exhibit fat tails (more extreme events than normal distribution predicts)
- Skewness (asymmetric distribution)
- Kurtosis (more peaked or flatter than normal)

**Impact**:
- May underestimate risk of extreme losses
- Variance may not fully capture risk

**Mitigation**:
- Use longer time periods to capture tail events
- Consider stress testing
- Supplement with Value-at-Risk (VaR) analysis

### 2. Historical Returns

**Assumption**: Past returns predict future returns

**Reality**:
- Market regimes change
- Past performance ≠ future results
- Structural breaks (e.g., 2008 crisis, COVID-19)

**Impact**:
- Optimal portfolio may not perform as expected
- Correlation structures change

**Mitigation**:
- Regular reoptimization (quarterly/annually)
- Use multiple time periods
- Forward-looking views when available

### 3. No Transaction Costs

**Assumption**: Rebalancing is free

**Reality**:
- Brokerage commissions (though minimal nowadays)
- Bid-ask spreads
- Market impact (for large orders)
- Tax implications

**Impact**:
- Frequent rebalancing erodes returns
- Optimal portfolio may not be implementable

**Mitigation**:
- Add transaction cost penalty to optimization
- Set rebalancing thresholds (e.g., 5% drift)
- Minimize turnover

### 4. Diversification Benefits

**Assumption**: Correlation structure is stable

**Reality**:
- Correlations increase during crises (when diversification needed most)
- "When you need it, it's not there"

**Impact**:
- Diversification provides less protection than expected in downturns

**Mitigation**:
- Include truly uncorrelated assets (gold, bonds, alternatives)
- Stress test under crisis scenarios

### 5. Liquidity

**Assumption**: Assets can be bought/sold at will

**Reality**:
- Small/mid-cap stocks less liquid
- International markets may have restrictions
- ETFs can deviate from NAV

**Impact**:
- Optimal weights may not be achievable
- Slippage during execution

**Mitigation**:
- Stick to liquid securities
- Account for minimum lot sizes
- Implement gradually

### 6. Short Selling Constraint

**Current Implementation**: No short positions (weights ≥ 0)

**Impact**:
- Reduces available opportunity set
- Cannot profit from overvalued assets
- Fewer hedge opportunities

**Alternative**: Allow short selling (advanced)
```python
bounds = tuple((-1, 1) for _ in range(num_assets))  # Allow shorts
# Add constraint: sum(abs(weights)) <= 1  # Leverage limit
```

### 7. Single Period Model

**Assumption**: Static, one-period optimization

**Reality**:
- Multi-period dependencies
- Time-varying investment opportunities
- Path dependence

**Impact**:
- Ignores future rebalancing options
- Myopic decision making

**Alternative**: Dynamic programming approaches (advanced)

## Extensions and Advanced Topics

### 1. Black-Litterman Model
Incorporates investor views with market equilibrium

### 2. Risk Parity
Equalizes risk contribution from each asset

### 3. Minimum Conditional Value-at-Risk (CVaR)
Optimizes tail risk rather than variance

### 4. Robust Optimization
Accounts for parameter uncertainty

### 5. Factor Models
Decomposes returns into systematic factors

### 6. Multi-Period Optimization
Optimizes over multiple rebalancing periods

### 7. Transaction Cost Models
Incorporates realistic trading costs

---

For implementation examples, see [USAGE.md](USAGE.md).

For mathematical proofs and derivations, refer to the included research paper:
`Financial Engineering_IEDA3330 Final Report_compressed.pdf`
