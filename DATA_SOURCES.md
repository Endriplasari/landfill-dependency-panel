# Data sources and variable dictionary

Harmonized country–year panel: 9 economies × 2015–2023 (N = 80; Latvia missing 2023).

## Variables (`data/processed/panel_landfill_2015_2023.csv`)

| Column (original) | Analysis name | Description | Source |
|---|---|---|---|
| `Shteti` | `country` | Country name | — |
| `Viti` | `year` | Year (2015–2023) | — |
| `norma_e_depozitimit_ne_landfill` | `landfill_rate` | Municipal waste landfilled / generated, scaled to [0, 1] (dependent variable) | Eurostat `env_wasmun` |
| `norma_e_trajtimit_jo_ne_landfill` | `nonlandfill_rate` | Non-landfill treatment share (descriptive only) | Eurostat `env_wasmun` |
| `mbetje_per_fryme` | `waste_per_capita` | Municipal waste generated, kg/inhabitant | Eurostat `env_wasmun` |
| `intensiteti_trajtimit` | `treatment_intensity` | Treated/generated ratio — **kept for transparency, excluded from all models** (under `env_wasmun`, "treatment" includes landfilling, so the ratio is mechanically tied to the outcome) | Eurostat `env_wasmun` |
| `PBB_per_fryme` | `gdp_per_capita` | GDP per capita, index (EU = 100) | Eurostat `nama_10_pc` |
| `popullesia_urbane_perqindja_mbi_popullesine_totale` | `urban_pop` | Urban population, % of total | World Bank WDI |
| `papunesia_perqindja_totale_fuqise_punetore` | `unemployment` | Unemployment, % of labour force | Eurostat `une_rt_a` |
| `perdorimi_internetit_nga_individe` | `internet` | Individuals using the internet, % | ITU / World Bank |
| `cluster` | — | Legacy outcome-based clustering label from an earlier exploratory phase — **not used**; retained only to compute the Adjusted Rand Index against the predictor-side typology | — |

## Sample notes

- **Unbalanced panel:** Latvia is missing 2023 (8 observations), so N = 80 rather than 81.
- **Interpolations (2 values):** Montenegro 2017; Serbia 2020 (linear). Robustness checks excluding these observations are in the notebook.
- **Exclusions:** North Macedonia and Bosnia & Herzegovina were excluded for extensive reporting gaps (symmetric rule).
- Countries were selected for consistent series availability, coverage of the full range of circular performance, and structural heterogeneity in income and digitalization.

## Reproducibility

All stochastic steps use seed 42: K-means (`n_init = 25`), wild cluster bootstrap (Webb weights, B = 1999).
