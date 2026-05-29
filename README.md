# Economic Analysis and GDP Prediction

> A data science project exploring global macroeconomic patterns from 2010 to 2024 using international economic indicators.

## Dataset

It covers **2010–2024** across a all countries, but this project used European nations (Austria, Belgium, France, Germany, Netherlands, Spain, Sweden, Switzerland, United Kingdom, and others) as well as major global economies (Canada, United States, China, Japan, Korea, and the Russian Federation).

**Key indicators included:**
- GDP (Current USD)
- GDP Growth (% Annual)
- GDP per Capita (Current USD)
- Inflation (CPI % and GDP Deflator %)
- Unemployment Rate (%)
- Government Expense (% of GDP)
- Government Revenue (% of GDP)
- Tax Revenue (% of GDP)
- Current Account Balance (% of GDP)
- Gross National Income GNI (USD)



## Project Overview

This project investigates how national economies evolve over time and what factors shape their growth trajectories. Rather than focusing on a single country, it takes a broad comparative perspective across advanced, emerging, and resource-driven economies.



## Methodology

### 1. Exploratory Data Analysis (EDA)
- Inspected and documented missing values by country and feature
- Applied two-step imputation: forward/backward fill for minor gaps, and KNN imputation for substantial missing data
- Removed high-missingness features: `Interest Rate (Real, %)`, `Public Debt (% of GDP)`, and `country_id`
- Applied log transformation to GDP, GNI, and GDP per Capita to reduce skewness and stabilize variance
- Detected outliers using the IQR method (retained as they reflect real economic phenomena)
- Assessed normality of all key variables (only GDP Growth and Current Account Balance showed near-normal distributions)

### 2. Comparative Visualization
- **Average GDP Growth per Country**: China, Ireland, and Malta led with >6% growth; Ukraine and Greece showed contraction
- **Risk vs. Reward Analysis**: Quadrant plot categorizing countries by growth reward and volatility risk — China and Lithuania emerged as ideal (high reward, low risk)
- **Misery Index Heatmap**: North Macedonia recorded the highest combined unemployment + inflation burden; Japan the lowest
- **Post-COVID Recovery**: Indexed GDP to 2019 baseline (Bulgaria recovered fastest; Japan slowest)
- **Comparative Investment Analysis**: Luxembourg and Switzerland ranked highest in 2023 using a composite investment score
- **Government Revenue Composition**: Denmark relies most heavily on tax revenue; China and Switzerland rely more on non-tax sources

### 3. Correlation Analysis
- Computed a full correlation matrix for all numeric indicators
- **Key finding**: GNI and GDP showed a near-perfect correlation — because GNI is mathematically derived from GDP (GDP + net foreign income), making them nearly redundant
- Government Expense, Revenue, and Tax Revenue showed strong mutual correlations, reflecting consistent fiscal patterns
- Most other indicators showed weak to moderate correlations

### 4. Predictive Modeling
Linear regression was used to predict Log(GDP) for 2023 under two scenarios.



## Models & Performance

### Linear Regression: Scenario 1 (with GNI as a feature)

| Metric | Value |
|--------|-------|
| R² Score | **0.9986** |
| MAE | 0.0426 |
| MSE | 0.0058 |
| RMSE | 0.0760 |

> **Misleading result.** The extremely high R² is not a sign of genuine predictive power. GNI is **mathematically derived from GDP** — including it as a predictor essentially reproduces an accounting identity (GNI = GDP + net foreign income). The model learns a near-tautology, not real economic relationships. This inflated performance offers no meaningful insight.



### Linear Regression: Scenario 2 (without GNI)

| Metric | Value |
|--------|-------|
| R² Score | **0.1075** |
| MAE | 1.5758 |
| MSE | — |
| RMSE | 1.9284 |

> **More honest and economically meaningful.** Only ~10.75% of GDP variance is explained. The included features (GDP Growth, Inflation, Unemployment, Current Account Balance, Government Expense) capture short-term deviations and rates of change — not the absolute size of an economy.

It is the more valid and informative model, despite its lower accuracy. It avoids data leakage through mathematical dependency and honestly reflects the difficulty of predicting GDP from macroeconomic flow variables alone.



## Note on Mathematical Relationships Between Features

A critical finding of this project is that several features in macroeconomic datasets are not independent.They share mathematical or accounting relationships:

- **GDP and GNI** are linked by definition: `GNI = GDP + net income from abroad`. This makes GNI a near-redundant predictor of GDP.
- **Government Revenue and Tax Revenue** are structurally related (tax revenue is a component of total revenue).
- **Log-transformed wealth variables** (Log GDP, Log GNI, Log GDP per Capita) are all highly collinear.

Including such variables together in a model creates **data leakage** or **multicollinearity**, producing misleadingly high performance metrics that do not reflect genuine predictive ability. Feature selection must account for these structural dependencies, not just statistical correlations.
