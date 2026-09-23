# Austin home prices: did the income story break, and can you predict next month?

This started as a 73-cell class notebook with a lot of duplicated code. I rebuilt it into something I can actually defend: a clean data pipeline, a formal test for whether Austin's income-price relationship broke after 2020, and an ML model that tries to predict next month's median price.

## The short version

- Income used to explain Austin prices well (R² = 0.765 before 2020). After 2020 it fell apart (R² = 0.386). A Chow test says the break is real (F = 17.3, p < 0.001).
- Ridge regression predicted 2022–2024 prices with RMSE $10,647, about 20% better than just guessing last month's price ($13,334).
- Random Forest and gradient boosting both lost to the naive guess. 72 training rows isn't enough for them, and trees can't extrapolate into a market that's changed.
- The model with the best in-sample fit (R² = 0.765) was the worst forecaster. That one's worth remembering.

## What's in here

`austin_housing_ml.ipynb` — the whole thing, in three parts:

1. Builds a monthly panel (2015–2024) from FRED mortgage rates, Redfin Austin data, and Census income.
2. Tests the pre/post-2020 break with OLS by era plus a Chow test.
3. Forecasts with lagged features and a strict time-based split (train 2015–2021, test 2022–2024). Baselines included so the models have to earn it.

## Running it

You need Anaconda, nothing else. Open the notebook and Shift+Enter top to bottom.

Part 1 needs the raw CSVs in the same folder (`MORTGAGE30US.csv`, `Redfin.csv`, the income files). If you already have `austin_final_research_dataset.csv`, skip to part 2.

Running it saves three charts next to the notebook.

## Things I'd flag before you ask

Only about 108 usable months, so take the model ranking as suggestive. Income is yearly data repeated across months. It's one city, so the 2020 break might be an Austin thing. And this predicts; it doesn't prove cause.

## Data

FRED (MORTGAGE30US), Redfin Data Center, Census ACS. All public.
