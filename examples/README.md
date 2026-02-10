# Quick Start Example - Portfolio Optimization

This example demonstrates basic usage of the Portfolio Optimization Tooling.

## Prerequisites

Make sure you have installed all requirements:
```bash
pip install -r requirements.txt
```

## Running the Example

### Option 1: Using Jupyter Notebook (Recommended)

1. Start Jupyter:
   ```bash
   jupyter notebook
   ```

2. Open `../testing.ipynb`

3. Run all cells in order

4. When prompted for tickers, try this diversified portfolio:
   ```
   DUK, XOM, CVX, PG, KO, BABA, SPY, VTI
   ```

5. When prompted for risk tolerance, try:
   ```
   0.20
   ```
   (20% annualized volatility - moderate risk)

### Option 2: Python Script (Coming Soon)

A standalone Python script version will be added for command-line usage.

## Example Output

After running the optimization, you will see:

### 1. Daily Statistics
```
               Mean Return    Std Dev    Variance
DUK            0.000523    0.013245    0.000175
XOM            0.000892    0.021456    0.000460
CVX            0.000834    0.020123    0.000405
PG             0.000445    0.011234    0.000126
KO             0.000398    0.010987    0.000121
BABA           0.001123    0.027654    0.000765
SPY            0.000712    0.015678    0.000246
VTI            0.000698    0.015432    0.000238
```

### 2. Efficient Frontier Plot

A scatter plot showing:
- **X-axis**: Portfolio risk (standard deviation)
- **Y-axis**: Portfolio return
- **Color**: Sharpe ratio (warmer = better)

The efficient frontier is the upper edge of the cloud of points.

### 3. Optimal Portfolio Weights

```
Optimal Portfolio Weights for Max Return at Target Risk:
XOM: 22.5%
BABA: 20.3%
SPY: 18.7%
CVX: 15.2%
VTI: 12.8%
KO: 6.3%
PG: 4.2%
DUK: 0.0%

Portfolio Risk: 0.2000 (20%)
Portfolio Return: 0.1845 (18.45% annual)
Sharpe Ratio: 0.82
```

## Interpretation

### What Do the Results Mean?

1. **Portfolio Weights**: How much to invest in each stock
   - Higher weights = more important to the portfolio
   - Zero weights = asset doesn't improve the portfolio at your target risk
   - All weights sum to 100%

2. **Portfolio Risk**: Expected volatility (standard deviation)
   - Lower = more stable, less volatile
   - Higher = more volatile, bigger swings
   - Target: 20% is moderate (between conservative 10% and aggressive 30%)

3. **Portfolio Return**: Expected annual return
   - Historical average, not guaranteed
   - 18.45% is strong (above market average of ~10%)
   - Comes with 20% risk

4. **Sharpe Ratio**: Risk-adjusted return
   - Higher is better
   - 0.82 is good (above 0.5)
   - Measures return per unit of risk

### Why Some Assets Get 0% Weight?

The optimizer found that certain assets don't improve the portfolio:
- **DUK (0%)**: Low return doesn't justify including it
- Assets with 0% are either:
  - Too correlated with included assets
  - Lower Sharpe ratio than alternatives
  - Don't fit the risk-return target

### What Should I Do With These Results?

**For actual investing:**
1. This is historical data - past performance ≠ future results
2. Consider transaction costs and taxes
3. Round weights to practical amounts ($1,000 minimums, etc.)
4. Rebalance quarterly or annually
5. Consult a financial advisor for personalized advice

**For learning:**
1. Try different time periods to see how results change
2. Experiment with different asset combinations
3. Compare multiple target risk levels
4. Understand the risk-return tradeoff

## Trying Different Scenarios

### Conservative Portfolio (Low Risk)
```
Tickers: JNJ, PG, KO, WMT, VZ, DUK
Target Risk: 0.12 (12%)
Expected: Low-volatility, dividend-focused portfolio
```

### Aggressive Growth (High Risk)
```
Tickers: NVDA, TSLA, AAPL, MSFT, AMZN, GOOGL
Target Risk: 0.30 (30%)
Expected: High returns with high volatility
```

### Balanced Portfolio
```
Tickers: SPY, AGG, VTI, BND, GLD, VNQ
Target Risk: 0.15 (15%)
Expected: Mix of stocks, bonds, and alternatives
```

## Common Issues

### Issue: "Stock with ticker symbol X does not exist"
**Solution**: Check the ticker on Yahoo Finance, ensure correct spelling and capitalization

### Issue: Optimization returns None
**Solution**: Target risk is outside feasible range. Check the efficient frontier plot to see the range of possible risks, then choose a value within that range.

### Issue: One stock gets 90%+ allocation
**Solution**: This means that stock dominates the others. While mathematically optimal, consider adding constraints or diversifying more broadly.

### Issue: Can't connect to Yahoo Finance
**Solution**: Check internet connection, or Yahoo Finance may be temporarily down. Try again later.

## Next Steps

1. Read the full [Usage Guide](../docs/USAGE.md) for detailed instructions
2. Explore [Examples](../docs/EXAMPLES.md) for more scenarios
3. Learn about the [Algorithms](../docs/ALGORITHMS.md) behind the tool
4. Experiment with your own stock selections!

## Important Disclaimer

This tool is for **educational purposes only** and does not constitute financial advice. 

- Past performance does not guarantee future results
- All investments carry risk, including potential loss of principal
- Consult with a qualified financial advisor before making investment decisions
- The authors are not responsible for any investment losses

## Questions?

- Check the [main README](../README.md)
- Review the [documentation](../docs/)
- Open an issue on GitHub

---

Happy investing! 📈
