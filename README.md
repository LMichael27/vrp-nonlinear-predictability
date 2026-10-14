# Variance Risk Premia and Stock Return Predictability

**Nonlinear Evidence from Polynomial and Spline Models**

Michael Ladaa · Quantitative Research Division, Minerva Investment Management Society (Bocconi University) · Academic Year 2025/2026

📄 **Paper:** [`paper/VRP_Nonlinear_Predictability_Ladaa.pdf`](paper/VRP_Nonlinear_Predictability_Ladaa.pdf)

---

## Overview

This project revisits the predictive power of the **variance risk premium (VRP)**, the spread between implied and realized variance, for future U.S. equity excess returns. It builds on Bollerslev, Tauchen & Zhou (2009). Their evidence rests on linear predictive regressions. This project tests whether allowing for **nonlinearities** improves forecasting performance.

Using monthly data from **2008 to 2021** (about 150 observations), the analysis moves from a linear benchmark to polynomial and cubic B-spline specifications. It then adds regime-dependent extensions based on VIX3M volatility states. All models are estimated by OLS with Newey–West (HAC) standard errors and evaluated with an expanding-window out-of-sample (OOS) forecast.

## Key Results

| Model | In-sample R² | OOS R² |
|---|---|---|
| Linear (VRP only) | 0.015 | 0.008 |
| Quadratic polynomial | – | 0.086 |
| Cubic B-spline | 0.253 | **0.256** |
| Regime-dependent spline | 0.332 | 0.176 |

- The **linear** relationship is weak and statistically insignificant in this sample.
- A simple **quadratic** term is highly significant and reveals a concave VRP–return relationship.
- **Spline** models deliver the best out-of-sample performance, which points to a strongly nonlinear relationship.
- **Regime-dependent** splines fit better in sample but forecast worse out of sample, which is consistent with overfitting in a small sample.

## Repository Structure

```
├── paper/
│   └── VRP_Nonlinear_Predictability_Ladaa.pdf   # Research paper
├── Data.ipynb                         # Data pipeline: builds data/final.csv
├── Exploratory_Data_Analysis.ipynb    # Summary stats, correlations, VIX regimes
├── Regression_final.ipynb             # Linear, polynomial, spline & regime models + OOS evaluation
├── data/
│   ├── final.csv                      # Final monthly dataset used for estimation
│   ├── cay_current.csv                # Lettau–Ludvigson consumption-wealth ratio (cay)
│   ├── ie_data.xls                    # Robert Shiller's market data (P, D, E)
│   └── data.csv                       # Intermediate output of an earlier pipeline version
├── archive/                           # Earlier notebook versions (not needed for replication)
└── requirements.txt
```

## Data

| Variable | Description | Source |
|---|---|---|
| RV | Realized variance from 5-minute S&P 500 (SPY) returns | [Kaggle: Intraday 1-min S&P 500 2008–2021](https://www.kaggle.com/datasets/gratefuldata/intraday-stock-data-1-min-sp-500-200821) |
| IV | Implied variance, (VIX/100)² | Yahoo Finance (`^VIX`) |
| VIX3M | 3-month implied volatility (regime definition) | Yahoo Finance (`^VIX3M`) |
| Excess return | S&P 500 return minus 3M T-bill | Kaggle / FRED (`TB3MS`) |
| log P/E, log P/D | Valuation ratios | [Robert Shiller](http://www.econ.yale.edu/~shiller/data.htm) |
| CAY | Consumption-wealth ratio (imputed 2020–21 via Kalman filter) | [Lettau & Ludvigson](https://sites.google.com/view/martinlettau/data) |
| DFSP, TMSP, RREL | Default spread, term spread, relative T-bill rate | FRED (`BAA`, `AAA`, `GS10`, `TB3MS`) |

> **Note:** The raw 1-minute SPY file (`1_min_SPY_2008-2021.csv`, ~145 MB) exceeds GitHub's file size limit and is **not included**. To rerun `Data.ipynb` from scratch, download it from Kaggle and place it in `data/`. All downstream notebooks run directly on the included `data/final.csv`.

## Reproducing the Results

```bash
git clone https://github.com/LMichael27/vrp-nonlinear-predictability.git
cd vrp-nonlinear-predictability
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

1. *(Optional)* `Data.ipynb`: rebuild `data/final.csv` (requires the Kaggle file plus internet access for Yahoo Finance and FRED).
2. `Exploratory_Data_Analysis.ipynb`: summary statistics, correlations and volatility regimes.
3. `Regression_final.ipynb`: all model estimations and out-of-sample forecasts.

## References

1. Bollerslev, T., Tauchen, G., & Zhou, H. (2009). Expected stock returns and variance risk premia. *Review of Financial Studies*, 22(11), 4463–4492. [doi:10.1093/rfs/hhp008](https://doi.org/10.1093/rfs/hhp008)
2. Lettau, M., & Ludvigson, S. (2001). Consumption, aggregate wealth, and expected stock returns. *Journal of Finance*, 56(3), 815–849.
3. Shiller, R. J. (2000). *Irrational Exuberance*. Princeton University Press.

## Disclaimer

This work was prepared exclusively for educational and research purposes. It does not constitute investment advice or a solicitation to buy or sell any financial instruments.
