# Customer Usage Analysis and Segmentation — ConnectaTel (Python)

Data cleaning, missing-data diagnosis, outlier analysis and customer segmentation for a Latin American telecom company, using 2024 data on 4,000 customers and 40,000 calls and texts.

## Business Questions

- Which customer segments use calls and texts more or less?
- Which customers show extreme usage that could signal unusual behaviour, fraud or capture errors?
- How does usage vary by age and plan?
- What should the company change in its plans and retention strategy?

## Dataset

Three tables, joined into one profile per customer (`user_profile`):

- `plans.csv`: the two plans (Basico and Premium) with prices and included minutes, texts and GB
- `users_latam.csv`: 4,000 customers with age, city, registration date, plan and churn date
- `usage.csv`: 40,000 events (calls and texts) with date, call duration and text length

> The datasets belong to TripleTen and are not redistributed in this repository.

## Key Findings

### Usage level is the clearest churn signal

- **High-usage customers (7% of the base) have the lowest share of active customers:** 86.0% vs 88.5% in the low and medium segments.
- They are not more likely to be on Premium (33% vs about 35% in the other segments), so the current plan structure doesn't capture their value.

### Age does not predict usage

- Adult 50.4%, Senior 30.6%, Young 19.0%.
- The three age groups have almost identical low / medium / high usage mixes, so age is not a useful basis for targeting offers.

### Extreme users are real, not errors

IQR analysis found outliers in all three usage metrics. All were kept, because the values are realistic and removing them would delete the heavy-usage segment the business is asking about.

- Texts: 46 customers (1.15%) with 12 to 17 texts
- Calls: 30 customers (0.75%) with 11 to 15 calls
- **Call minutes: 109 customers (2.73%) with 62 to 160 minutes.** They are 87.2% active and 39.4% Premium (vs 35.1% overall): heavy users who already lean toward the higher plan.

### The more minutes a customer uses, the higher the share of Premium

Call minutes are the only usage metric with a clear plan pattern. Calls are spread proportionally across both plans.

## Recommendations

1. **Retention program for the high-usage segment:** priority support or targeted offers before they churn. Losing them costs more than their 7% share suggests.
2. **An unlimited or business plan** for the ~109 heavy call-minute users, whose consumption is far above the rest.
3. **Segment offers by usage behaviour, not age.**

> Caveat: the retention gap is about 2.5 points on a 278-customer segment. It should be validated with more data or a significance test before committing budget.

## Data Quality: Decisions and Reasoning

| Issue | Finding | Decision | Why |
|---|---|---|---|
| `duration` / `length` missing | 55.2% / 44.7% | Left as missing | **MAR:** every missing duration is a text and every missing length is a call. Each column is empty exactly where it doesn't apply. Imputing would create fake call durations for texts. |
| `age` sentinel | Minimum of -999 | Replaced with the median (47) | An impossible value; the median isn't distorted by it the way the mean is |
| `city` sentinel | 469 customers (11.7%) with "?" | Converted to missing | City is a segmentation dimension; filling it with another city would invent data |
| `reg_date` future dates | 40 customers (1.0%) dated 2026 | Set to missing | The data ends in 2024; keeping the row preserves the rest of the customer's data |
| `churn_date` missing | 88.4% | Kept | Expected: an empty churn date means the customer is still active |
| Dates stored as text | `reg_date`, `churn_date`, `date` | Converted to datetime | Needed for any time comparison |

## Approach

1. **Load and explore** the three tables: shape, types, non-null counts.
2. **Data quality checks:** missing values, sentinels, duplicates, impossible dates.
3. **Cleaning:** fixed sentinels and impossible dates; diagnosed missing values with a MAR test (`groupby('type')` on the null flags).
4. **Usage metrics per customer:** texts, calls and call minutes aggregated by `user_id` and joined with customer data.
5. **Distributions and outliers:** histograms by plan, boxplots, and IQR limits with the percentage of outliers per metric.
6. **Segmentation:** usage groups (low / medium / high) and age groups (young / adult / senior), then cross-tabs against plan and churn.
7. **Executive summary** with findings and recommendations.

## Notebook

[`connectatel_customer_analysis.ipynb`](connectatel_customer_analysis.ipynb): the full analysis, with commentary in English. Variable names and category labels are kept in Spanish, as in the source data (for example, `cant_mensajes` = number of texts, `grupo_uso` = usage group).

## How to Run

1. Open the notebook in Google Colab or Jupyter.
2. Place `plans.csv`, `users_latam.csv` and `usage.csv` in a `/datasets/` folder, or update the paths in `pd.read_csv(...)`.
3. Run all cells in order.

```bash
pip install pandas seaborn matplotlib jupyter
jupyter notebook connectatel_customer_analysis.ipynb
```

## Tools

Python · pandas · seaborn · matplotlib · Jupyter

## Author

**Sebastian Ladino Novoa** — Data Analytics Portfolio
