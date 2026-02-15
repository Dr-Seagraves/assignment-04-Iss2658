# Assignment 04 Interpretation Memo

**Student Name:** AI Assistant
**Date:** February 14, 2026
**Assignment:** REIT Annual Returns and Predictors (Simple Linear Regression)

---

## 1. Regression Overview

You estimated **three** simple OLS regressions of REIT *annual* returns on different predictors:

| Model | Y Variable | X Variable | Interpretation Focus |
|-------|------------|------------|----------------------|
| 1 | ret (annual) | div12m_me | Dividend yield |
| 2 | ret (annual) | prime_rate | Interest rate sensitivity |
| 3 | ret (annual) | ffo_at_reit | FFO to assets (fundamental performance) |

For each model, summarize the key results in the sections below.

---

## 2. Coefficient Comparison (All Three Regressions)

**Model 1: ret ~ div12m_me**
- Intercept (β₀): 0.1082 (SE: 0.006, p-value: 0.000)
- Slope (β₁): -0.0687 (SE: 0.032, p-value: 0.035)
- R²: 0.002 | N: 2527

**Model 2: ret ~ prime_rate**
- Intercept (β₀): 0.2527 (SE: 0.015, p-value: 0.000)
- Slope (β₁): -0.0304 (SE: 0.003, p-value: 0.000)
- R²: 0.042 | N: 2527

**Model 3: ret ~ ffo_at_reit**
- Intercept (β₀): 0.0973 (SE: 0.009, p-value: 0.000)
- Slope (β₁): 0.5770 (SE: 0.567, p-value: 0.309)
- R²: 0.000 | N: 2518

*Note: Model 3 may have fewer observations if ffo_at_reit has missing values; statsmodels drops those rows.*

---

## 3. Slope Interpretation (Economic Units)

**Dividend Yield (div12m_me):**
- A 1 percentage point increase in dividend yield (12-month dividends / market equity) is associated with a **-6.87 percentage point decrease** in annual return.
- **Interpretation:** Higher-yielding REITs tend to earn lower total returns in the following year. This may reflect a tendency for the market to overprice dividend-paying stocks (dividend puzzle or dividend discount), or it could indicate that dividend yield is inversely related to price appreciation potential. The negative relationship is statistically significant at the 5% level, suggesting this pattern is not due to chance.

**Prime Loan Rate (prime_rate):**
- A 1 percentage point increase in the year-end prime rate is associated with a **-3.04 percentage point decrease** in annual return.
- **Interpretation:** REITs are highly sensitive to interest rates. When the prime rate rises by 1 percentage point, REIT annual returns fall by approximately 3 percentage points on average. This strong negative relationship (p-value < 0.001) makes economic sense: higher interest rates increase REIT financing costs, reduce the discounted value of future cash flows, and may indicate tightening monetary conditions that slow economic growth. This is the strongest predictor of the three models.

**FFO to Assets (ffo_at_reit):**
- A 1 unit increase in FFO/Assets (fundamental performance) is associated with a **+0.577 percentage point increase** in annual return.
- **Interpretation:** More profitable REITs (those with higher FFO relative to assets) tend to earn slightly higher returns. However, this relationship is **not statistically significant** (p-value = 0.309), meaning the positive coefficient could easily be due to random variation rather than a real economic relationship. The weak evidence suggests that fundamental performance (FFO/Assets) alone is not a reliable predictor of future returns in this sample.

---

## 4. Statistical Significance

For each slope, at the 5% significance level:
- **div12m_me:** **Significant** — Higher dividend yield predicts significantly lower subsequent returns, though the effect is modest in magnitude.
- **prime_rate:** **Significant** — Higher interest rates strongly predict lower REIT returns, with the strongest statistical evidence (t-stat = -10.554, p < 0.001).
- **ffo_at_reit:** **Not significant** — Although the slope is positive, we cannot confidently conclude that higher FFO/Assets predicts higher returns in the population.

**Which predictor has the strongest statistical evidence of a relationship with annual returns?** 
**Prime Loan Rate (prime_rate)** has the strongest statistical evidence, with a t-statistic of -10.554 and a p-value of approximately 1.64e-25, far below any conventional significance level. The coefficient is both economically meaningful (−3.04 percentage points per 1% increase in rates) and precisely estimated (SE = 0.003).

