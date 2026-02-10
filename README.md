# Portfolio Optimization Tooling

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md)

A comprehensive Python-based portfolio optimization toolkit that implements Modern Portfolio Theory (MPT) and efficient frontier analysis to help investors construct optimal investment portfolios.

![Portfolio Optimization](https://img.shields.io/badge/Portfolio-Optimization-success)
![Modern Portfolio Theory](https://img.shields.io/badge/MPT-Markowitz-blue)

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Requirements](#-requirements)
- [Installation](#-installation)
- [Usage](#-usage)
- [Technical Details](#-technical-details)
- [Project Structure](#-project-structure)
- [Background & Theory](#-background--theory)
- [Educational Use](#-educational-use)
- [Disclaimers](#-important-disclaimers)
- [Contributing](#-contributing)
- [License](#-license)
- [Authors](#-authors)
- [Acknowledgments](#-acknowledgments)

---

## 🎯 Overview

This project provides a complete suite of tools for portfolio optimization, allowing users to:
- Analyze multiple stocks using historical market data
- Calculate risk-return profiles for individual assets
- Generate efficient frontier visualizations
- Optimize portfolio allocations based on risk tolerance or return targets
- Incorporate risk-free rates for realistic portfolio analysis

The tooling is based on research conducted as part of the Financial Engineering course (IEDA3330) and implements industry-standard portfolio optimization techniques.

## ✨ Key Features

### 1. **Data Retrieval & Analysis**
- Fetch real-time and historical stock data from Yahoo Finance
- Calculate daily returns, mean returns, standard deviations, and variances
- Support for multiple asset classes (stocks, ETFs, indices)

### 2. **Portfolio Optimization Algorithms**
- **Efficient Frontier Generation**: Simulate thousands of random portfolios to visualize the risk-return tradeoff
- **Maximum Return Optimization**: Find optimal weights for maximum return given a target risk level
- **Minimum Risk Optimization**: Find optimal weights for minimum risk given a target return level
- **Sharpe Ratio Analysis**: Identify portfolios with the best risk-adjusted returns

### 3. **Risk Management**
- Integration of risk-free rate (Treasury Bill rates) for realistic benchmarking
- Annualized risk and return calculations (252 trading days)
- Portfolio variance and covariance matrix analysis

### 4. **Visualization**
- Interactive efficient frontier plots with color-coded Sharpe ratios
- Clear visualization of risk-return tradeoffs
- Portfolio weight distribution analysis

## 📋 Requirements

### Python Dependencies
```
yfinance>=0.2.28
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
scipy>=1.10.0
jupyter>=1.0.0
```

### Python Version
- Python 3.8 or higher

## 🚀 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/nikhiljoseph2004/Portfolio-Optimization-Tooling.git
   cd Portfolio-Optimization-Tooling
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch Jupyter Notebook**
   ```bash
   jupyter notebook testing.ipynb
   ```

## 📖 Usage

### Quick Start Example

The notebook `testing.ipynb` provides an interactive interface for portfolio optimization. Here's a complete workflow:

#### Step-by-Step Guide

1. **Launch the notebook**:
   ```bash
   jupyter notebook testing.ipynb
   ```

2. **Run the setup cells** (Cells 1-3):
   - Cell 1: Import libraries
   - Cell 2: Load function definitions
   - Cell 3: Set date range and fetch risk-free rate

3. **Enter your stock tickers** (Cell 5):
   When prompted, enter comma-separated ticker symbols:
   ```
   Enter stock tickers, separated by commas: AAPL, MSFT, GOOGL, AMZN
   ```

4. **View the efficient frontier**:
   The tool will automatically:
   - Fetch historical data from Yahoo Finance
   - Calculate daily returns and statistics
   - Generate 10,000 random portfolios
   - Display an interactive plot showing risk vs. return

5. **Optimize your portfolio** (Cell 6):
   Enter your target risk level:
   ```
   Enter your desired portfolio risk (standard deviation): 0.25
   ```
   
   The tool will output optimal weights:
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

Try these tested combinations for different investment strategies:

#### **Conservative Portfolio** (Low Risk - 12-15% volatility)
```
JNJ, PG, KO, WMT, DUK, VZ
```
Focus: Dividend aristocrats and defensive stocks

#### **Balanced Portfolio** (Moderate Risk - 15-20% volatility)
```
duk, xom, cvx, pg, ko, baba, spy, vti
```
Focus: Diversified across sectors and geographies

#### **Growth Portfolio** (High Risk - 25-35% volatility)
```
AAPL, MSFT, NVDA, GOOGL, AMZN, TSLA, META
```
Focus: Technology and high-growth companies

#### **Diversified Portfolio** (Comprehensive - 19 assets)
```
aapl, msft, nvda, jnj, pfe, amzn, tsla, jpm, bac, nee, duk, xom, cvx, pg, ko, baba, spy, vti, eem
```
Focus: Maximum diversification across all sectors

### Understanding the Outputs

#### 1. **Daily Statistics Table**
Shows for each asset:
- Mean Return (average daily return)
- Standard Deviation (volatility)
- Variance (squared volatility)

#### 2. **Efficient Frontier Plot**
- **X-axis**: Portfolio Risk (Standard Deviation)
- **Y-axis**: Portfolio Return
- **Color**: Sharpe Ratio (higher is better)
- Each point represents a possible portfolio allocation

#### 3. **Optimized Portfolio Weights**
Shows the percentage allocation to each stock that achieves your optimization goal.

## 🔬 Technical Details

### Optimization Algorithms

#### 1. Efficient Frontier Calculation
Uses Monte Carlo simulation to generate random portfolio combinations:
- Generates 10,000 random weight combinations
- Ensures weights sum to 1 (fully invested)
- Calculates annualized returns and risks
- Computes Sharpe ratios for each portfolio

#### 2. Constrained Optimization
Uses `scipy.optimize.minimize` with constraints:
- **Sum-to-one constraint**: All weights must sum to 1
- **Non-negativity constraint**: No short selling (weights ≥ 0)
- **Target constraints**: Risk or return targets as specified

#### 3. Risk-Return Calculations
- **Portfolio Return**: `R_p = Σ(w_i × r_i)` annualized × 252
- **Portfolio Risk**: `σ_p = √(w^T × Σ × w)` annualized × √252
- **Sharpe Ratio**: `(R_p - R_f) / σ_p`

Where:
- `w_i` = weight of asset i
- `r_i` = return of asset i
- `Σ` = covariance matrix
- `R_f` = risk-free rate

## 📁 Project Structure

```
Portfolio-Optimization-Tooling/
│
├── testing.ipynb                          # Main interactive notebook
├── Financial Engineering_IEDA3330         # Research paper and methodology
│   Final Report_compressed.pdf
├── README.md                              # This file
└── requirements.txt                       # Python dependencies
```

## 📚 Background & Theory

This project implements **Modern Portfolio Theory (MPT)**, developed by Harry Markowitz, which earned him the Nobel Prize in Economics. Key concepts:

- **Diversification**: Combining assets reduces overall portfolio risk
- **Efficient Frontier**: The set of optimal portfolios offering the highest expected return for a given level of risk
- **Sharpe Ratio**: Measures risk-adjusted returns (return per unit of risk)
- **Mean-Variance Optimization**: Balancing expected returns against risk

For detailed theoretical background and implementation details, please refer to the included research paper: `Financial Engineering_IEDA3330 Final Report_compressed.pdf`

## 🎓 Educational Use

This toolkit is designed for:
- **Students** learning portfolio theory and quantitative finance
- **Researchers** exploring portfolio optimization techniques
- **Individual investors** seeking to understand portfolio construction
- **Finance professionals** prototyping investment strategies

## ⚠️ Important Disclaimers

1. **Not Financial Advice**: This tool is for educational and research purposes only. It does not constitute financial advice.
2. **Past Performance**: Historical returns do not guarantee future results.
3. **Model Limitations**: The tool assumes normally distributed returns and uses historical data, which may not reflect future market conditions.
4. **Transaction Costs**: The model does not account for trading fees, taxes, or market impact.
5. **Market Risk**: All investments carry risk. Diversification does not eliminate the risk of investment losses.

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

## 📄 License

This project is available for educational and research purposes. Please see the LICENSE file for details.

## 👥 Authors

- **Nikhil Joseph** - [nikhiljoseph2004](https://github.com/nikhiljoseph2004)

## 🙏 Acknowledgments

- Course: Financial Engineering (IEDA3330)
- Built with [yfinance](https://github.com/ranaroussi/yfinance) for market data
- Optimization powered by [SciPy](https://scipy.org/)
- Data analysis with [pandas](https://pandas.pydata.org/) and [NumPy](https://numpy.org/)

## 📞 Contact

For questions or feedback, please open an issue on GitHub or contact through the repository.

---

**Happy Investing! 📈**
