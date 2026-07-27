<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/ML-K--Means%20Clustering-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Factors-Fama--French%205-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Optimizer-PyPortfolioOpt-purple?style=for-the-badge" />
</p>

<h1 align="center">🤖 Unsupervised Trading Strategy</h1>

<p align="center">
  <b>Letting K-Means discover market regimes — then trading the momentum cluster with an optimized portfolio</b>
</p>

---

## 🔍 What it does

Instead of telling a model what a "good stock" looks like, this strategy lets **unsupervised learning find structure in the cross-section of equities** and builds a monthly-rebalanced portfolio from the cluster that historically carries momentum.

## ⚙️ The pipeline

1. **Universe construction** — pull historical prices via `yfinance`, compute rolling 10-day average dollar volume, and keep only the most liquid names with sufficient history.
2. **Feature engineering** — technical indicators via `pandas_ta` (incl. RSI), plus momentum features at 1/2/3/6/9/12-month lags with outlier clipping.
3. **Factor exposures** — download the **Fama-French 5-factor** dataset and estimate rolling betas (`Mkt-RF`, `SMB`, `HML`, `RMW`, `CMA`) per stock with `RollingOLS`.
4. **Clustering** — run **K-Means (4 clusters)** each month on the feature panel, with RSI-guided initial centroids (30 / 45 / 55 / 70) so clusters are interpretable across dates.
5. **Selection** — pick the momentum cluster (cluster 3, anchored near RSI 70) as the candidate long book each month.
6. **Portfolio optimization** — feed selected names into **PyPortfolioOpt** (mean historical returns + efficient frontier) to weight the final portfolio.
7. **Visualization** — per-date cluster scatter plots (returns vs market beta) to see regime structure evolve.

## 🧰 Tech stack

`pandas` · `numpy` · `yfinance` · `pandas_ta` · `scikit-learn` · `statsmodels` · `pandas-datareader` · `PyPortfolioOpt` · `matplotlib`

## 🚀 Getting started

```bash
git clone https://github.com/sinhaarya04/UnsupervisedTradingStrategy.git
cd UnsupervisedTradingStrategy
pip install pandas numpy yfinance pandas_ta scikit-learn statsmodels pandas-datareader PyPortfolioOpt
jupyter notebook UnsupervisedStrategy.ipynb
```

---

<p align="center"><i>Research project — not investment advice.</i></p>
