# systematic-active-rotation
Systematic active investment strategy combining momentum and mean reversion signals on Fama-French 49 industry portfolios with dynamic regime-based allocation.

---

## Data

All datasets used in this strategy are stored in the `data/` folder:

- `data/data_ff3.csv`: Fama-French 3-factor monthly returns  
- `data/data_ff5.csv`: Fama-French 5-factor monthly returns  
- `data/data_ind49.csv`: Monthly returns for 49 industry portfolios  

---

## Objective

This project builds a dual-signal investment strategy using the Fama-French 49 industry portfolios from 1931–2024. It blends:
- A **momentum strategy** (130/30 long-short) based on 12-month returns (skipping most recent month),
- A **mean reversion strategy** (long-only) based on 60-month cumulative underperformance.

The strategy is evaluated both in-sample and through randomized 5-year out-of-sample windows. Performance is assessed using Sharpe ratio, drawdown, and cumulative returns, with an additional market regime filter (bull, bear, neutral) to dynamically adapt allocations.

---

## Strategies Analyzed

### Strategy A: Mean Reversion (Long-Only)
- Signals from 60-month cumulative return  
- Buys bottom 20th percentile, trims top 90th percentile  
- Reallocation capped at 30% per month  
- Inversely weighted to return ranks  

### Strategy B: Momentum (Long/Short)
- 12-month returns, excluding most recent month  
- Long top 15 industries, short bottom 15  
- Gross exposure: 130% long / 30% short  
- Positions scaled monthly  

### Blended Allocation Strategies
- **Static Blend**: 50/50 split between Strategies A and B  
- **Dynamic Regime-Based Blend**:
  - Bull market (>10% 12m return): 100% Momentum  
  - Bear market (<–10% 12m return): 100% Mean Reversion  
  - Neutral market: 70% Mean Reversion / 30% Momentum  

---

## Performance Summary

| Strategy         | Total Return | Sharpe Ratio | Max Drawdown |
|------------------|--------------|--------------|---------------|
| Mean Reversion   | $9,480       | 0.387        | –65.87%       |
| Momentum         | $48,800      | 0.655        | –59.17%       |
| Static Blend     | $27,500      | 0.634        | –53.24%       |
| Dynamic Blend    | $74,000      | 0.651        | –58.02%       |

---

## Robustness Test: 5-Year Rolling Windows

1,000 randomly selected non-overlapping 5-year windows were evaluated.  
The **Dynamic Blend** strategy led with:

- **Average 5Y Total Return**: 132.18%  
- **Average Sharpe Ratio**: 0.858  
- **Minimum 5Y Return**: –46.03%  

---

## Market Regime Analysis

| Regime       | Months | Best Performer         |
|--------------|--------|-------------------------|
| Bull Market  | 576    | Dynamic Blend (Momentum-heavy)  
| Neutral      | 403    | Dynamic Blend (Mixed Tilt)  
| Bear Market  | 203    | Mean Reversion / Dynamic Blend  

Dynamic allocation preserves capital in bear markets and captures upside in bullish regimes.

---

## Industry-Level Insights

- **Top Momentum Contributors (Bull Markets)**: Chips, Fun, Aerospace  
- **Top Reversion Picks (Bear Markets)**: Sectors that had already declined — Gold, Utilities, Personal Services  

K-Means clustering shows industries rotate in and out of high/low risk-return profiles, supporting the reversion hypothesis.

---

## Regression & Factor Exposure

- **Dynamic Blend** has an R² = 0.855 with FF5  
- Monthly alpha = 0.21%  
- Loads positively on:  
  - Market (MKT)  
  - Size (SMB)  
  - Profitability (RMW)  
  - Investment (CMA)  
- Negligible or negative HML exposure (value factor)

---

## Conclusion

Combining momentum and mean reversion via regime-based blending improves overall portfolio quality.  
Dynamic strategies:
- Smooth volatility
- Reduce drawdowns
- Adapt to market conditions
- Enhance long-term return and risk-adjusted metrics

---

## Files

- [`Systematic_Active_Rotation.ipynb`](./Systematic_Active_Rotation.ipynb): Full code + backtesting logic  
- `data/`: Contains all raw data CSVs  
- `plots/`: Visuals for return, drawdown, industry heatmaps, factor regression  
- `.gitignore`, `LICENSE`: Project hygiene  

---

## 👤 Author

Neel Shah  
M.S. Quantitative Finance, Northeastern University  
CFA Level III Candidate | Quant Strategies | Asset Allocation | Python  
[LinkedIn](https://www.linkedin.com/in/neelshah1903)

---
