# Real Estate Intelligence in Ho Chi Minh City

**Paper:** A Dual-Task Framework for Real Estate Intelligence: Market Forecasting and Property Valuation in Ho Chi Minh City

---

## Overview

This repository is a minimal, reviewer-facing artifact package for the above paper. It contains the final paper PDF, figures, result tables, and reproduction scripts. It does **not** contain raw private transaction data.

The paper addresses two tasks:

- **Task 1 — Market Forecasting:** Predicts the monthly average price per m² as a ward-level time series using MA, ARIMA, Prophet, and LSTM. Primary evaluation: 37-month training / 10-month holdout.
- **Task 2 — Property-Level Valuation:** Predicts individual property price per m² using gradient boosting (XGBoost, LightGBM, CatBoost) benchmarked against an interpretable OLS hedonic model (H3, HC3-robust errors). Primary evaluation: **P3 temporal split** (train: before 2025-01-01; test: 2025 transactions, N = 1,658).

---

## Repository Contents

```
paper_assets/
  PAPER_final.pdf                        ← Final submitted paper

results/
  figures/
    figure1_framework.png                ← Dual-task framework overview
    figure2_price_timeseries.png         ← Task 1 monthly price series
    figure3_actual_vs_predicted.png      ← Task 1 forecast vs actual
    fig_road_network_accessibility.png   ← Task 2 spatial accessibility map
  tables/
    task2_p3_benchmark.csv              ← Task 2 P3 results (all models)
    bootstrap_xgb_vs_h3.json           ← Paired bootstrap: XGBoost vs H3
    h3_coefficients.csv                ← H3 hedonic model coefficients (HC3)
    table2_model_performance.csv       ← Task 1 model performance
    table4_comparison_with_simple_forecasting_baselines.csv

scripts/
  run_task2_p3_benchmark.py   ← Reproduce Task 2 P3 benchmark (all ML models)
  run_hedonic_h3.py           ← Reproduce H3 OLS hedonic model on P3 split
  run_bootstrap.py            ← Reproduce XGBoost vs H3 paired bootstrap

data/
  public/                     ← Sanitized monthly aggregate data (safe to release)
    dataset_a_monthly_public.csv
    dataset_b_monthly_public.csv
    dataset_c_macro_public.csv
    unified_monthly_panel_public.csv
  README.md                   ← Data availability and withholding policy

requirements.txt              ← Python dependencies
LICENSE                       ← MIT license
```

---

## What Is Excluded

| Item | Reason |
|---|---|
| Raw transaction-level Dataset A (`task2_modeling_trimmed.parquet`) | Confidential — non-public source records |
| Raw Dataset B registration records | Non-public administrative data |
| Trained model binaries | Not needed; models fully reproducible from scripts |
| Virtual environments | Standard exclusion |
| Jupyter notebooks, drafts, logs | Not reviewer-relevant |
| Shapefile GIS data | Large; publicly available from GADM / OpenStreetMap |

---

## Finding the Paper PDF

```
paper_assets/PAPER_final.pdf
```

---

## Primary Benchmark (Task 2)

**P3 temporal split** is the primary evaluation protocol for Task 2:

| Model | MAPE | MAE (M VND/m²) | R² |
|---|---|---|---|
| XGBoost | **16.55%** | 18.07 | 0.640 |
| H3 (OLS, HC3) | 18.27% | 19.94 | 0.522 |

Paired bootstrap (B = 5,000, seed 42):
- Observed MAPE gap (H3 − XGBoost): **1.75 pp**
- 95% CI: **[1.10, 2.45] pp**
- One-sided p-value: < 0.001

Full bootstrap output: `results/tables/bootstrap_xgb_vs_h3.json`

---

## Reproducibility

**Task 2 (requires withheld dataset):**
```bash
# Step 1 – reproduce P3 benchmark table
python scripts/run_task2_p3_benchmark.py

# Step 2 – reproduce H3 hedonic model
python scripts/run_hedonic_h3.py

# Step 3 – reproduce paired bootstrap
python scripts/run_bootstrap.py
```
All three scripts require `task2_modeling_trimmed.parquet` (withheld — see `data/README.md`).

**Task 1 (public data included):**
Source modules are in `src/` (not included in this minimal package). The public monthly panels in `data/public/` are sufficient to verify the time-series inputs.

**Environment:**
```bash
pip install -r requirements.txt
```

---

## Raw Data Notice

Raw transaction-level data (Dataset A) is **not publicly released** due to source confidentiality. Only monthly aggregate statistics are provided in `data/public/`. See `data/README.md` for full disclosure and contact information for reviewer data access.
