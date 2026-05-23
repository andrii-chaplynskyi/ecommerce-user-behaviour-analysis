# Project Summary

**E-commerce Sales and User Behaviour Analysis** — a full analyst workflow on session-level e-commerce data: SQL on BigQuery, Python cleaning and EDA, applied statistics, and a published Tableau dashboard.

## At a Glance

- **Data**: ~349,545 session-level rows joined across 6 BigQuery tables; 2020-11-01 to 2021-01-31.
- **Tools**: SQL (BigQuery), Python (pandas, numpy, matplotlib, seaborn), scipy / statsmodels, Tableau Public.
- **Tests applied**: Mann-Whitney U, Shapiro-Wilk, Pearson, Spearman, Kruskal-Wallis, chi-square, two-proportion z-test.
- **Deliverables**: analysis notebook with inline outputs and a public Tableau dashboard.

## Headline Findings

- Americas drive ~17.7M of sales, ~2.3x Asia and ~3x Europe over the 3-month window; the United States alone is ~13.9M.
- Desktop generates 59.0% of revenue, mobile 38.7%, tablet 2.3%.
- Organic Search is the largest revenue channel (35.8%), ahead of Paid Search (26.6%) and Direct (23.4%).
- 71.7% of registered users verified email; 16.9% unsubscribed. Spend difference between subscribed and unsubscribed users is not statistically significant (Mann-Whitney p = 0.168).
- Daily sessions and daily revenue are positively and significantly correlated, supporting session volume as a daily leading indicator.

## What This Demonstrates

- Writing a multi-table SQL join against a real-world style schema.
- Choosing the right statistical test (parametric vs non-parametric) and reporting both significant and non-significant results.
- Translating output into business-readable findings without overclaiming.
- Closing the loop with a stakeholder-facing Tableau dashboard.

## Links

- Notebook: `notebooks/ecommerce_user_behaviour_analysis.ipynb`
- Tableau Public dashboard: https://public.tableau.com/shared/4435X9XPB?:display_count=n&:origin=viz_share_link
- Full write-up: `README.md`

## Author

Andrii Chaplynskyi — Data Analyst (career-changer), Hong Kong. Background in investigative and statistical reporting.
