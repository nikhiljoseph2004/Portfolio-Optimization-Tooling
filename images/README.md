# Images and Visual Assets

This directory contains visual assets for the Portfolio Optimization Tooling project.

## Contents

### Screenshots (To be added)
- `efficient_frontier_example.png` - Example of efficient frontier visualization
- `portfolio_weights_output.png` - Example of optimized portfolio weights
- `statistics_table.png` - Example of daily statistics output

### Diagrams
- `architecture_diagram.png` - System architecture overview
- `workflow_diagram.png` - Optimization workflow

## How to Generate Screenshots

To generate screenshots of your optimization results:

1. Run the Jupyter notebook (`testing.ipynb`)
2. When the efficient frontier plot appears, save it using:
   - Jupyter: Right-click on the plot → Save image as...
   - Or in code: `plt.savefig('images/my_frontier.png', dpi=300, bbox_inches='tight')`

3. For output screenshots, use:
   - macOS: Cmd+Shift+4 (select area)
   - Windows: Snipping Tool or Win+Shift+S
   - Linux: Screenshot tool or `gnome-screenshot -a`

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Portfolio Optimization Tool               │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
         ┌────────────────────────────────────────┐
         │      Jupyter Notebook Interface        │
         │         (testing.ipynb)                │
         └────────────────────────────────────────┘
                              │
                              ▼
         ┌────────────────────────────────────────┐
         │        Data Retrieval Module           │
         │    (Yahoo Finance via yfinance)        │
         └────────────────────────────────────────┘
                              │
                              ▼
         ┌────────────────────────────────────────┐
         │     Statistical Analysis Module        │
         │  (Returns, Mean, Std Dev, Variance)    │
         └────────────────────────────────────────┘
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
         ┌──────────────────┐   ┌─────────────────┐
         │  Monte Carlo     │   │  Constrained    │
         │  Simulation      │   │  Optimization   │
         │  (Efficient      │   │  (SLSQP)        │
         │   Frontier)      │   │                 │
         └──────────────────┘   └─────────────────┘
                    │                   │
                    └─────────┬─────────┘
                              ▼
         ┌────────────────────────────────────────┐
         │       Visualization Module             │
         │   (Matplotlib - Efficient Frontier)    │
         └────────────────────────────────────────┘
                              │
                              ▼
         ┌────────────────────────────────────────┐
         │         Optimal Portfolio              │
         │     (Weights, Risk, Return)            │
         └────────────────────────────────────────┘
```

## Workflow Diagram

```
START
  │
  ├─► Load Libraries & Functions
  │
  ├─► Set Date Range
  │
  ├─► Fetch Risk-Free Rate (T-Bills)
  │
  ├─► User Input: Stock Tickers
  │        │
  │        └─► Validate Tickers
  │                  │
  ├─► Fetch Historical Data (Yahoo Finance)
  │        │
  │        └─► Handle Missing/Invalid Data
  │
  ├─► Calculate Daily Returns
  │
  ├─► Calculate Statistics
  │        ├─► Mean Returns
  │        ├─► Standard Deviations
  │        └─► Covariance Matrix
  │
  ├─► Generate Efficient Frontier
  │        │
  │        └─► Monte Carlo: 10,000 Random Portfolios
  │                  ├─► Calculate Returns
  │                  ├─► Calculate Risks
  │                  └─► Calculate Sharpe Ratios
  │
  ├─► Display Efficient Frontier Plot
  │
  ├─► User Input: Target Risk Level
  │
  ├─► Constrained Optimization
  │        │
  │        └─► SLSQP Algorithm
  │                  ├─► Maximize Return
  │                  ├─► Subject to: Risk ≤ Target
  │                  └─► Subject to: Σ weights = 1
  │
  ├─► Output Optimal Weights
  │
  ├─► Calculate Portfolio Metrics
  │        ├─► Expected Return
  │        ├─► Expected Risk
  │        └─► Sharpe Ratio
  │
END (Display Results)
```

## Contributing Images

If you'd like to contribute screenshots or diagrams:

1. Ensure high quality (300 DPI for screenshots)
2. Use PNG format for charts/diagrams
3. Name files descriptively
4. Include a caption in this README

## License

All images and diagrams in this directory are covered by the project's MIT License unless otherwise noted.
