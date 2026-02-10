# Portfolio Optimization Tooling

A Python-based portfolio optimization toolkit that implements Modern Portfolio Theory (MPT) and efficient frontier analysis.

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Technical Details](#technical-details)
- [Project Structure](#project-structure)
- [Background & Theory](#background--theory)
- [Authors](#authors)

## Overview

This project provides tools for portfolio optimization, allowing users to:
- Analyze stocks using historical market data
- Calculate risk-return profiles
- Generate efficient frontier visualizations
- Optimize portfolio allocations based on risk tolerance
- Incorporate risk-free rates

This project was developed as part of the Financial Engineering course (IEDA3330).

## Key Features

### Data Retrieval & Analysis
- Fetch historical stock data from Yahoo Finance
- Calculate daily returns, mean returns, standard deviations, and variances
- Support for stocks, ETFs, and indices

### Portfolio Optimization
- Efficient frontier generation using Monte Carlo simulation
- Maximum return optimization for a given risk level
- Minimum risk optimization for a given return level
- Sharpe ratio analysis

### Risk Management
- Risk-free rate integration (Treasury Bill rates)
- Annualized risk and return calculations
- Covariance matrix analysis

### Visualization
- Efficient frontier plots with Sharpe ratios
- Risk-return tradeoff visualization

## Requirements

Python 3.8 or higher and the following packages:
```
yfinance>=0.2.28
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
scipy>=1.10.0
jupyter>=1.0.0
```

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/nikhiljoseph2004/Portfolio-Optimization-Tooling.git
   cd Portfolio-Optimization-Tooling
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Launch Jupyter Notebook:
   ```bash
   jupyter notebook main.ipynb
   ```

## Usage

### Quick Start

The notebook `main.ipynb` provides an interactive interface for portfolio optimization.

#### Steps

1. Launch the notebook:
   ```bash
   jupyter notebook main.ipynb
   ```

2. Run the setup cells:
   - Cell 1: Import libraries
   - Cell 2: Load function definitions
   - Cell 3: Set date range and fetch risk-free rate

3. Enter stock tickers when prompted:
   ```
   Enter stock tickers, separated by commas: AAPL, MSFT, GOOGL, AMZN
   ```

4. View the efficient frontier:
   - Fetches historical data from Yahoo Finance
   - Calculates returns and statistics
   - Generates 10,000 random portfolios
   - Displays a plot showing risk vs. return

5. Optimize your portfolio by entering a target risk level:
   ```
   Enter your desired portfolio risk (standard deviation): 0.25
   ```
   
   Output example:
   ```
   Optimal Portfolio Weights:
   AAPL: 25.3%
   MSFT: 30.1%
   GOOGL: 22.4%
   AMZN: 22.2%
   
   Portfolio Return: 28.5% (annualized)
   Portfolio Risk: 25.0% (annualized)
   ```

### Sample Stock Combinations

Example combinations for different strategies:

#### Conservative (Low Risk)
```
JNJ, PG, KO, WMT, DUK, VZ
```

#### Balanced (Moderate Risk)
```
DUK, XOM, CVX, PG, KO, BABA, SPY, VTI
```

#### Growth (High Risk)
```
AAPL, MSFT, NVDA, GOOGL, AMZN, TSLA, META
```

#### Diversified
```
AAPL, MSFT, NVDA, JNJ, PFE, AMZN, TSLA, JPM, BAC, NEE, DUK, XOM, CVX, PG, KO, BABA, SPY, VTI, EEM
```

### Output Descriptions

#### Daily Statistics Table
- Mean Return: average daily return
- Standard Deviation: volatility
- Variance: squared volatility

#### Efficient Frontier Plot
- X-axis: Portfolio risk (standard deviation)
- Y-axis: Portfolio return
- Color: Sharpe ratio (higher is better)

#### Optimized Portfolio Weights
Percentage allocation to each stock for the target risk/return.

## Technical Details

### Optimization Algorithms

#### Efficient Frontier Calculation
Monte Carlo simulation:
- Generates 10,000 random weight combinations
- Weights sum to 1
- Calculates annualized returns and risks
- Computes Sharpe ratios

#### Constrained Optimization
Uses `scipy.optimize.minimize`:
- Weights sum to 1
- No short selling (weights ≥ 0)
- Target risk or return constraints

#### Calculations
- Portfolio Return: `R_p = Σ(w_i × r_i) × 252`
- Portfolio Risk: `σ_p = √(w^T × Σ × w) × √252`
- Sharpe Ratio: `(R_p - R_f) / σ_p`

Where:
- `w_i` = weight of asset i
- `r_i` = return of asset i
- `Σ` = covariance matrix
- `R_f` = risk-free rate

## Project Structure

```
Portfolio-Optimization-Tooling/
│
├── main.ipynb                             # Main notebook
├── Financial Engineering_IEDA3330         # Course report
│   Final Report_compressed.pdf
├── README.md                              # This file
├── requirements.txt                       # Dependencies
├── docs/                                  # Documentation
│   ├── USAGE.md
│   ├── ALGORITHMS.md
│   └── EXAMPLES.md
└── examples/
    └── README.md
```

## Background & Theory

This project implements Modern Portfolio Theory (MPT) developed by Harry Markowitz:

- **Diversification**: Combining assets reduces portfolio risk
- **Efficient Frontier**: Set of optimal portfolios with highest return for a given risk
- **Sharpe Ratio**: Risk-adjusted return measure
- **Mean-Variance Optimization**: Balancing returns against risk

See `Financial Engineering_IEDA3330 Final Report_compressed.pdf` for details.

## Authors

Nikhil Joseph - [nikhiljoseph2004](https://github.com/nikhiljoseph2004)
