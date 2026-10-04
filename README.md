# Enhanced Index Tracking with MIQP

This project implements an **Enhanced Index Tracking** approach for the **Swiss Market Index (SMI)** using **Mixed-Integer Quadratic Programming (MIQP)**.

The objective is to construct a portfolio that closely tracks the SMI while achieving a predefined minimum expected excess return. The optimisation model uses historical stock prices, SMI constituent weights and the covariance matrix of stock returns.

The implementation is based on the portfolio optimisation model presented by Gnägi and Strub (2020), with a simplified setup that does not consider transaction costs or cash holdings.

## Methodology

The portfolio is constructed by minimising **Tracking Error Variance (TEV)** relative to the SMI.
TEV measures the variance of the difference between the portfolio weights and the corresponding index weights:

$$
TEV =
\sum_{i \in U}\sum_{j \in U}
\sigma_{ij}
\left(
\frac{P_{iT}X_i}{C} - w_i^I
\right)
\left(
\frac{P_{jT}X_j}{C} - w_j^I
\right)
$$

where $\sigma_{ij}$ denotes the covariance between the returns of stocks $i$ and $j$, $P_{iT}$ the stock price at the optimisation date, $X_i$ the number of shares held, $C$ the available investment budget and $w_i^I$ the corresponding SMI weight.

The optimisation model incorporates the following constraints:

- full investment of the available budget
- no short selling
- minimum expected excess return relative to the SMI
- minimum and maximum portfolio weights for selected stocks
- portfolio cardinality constraint

The model is formulated as a mixed-integer quadratic program and solved using **Gurobi**.

The SMI constitutes the investment universe, with all index constituents considered as potential portfolio holdings. The model uses a minimum portfolio weight of 1%, a maximum weight of 30%, a target excess return of 2% and a portfolio size of 10 stocks.

## Portfolio Construction

The repository contains two Jupyter notebooks covering the two stages of the portfolio construction process.

### Initialisation

`enhanced_index_tracking_initialisation.ipynb`

The initialisation notebook constructs the first portfolio based on a fixed investment budget of **CHF 100,000**.

It covers:

1. preparation of historical SMI price data
2. calculation of stock returns and the covariance matrix
3. preparation of SMI constituent weights
4. formulation of the MIQP
5. optimisation with Gurobi

### Rebalancing

`enhanced_index_tracking_rebalancing.ipynb`

The rebalancing notebook applies the same optimisation framework to an existing portfolio.

Instead of using a fixed budget, the available budget is determined from the **current market value of the existing holdings**. The portfolio is then re-optimised based on the updated market data.

## Model Parameters

| Parameter | Value |
|---|---:|
| Minimum portfolio weight | 1% |
| Maximum portfolio weight | 30% |
| Minimum expected excess return | 2% |
| Portfolio cardinality | 10 stocks |
| Initial investment | CHF 100,000 |

## Tools & Technologies

- Python
- Jupyter Notebook
- Gurobi
- pandas
- NumPy
- Matplotlib

## Reference

Gnägi, M. and Strub, O. (2020). *Tracking and outperforming large stock-market indices*. Omega, 90, 101999.

The model formulation and notation used in this project follow the framework presented by Gnägi and Strub (2020).
