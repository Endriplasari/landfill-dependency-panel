# Data Sources

This document lists the sources, codes, and access details for every indicator used in the
analysis.

## Coverage

- **Countries (panel):** Albania, Bulgaria, Estonia, Latvia, Lithuania, Montenegro, Romania, Serbia, Slovenia (N = 9).
- **Period (panel):** 2015–2023 (balanced; N = 80 country–year observations).
- **Municipality (forecasting):** Tirana, monthly, 2017–2024.

Countries with extensive reporting gaps that would compromise panel comparability
(e.g., North Macedonia, Bosnia and Herzegovina) were excluded. A small number of isolated
missing values were addressed by linear interpolation.

---

## 1. Country–year panel (`data/processed/panel_landfill_2015_2023.csv`)

| Variable (processed) | Source | Dataset / code | Notes |
|---|---|---|---|
| `landfill_rate` (0–1) | Eurostat | `env_wasmun` (municipal waste by treatment) | Share of municipal waste landfilled; rescaled to [0, 1] |
| `nonlandfill_rate` | Eurostat | `env_wasmun` | Complement of the landfill share |
| `waste_per_capita` (kg/inh.) | Eurostat | `env_wasmun` | Municipal waste generated per capita |
| `treatment_intensity` | Eurostat | `env_wasmun` | Treated share of generated waste (derived) |
| `gdp_per_capita` (index) | Eurostat | `nama_10_pc` | Indexed, EU = 100 |
| `unemployment` (%) | Eurostat | `une_rt_a` | Total unemployment, % of labour force |
| `urban_pop` (%) | World Bank | WDI `SP.URB.TOTL.IN.ZS` | Urban population, % of total |
| `internet` (%) | ITU / World Bank | WDI `IT.NET.USER.ZS` | Individuals using the internet, % |
| `cluster` | Derived | K-means (k = 3) | CE–BDA regime id, computed on standardized indicators |

**Links**
- Eurostat data browser: https://ec.europa.eu/eurostat/data/database
  - `env_wasmun` — municipal waste by waste-management operations
  - `nama_10_pc` — main GDP aggregates per capita
  - `une_rt_a` — unemployment by sex and age, annual
- World Bank WDI: https://databank.worldbank.org/source/world-development-indicators
- ITU statistics: https://www.itu.int/en/ITU-D/Statistics/

`[download date: 10/01/2026 ]`

### How the `cluster` id was produced
K-means (`n_clusters=3`, `random_state=42`) on the z-scored vector of: landfill rate,
non-landfill treatment, treatment intensity, waste per capita, GDP per capita, unemployment,
urban population, internet use. The number of clusters was selected via the elbow (inertia)
and silhouette criteria; stability across scaling strategies (Standard, Standard+clip, Robust)
is confirmed by the Adjusted Rand Index (0.956–1.000) in the notebook (Section A.6).

---

## 2. Monthly series — Tirana (`data/raw/tirana_landfill_monthly_2017_2024.csv`)

| Field | Description |
|---|---|
| Monthly landfilled tonnage | Total tonnes of municipal waste deposited in landfill per month, Municipality of Tirana |
| Coverage | January 2017 – December 2024 |

- **Source:** Municipality of Tirana (administrative records). `[source link / reference: https://ckan.tirana.al/dataset/drejtoria-e-pergjithshme-e-pastrimit-dhe-gjelberimit/resource/86159672-64f6-463b-ba8c-2ccbd1cf39c6 \]`
- **Missing values:** isolated gaps (notably parts of 2023) were linearly interpolated; this is
  documented in the notebook and affects the Seasonal-Naive benchmark.

> If the monthly administrative data are not openly licensed for redistribution, replace the raw
> file with an aggregated/anonymized version or provide access instructions here instead.

`[download date: 19/02/2026 ]`

---

## Licensing of sources

- **Eurostat** — free reuse with attribution (© European Union). https://ec.europa.eu/eurostat/about-us/policies/copyright
- **World Bank Open Data** — CC BY 4.0. https://datacatalog.worldbank.org/public-licenses
- **ITU** — consult the ITU terms of use before redistributing the raw series; processed/aggregated values are shared here for reproducibility.
- **Municipality of Tirana** — administrative data; verify redistribution permission.

Processed datasets in this repository are released under **CC BY 4.0** (`LICENSE-DATA`); analysis
code is released under the **MIT License** (`LICENSE`).