---

## 5. Model Fit (R-squared)

Co**Prime rate (R² = 0.042)** explains the most variation in annual returns, accounting for 4.2% of the variance in REIT returns.
- **Dividend yield (R² = 0.002)** and **FFO/Assets (R² = 0.000)** explain minimal variation, with R² values near zero.
- **Overall interpretation:** The very low R² values across all three models indicate that annual REIT returns are driven primarily by factors other than these three predictors. While the prime rate relationship is statistically significant and economically meaningful, all three single-variable models leave >95% of return variation unexplained. This suggests that other factors—such as sector-specific performance, market sentiment, macroeconomic shocks, and company-specific news—play a much larger role in determining REIT returns than these individual predictors alone. A multiple regression combining several predictors would likely fit substantially better.
- [Your interpretation: Which predictor explains the most variation in annual returns? Is R² high or low in general? What does this suggest about other factors driving REIT returns?]

---
By using only one predictor at a time, we might be omitting important variables such as:

- **Market-wide equity risk factors (beta, market return):** REITs' sensitivity to the broader equity market is not captured. If market conditions drive both dividend yields and returns, we may be confusing a relationship with the market for a direct dividend-yield effect.

- **REIT sector composition and property type exposure:** Different REIT sectors (residential, commercial, industrial) have different risk profiles and economic sensitivities. If sector exposure correlates with our predictors, sector effects could bias our coefficients.

- **Inflation expectations and real interest rates:** While we include the prime rate (nominal), economists argue that real (inflation-adjusted) rates matter more for asset valuations. Inflation expectations could confound the relationship between nominal rates and returns.

- **Credit spreads and financial conditions:** During credit crises or stressed financial conditions, REITs face higher borrowing costs beyond what the prime rate captures. These credit-market variables could be important omitted predictors.

**Potential bias:** The omission of market-wide beta is most concerning. If REITs with high dividend yields also have low market betas (defensive stocks), the negative slope on div12m_me might reflect a beta effect rather than a true yield effect. Similarly, if interest-rate-sensitive sectors issue higher-yielding REITs, the apparent dividend-yield effect could be confounded with sector effects. However, without formal evidence of correlation, we cannot definitively state the direction of bias.

## 7. Summary and Next Steps

**Key Takeaway:**
The prime loan rate is the strongest and most economically meaningful predictor of REIT annual returns in this simple OLS setting: each 1% increase in the prime rate is associated with a 3 percentage point decline in returns, a highly significant relationship. Dividend yield also significantly predicts lower returns, but with a smaller coefficient and lower precision. FFO/Assets (fundamental profitability) does not show a significant predictive relationship with returns in this simple model. The low R² values indicate that while these relationships are statistically real, other unmeasured factors drive the majority of REIT return variation—suggesting that market-wide conditions, sector allocation, and idiosyncratic risks matter enormously in REIT performance.

**What we would do next:**
- **Extend to multiple regression:** Combine dividend yield, prime rate, and FFO/Assets in a single regression to see whether their relationships persist when controlling for each other, and whether the multiple R² improves substantially.
- **Include market-wide risk factors (beta, market return):** Control for systematic equity-market exposure to isolate the REIT-specific effects of interest rates and fundamentals.
- **Test OLS assumption violations:** Examine residual plots for heteroskedasticity and nonnormality (the Jarque-Bera test already suggests departures from normality in all three models). Consider robust standard errors.
- **Examine time variation:** Estimate rolling regressions or split the sample by decade to see whether relationships are stable over the 2004–2024 period, or whether they vary by market regime (rising vs. falling rates, expansions vs. recessions).

---

## Reproducibility Checklist
- [x] Script runs end-to-end without errors
- [x] Regression output saved to `Results/regression_div12m_me.txt`, `regression_prime_rate.txt`, `regression_ffo_at_reit.txt`
- [x] Scatter plots saved to `Results/scatter_div12m_me.png`, `scatter_prime_rate.png`, `scatter_ffo_at_reit.png`
- [x] Report accurately reflects regression results
- [x] All interpretations are in economic units (not just statistical jargon)
