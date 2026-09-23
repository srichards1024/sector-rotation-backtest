# Sector Rotation Portfolio Backtest

Case study from STAT 4170 (Financial Time Series and Forecasting) at UVA, Fall 2026.

I built a mean-variance portfolio that rotates across the 11 SPDR sector ETFs and tested whether it could beat SPY or a simple equal-weight portfolio. Before running the backtest, I looked at how reliable the inputs (expected returns and covariances) actually are, and how much the optimal weights move when those inputs change.

## Results

I ran a walk-forward backtest from 2019 to 2026 with monthly rebalancing, 10 bp transaction costs, and a 50 bp annual fee. None of the sector portfolios beat SPY.

| Portfolio | Annual return | Volatility | Sharpe | Max drawdown |
|---|---|---|---|---|
| SPY | 16.8% | 19.7% | 0.85 | 33.7% |
| Equal-weight (11 sectors) | 13.5% | 18.5% | 0.73 | 36.2% |
| Optimized (long-only) | 15.1% | 23.5% | 0.64 | 34.8% |

The optimized portfolio earned more than equal-weight but took on much more risk to do it, so it came out last on a risk-adjusted basis.

The main reason is that the expected return estimates were mostly noise. For 82% of sectors, the bootstrap confidence interval for the mean return included zero. Because of that, the optimizer ended up concentrated in about 1.5 sectors on average instead of diversifying.

I also tested a few alternatives. Risk parity (Sharpe 0.71) and a momentum tilt (0.69) closed most of the gap to equal-weight but still trailed SPY, and pure minimum-variance did worse (0.50).

Sharpe ratios use a 0% risk-free rate.

## Notebook sections

1. Data pipeline that pulls daily prices from yfinance and caches them locally
2. Estimation error in means and covariances (simulation, bootstrap, rolling windows)
3. Sensitivity of the optimal weights to risk aversion, returns, correlations, and volatility, plus long-only and leverage constraints
4. Rebalancing frequency and transaction costs
5. Walk-forward backtest, performance tearsheet, and recommendation
6. Alternative strategies (minimum-variance, risk parity, momentum)

Built with Python, pandas, NumPy, cvxpy, yfinance, and matplotlib.
