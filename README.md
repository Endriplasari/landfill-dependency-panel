# Landfill Dependency, Circular-Economy Regimes, and Big Data Analytics

Reproducible data and code for the study:

> **Structural Determinants of Landfill Dependency in Emerging and Transition European Economies: A Panel Analysis of Circular-Economy Regimes and the Mediating Role of Digitalization**
> Elena Myftaraj (Tomori), Irena Fata, Endri Plasari.

The analysis combines an unsupervised classification of circular-economy / Big-Data-Analytics (CE–BDA) regimes with panel OLS regressions (country-clustered standard errors) for nine emerging and transition European economies (2015–2023), and a complementary machine-learning forecast of landfilled waste for the Municipality of Tirana.

---

## Repository structure

```
.
├── CE_BDA_Analysis_OLS_and_ML.ipynb   # main reproducible notebook (OLS + ML)
├── requirements.txt
├── LICENSE                            # MIT (code)
├── LICENSE-DATA                       # CC BY 4.0 (processed data)
├── CITATION.cff
├── data/
│   ├── processed/
│   │   └── panel_landfill_2015_2023.csv          # country–year panel (incl. cluster id)
│   ├── raw/
│   │   └── tirana_landfill_monthly_2017_2024.csv # monthly landfilled tonnage, Tirana
│   └── DATA_SOURCES.md                # exact source codes, links, download dates
├── results/
│   ├── tables/                        # Table02–Table14 exported as CSV
│   └── figures/
└── README.md
```

## Data

Two input files are required by the notebook:

| File | Level | Period | Contents |
|---|---|---|---|
| `data/processed/panel_landfill_2015_2023.csv` | Country–year | 2015–2023 (N = 80) | Landfill rate, waste per capita, treatment intensity, GDP per capita, urban population, unemployment, internet use, and the pre-computed CE–BDA `cluster` id |
| `data/raw/tirana_landfill_monthly_2017_2024.csv` | Monthly | 2017–2024 | Landfilled tonnage (Municipality of Tirana) |

Raw indicators derive from **Eurostat**, the **World Bank (WDI)** and the **ITU**; the monthly series comes from the **Municipality of Tirana**. Exact source codes, links and download dates are documented in [`data/DATA_SOURCES.md`](data/DATA_SOURCES.md).

> **Note on regimes.** The `cluster` column in the panel file was produced by K-means (k = 3) on the standardized indicator vector in a data-preparation step. Its stability across scaling strategies is verified in the notebook (Adjusted Rand Index). The stored `cluster` id is used so that the regime dummies match the published tables; re-deriving clusters in-notebook can permute cluster labels.

## How to run

### Option A — Google Colab (recommended)
1. Open `CE_BDA_Analysis_OLS_and_ML.ipynb` in Colab.
2. In the **Config** cell, either set `BASE_URL` to this repository's raw data path, e.g.
   ```python
   BASE_URL = "https://raw.githubusercontent.com/Endriplasari/landfill-dependency-panel/main/data/processed/"
   ```
   or leave `BASE_URL = ""` and upload the two CSV files when prompted.
3. **Runtime → Run all.**

### Option B — Local
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook CE_BDA_Analysis_OLS_and_ML.ipynb
```
Place the two CSV files next to the notebook (or set `BASE_URL`).

All result tables are written to `results/tables/` (as `outputs/` inside the notebook) as CSV.

## Reproducing the manuscript tables

| Manuscript | Notebook section |
|---|---|
| Table 2 — OLS diagnostics | A.8 |
| Table 3 — Descriptive statistics | A.2 |
| Tables 4–5 — Regime classification & profiles | A.3 |
| Table 6 — Welch ANOVA | A.4 |
| Table 7 — Games–Howell | A.4 |
| Table 8 — Landfill trend by regime | A.5 |
| Table 9 — ARI stability | A.6 |
| Table 10 — Panel OLS (M_VAR / M_KL / M1 / M2) | A.7 |
| Table 11 — VIF | A.9 |
| Table 12 — Albania vs. typologies | A.10 |
| Table 13 — Forecast accuracy | B.4 |
| Table 14 — Policy scenarios | B.6 |

## Reproducibility

- All stochastic steps use `RANDOM_STATE = 42` (K-means, XGBoost).
- XGBoost feature engineering is leakage-safe: moving averages at month *t* use months *t−2 … t−(k+1)*, identically in training and recursive inference.
- Pin exact package versions with `requirements.txt` (see below).

## Citation

If you use this repository, please cite the manuscript and the software (see `CITATION.cff`). For a permanent DOI, archive a release via [Zenodo](https://zenodo.org/).

## License

- **Code:** MIT (see `LICENSE`).
- **Processed data:** CC BY 4.0 (see `LICENSE-DATA`), consistent with the Eurostat and World Bank open-data terms. ITU-derived values are redistributed only in aggregated/processed form; consult the ITU terms for the raw series.
