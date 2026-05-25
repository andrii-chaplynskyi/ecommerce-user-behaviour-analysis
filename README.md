# E-commerce Sales and User Behaviour Analysis

End-to-end analysis of an e-commerce dataset covering sessions, traffic sources, user registration behaviour and order revenue. The project moves from raw event data in BigQuery through Python-based exploration, statistical testing, and a Tableau Public dashboard, with the goal of producing decision-ready insight rather than just descriptive statistics.

## Business Context

The analysis treats the dataset as if it belonged to an online furniture retailer that wants to understand:

- Where revenue actually comes from (continent, country, device, traffic channel).
- Whether registration and email-engagement behaviour translates into measurable revenue differences.
- Whether session volume reliably predicts revenue at a daily level.
- Whether traffic-channel mix differs in a statistically meaningful way across major regions.

The intent is to demonstrate the analyst workflow a marketing or product team would expect: framing the question, querying the right slice of data, validating assumptions with statistical tests, and ending with a visual summary stakeholders can read in two minutes.

## Business Questions

1. Which continents, countries, devices and traffic channels generate most of the revenue, and how concentrated is that share?
2. Do registered users behave differently from non-registered users in terms of daily revenue contribution?
3. Among registered users, does email verification or unsubscription correlate with spend?
4. Is the relationship between daily session count and daily revenue strong enough to be used as a leading indicator?
5. Does the share of organic traffic differ between Europe and the Americas?

## Dataset

- Source: BigQuery training dataset `data-analytics-mate.DA` (tables: `session`, `session_params`, `order`, `product`, `account`, `account_session`).
- Shape after the SQL join: ~349,545 session-level rows, 18 columns.
- Time range: 2020-11-01 to 2021-01-31 (88 distinct sale-active days).
- Granularity: one row per session, with optional product, order and account attributes joined in.
- Key fields used: `order_date`, `ga_session_id`, `continent`, `country`, `device`, `channel`, `traffic_source`, `category`, `price`, `registered_user_id`, `email_confirmed`, `unsubscribed`.

The notebook loads this data directly from BigQuery via `pandas_gbq`, so the raw CSV is not stored in the repo.

## Workflow

1. Pull a flat, analysis-ready table from BigQuery with a single SQL join across six tables.
2. Inspect schema, null share and column types; identify columns with more than 30% missing values.
3. Restrict revenue analyses to rows where `price` is not null (i.e. actual order lines).
4. Compute revenue and session breakdowns by continent, country, category, device, channel and traffic source.
5. Build daily time series for total sales and total sessions; analyse correlation and significance.
6. Run group comparisons with non-parametric tests (Mann-Whitney U) where normality cannot be assumed; use chi-square and a two-proportion z-test for the organic-share comparison.
7. Export aggregated data and publish a Tableau Public dashboard for the executive view.

## Tools

- SQL (BigQuery dialect) for the source query.
- Python: `pandas`, `numpy`, `matplotlib`, `seaborn`.
- Statistical testing: `scipy.stats` (Mann-Whitney U, Shapiro-Wilk, Pearson, Spearman, Kruskal-Wallis, chi-square) and `statsmodels` (`proportions_ztest`).
- BigQuery access via `pandas-gbq`.
- Tableau Public for the final dashboard.

## Selected Findings

All numbers below are taken directly from the notebook outputs on the dataset described above; no figures are inferred or extrapolated.

- **Revenue is heavily concentrated in the Americas.** The Americas account for ~17.7M in sales, Asia ~7.6M, Europe ~5.9M over the 3-month window. The United States alone contributes ~13.9M, roughly 5x the next country (India, ~2.8M).
- **Desktop still dominates revenue.** Desktop sessions drive 59.0% of revenue, mobile 38.7%, tablet 2.3%, despite mobile usually being assumed dominant in modern e-commerce traffic.
- **Organic Search is the largest revenue channel.** Organic Search delivers 35.8% of revenue, Paid Search 26.6%, Direct 23.4%, Social Search 7.9%.
- **Email funnel is leaky but not catastrophic.** Of registered users, 71.7% have verified their email and 16.9% have unsubscribed from marketing communications.
- **Unsubscribing is not the revenue red flag it looks like.** Median per-user spend is marginally higher for unsubscribed users (450) than for still-subscribed users (395), but a Mann-Whitney U test returns p = 0.168 — not statistically significant on this sample.
- **Daily sessions and daily revenue move together.** Pearson and Spearman correlations between daily sessions and daily revenue are positive and statistically significant, supporting the use of session volume as a leading indicator for revenue.
- **Top-5 countries by registered users mirror revenue distribution.** United States (12,384), India (2,687), Canada (2,067), United Kingdom (859), France (553).

The notebook contains the complete output for each test, including cases where the result is "no significant difference" — these are kept rather than filtered out, because a negative result is still a result for stakeholders.

## Dashboard

Tableau Public dashboard: [View on Tableau Public](https://public.tableau.com/shared/4435X9XPB?:display_count=n&:origin=viz_share_link)

## Limitations

- The dataset is a training dataset, not real production data — magnitudes should not be interpreted as a real business, only the relative patterns and the workflow.
- The observation window is only ~3 months (Nov 2020 – Jan 2021), which overlaps with Black Friday and the winter holiday peak. Seasonal patterns are therefore only partially observable.
- About one-third of rows have `language` missing and a large share have no `registered_user_id`, which is normal for session-level data but constrains user-level analyses.
- Some `channel` and `traffic_source` values are `Undefined`, `(none)`, `<Other>` or `(data deleted)`; these are kept as-is rather than re-mapped, to avoid implicit assumptions about source attribution.
- The notebook depends on Google BigQuery credentials; reviewers without access should read the saved outputs in-place rather than re-executing.

## How to Run

1. Clone the repository and open the notebook in Jupyter or Google Colab.
2. Install dependencies: `pip install -r requirements.txt`.
3. Authenticate with Google Cloud (`gcloud auth application-default login`) and ensure the BigQuery dataset is accessible.
4. Run the notebook top to bottom. Cells are ordered so each section can be read independently.

To review without running: open `notebooks/ecommerce_user_behaviour_analysis.ipynb` directly — all outputs are committed.

## Repository Structure

```
.
├── README.md
├── PROJECT_SUMMARY.md
├── requirements.txt
├── .gitignore
└── notebooks/
    └── ecommerce_user_behaviour_analysis.ipynb
```

## Portfolio Positioning

This project demonstrates, in one place, the full analyst workflow that an entry- to mid-level Data Analyst role typically requires: SQL on a multi-table source, Python-based cleaning and exploration, applied statistics with appropriate test selection (parametric vs non-parametric), and a published dashboard. The narrative focus is on framing business questions and reporting honestly — including non-significant results — rather than maximising the count of charts.
