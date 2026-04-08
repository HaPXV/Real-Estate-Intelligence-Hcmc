# Data — Availability and Withholding Policy

---

## What Is Included (Public)

All files under `data/public/` are monthly aggregate statistics. No row-level records, addresses, or personal identifiers are included.

### `dataset_a_monthly_public.csv`
- **Content:** Monthly aggregated real-estate transaction series (Dataset A).
- **Columns:** `year_month`, `avg_price_m2_million`, `transaction_count`
- **Safe to publish:** month-level aggregates only; no individual transaction records.
- **Scope:** 47 months used in Task 1 forecasting.

### `dataset_b_monthly_public.csv`
- **Content:** Monthly aggregated land-registration activity counts (Dataset B).
- **Columns:** `year_month`, `registration_count`
- **Safe to publish:** monthly counts only.

### `dataset_c_macro_public.csv`
- **Content:** Monthly macroeconomic indicators (Dataset C).
- **Columns:** `year_month`, `interest_rate`, `gold_price_sjc`, `vn_index`, `cpi_index`
- **Safe to publish:** public macro aggregates; no personal records.

### `unified_monthly_panel_public.csv`
- **Content:** Merged monthly panel (Dataset A + B + C) for Task 1 forecasting.
- **Safe to publish:** all columns are monthly aggregate or public macro variables.

---

## What Is Withheld (Private)

### `task2_modeling_trimmed.parquet` — NOT RELEASED

- **Content:** 5,005 individual property transaction records used for Task 2 (property-level valuation).
- **Why withheld:** Sourced from non-public data providers. Contains transaction-level price, location, and structural data that cannot be publicly released under the source data terms.
- **Features (not released):**
  - Structural: `area_m2`, `so_tang` (floors), `cap_cong_trinh` (quality grade), `frontage_m`, `is_mat_tien`
  - Location: `district`, `ward_key`
  - Temporal: `transaction_date`, `transaction_year`, `transaction_quarter`
  - Spatial: `dist_ben_thanh_km` (road-network distance to Ben Thanh Market)
  - Target: `price_m2` (VND/m²)

### Aggregate Task 2 results (INCLUDED in `results/tables/`)

To allow reviewer verification without releasing the raw dataset, the following aggregate artifacts are included:

- `results/tables/task2_p3_benchmark.csv` — MAPE, MAE, RMSE, R² for all models on the P3 test set (N = 1,658).
- `results/tables/bootstrap_xgb_vs_h3.json` — Full paired bootstrap output (B = 5,000 replicates).
- `results/tables/h3_coefficients.csv` — H3 OLS coefficients with HC3 standard errors and significance levels.

---

## No PII

No personally identifiable information is present in any publicly released file.

---

## Reviewer Data Access

Reviewers requiring the Task 2 dataset for deeper verification may contact the corresponding author. Access may be granted under a data-sharing agreement subject to source licensing terms.
