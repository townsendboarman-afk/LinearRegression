Does a 200-Day SMA Crossover Generate Alpha?

A regression analysis of a simple trend-following strategy on SPY, using the Fama-French three-factor model.

Article: Regression Analysis for Trading Strategy Development (Medium)

The question:

The strategy holds SPY when its price is above its 200-day simple moving average, and holds cash (earning the risk-free rate) otherwise. Does this generate alpha, meaning returns that standard risk factors can't explain?

Null hypothesis: the strategy does not generate alpha (α = 0).
Alternative hypothesis: the strategy generates alpha (α ≠ 0).

Method:

Download daily SPY prices and monthly Fama-French factors (2000–2023) with tidyfinance.
Build the strategy in Polars, lagging both the moving average and the trading signal by one day to avoid lookahead bias.
Compound daily returns to monthly excess returns. Cash days earn the risk-free rate, which counts as zero excess return.
Regress the strategy's excess returns on the market, value (HML) and size (SMB) factors, using Newey-West (HAC) standard errors.
Check the regression assumptions: normality, independence, multicollinearity and homoscedasticity.

The model:

$$R_{p,t} - R_{f,t} = \alpha + \beta_{1}(R_{m,t} - R_{f,t}) + \beta_{2}\,HML_t + \beta_{3}\,SMB_t + \varepsilon_t$$
Results:

- Alpha (monthly)
    - coefficient: 0.0026
    - z: 1.84	
    - p-value: 0.066
- Market (Mkt-RF)	
    - coefficient: 0.41	
    - z: 5.43	
    - p-value: < 0.001
- Value (HML)	
    - coefficient: 0.06	
    - z: 0.96	
    - p-value: 0.339
- Size (SMB)	
    - coefficient: −0.01	
    - z: −0.16	
    - p-value: 0.873

279 monthly observations (October 2000 to December 2023), R² = 0.41.

We fail to reject the null hypothesis. The strategy's alpha of about 3.1% a year is not statistically significant at the 5% level. Its returns are mostly explained by holding roughly 41% of the market's exposure.

The diagnostics also show that a linear model fits a timing strategy poorly. The residuals form a distinctive X shape against the fitted values, because the strategy switches between two regimes (fully invested or in cash) while the model fits a single market beta to both.

Repository contents
- regression.ipynb: the full analysis, with explanations of each step and the diagnostic plots.

Limitations and next steps:

- Tests a single asset and a single moving-average length. Trying many lengths and reporting the best one would require a multiple-testing correction.
- Ignores transaction costs and taxes, though the strategy trades infrequently.
Possible extensions: 
- an interaction term between the market factor and the fraction of each month invested, a Treynor-Mazuy squared market term to test for market timing, and the momentum factor (UMD).
