# Structural Determinants of Landfill Dependency in Emerging and Transition European Economies

Replication package for:

> Myftaraj (Tomori), E., Fata, I., & Plasari, E. — *Structural Determinants of Landfill Dependency in Emerging and Transition European Economies: A Fixed-Effects Panel Analysis.* Manuscript under review at **Economies** (MDPI), ID `economies-4451193`.

Country–year panel of **9 emerging and transition European economies** (Albania, Bulgaria, Estonia, Latvia, Lithuania, Montenegro, Romania, Serbia, Slovenia), **2015–2023** (N = 80). The dependent variable is the **landfill rate** (share of municipal waste landfilled, scaled to [0, 1]).

## Headline results

- Landfill dependency is overwhelmingly a **between-country, structural** phenomenon (between-country share of variance ≈ 0.93), associated with the level of economic development.
- **Within countries** over 2015–2023 there is **no robust association** with waste generation, income, urbanization, or digitalization under wild-bootstrap inference (a suggestive negative link with unemployment is the exception, p ≈ 0.06).
- Internet penetration shows **no detectable direct within-country effect**; under a TOST equivalence test with bounds set in advance at ±0.01 on the landfill rate, the effect is statistically equivalent to zero (p = 0.042).
- No evidence of an Environmental Kuznets Curve (quadratic GDP insignificant; turning point outside the relevant range).

## Repository contents

| Path | Description |
|---|---|
| `FINAL_Panel_Analysis.ipynb` | Complete analysis notebook (data prep → descriptive typology → FE/CRE panel models → wild cluster bootstrap → diagnostics → robustness → figures → export). |
| `data/processed/panel_landfill_2015_2023.csv` | Harmonized panel dataset. |
| `outputs/` | Figures (workflow + F1–F4) and all result tables (`all_tables.xlsx`, one sheet per table). |
| `DATA_SOURCES.md` | Full variable dictionary, sources, and sample notes. |
| `requirements.txt` | Python dependencies. |
| `CITATION.cff` | Citation metadata. |

## Variables at a glance

| Variable | Role | Definition | Source |
|---|---|---|---|
| `landfill_rate` | Dependent | Municipal waste landfilled / generated, scaled to [0, 1] | Eurostat `env_wasmun` |
| `waste_per_capita` | Covariate | Municipal waste generated, kg per inhabitant | Eurostat `env_wasmun` |
| `gdp_per_capita` | Covariate | GDP per capita, index (EU = 100) | Eurostat `nama_10_pc` |
| `urban_pop` | Covariate | Urban population, % of total | World Bank WDI |
| `unemployment` | Covariate | Unemployment, % of labour force | Eurostat `une_rt_a` |
| `internet` | Covariate | Individuals using the internet, % (proxy for digital development) | ITU / World Bank |
| `treatment_intensity` | **Excluded** | Treated/generated ratio — mechanically tied to the outcome, since `env_wasmun` counts landfilling among treatment operations; retained in the data for transparency only | Eurostat `env_wasmun` |

Full definitions, sample notes (unbalanced panel; interpolated observations) and the original column names are in [`DATA_SOURCES.md`](DATA_SOURCES.md).

## How to run

**Google Colab (recommended):**
1. Open `FINAL_Panel_Analysis.ipynb` in Colab.
2. **Runtime → Run all.** When prompted, upload `data/processed/panel_landfill_2015_2023.csv`, or set `BASE_URL` in the setup cell to
   `https://raw.githubusercontent.com/Endriplasari/landfill-dependency-panel/main/data/processed/` to load it directly.
3. All tables are written to `outputs/`; the final cell bundles them into `all_tables.xlsx` and `landfill_outputs.zip`.

**Locally:**
```bash
git clone https://github.com/Endriplasari/landfill-dependency-panel.git
cd landfill-dependency-panel
pip install -r requirements.txt
jupyter notebook FINAL_Panel_Analysis.ipynb
```

All stochastic steps use a fixed seed (**42**): K-means (`n_init = 25`), wild cluster bootstrap (**Webb weights, B = 1999**).

## Software versions

Results reported in the paper were produced with **Python 3.11** and:

| Package | Version |
|---|---|
| pandas | 2.2 |
| numpy | 1.26 |
| scipy | 1.11 |
| statsmodels | 0.14 |
| linearmodels | 5.4 |
| scikit-learn | 1.3 |
| matplotlib | 3.8 |
| openpyxl | 3.1 |

Panel estimates were independently cross-validated in **Stata 17** (`xtreg, fe`; `boottest` for the wild cluster bootstrap; the CRE/Mundlak specification estimated by augmenting the model with country means of the time-varying regressors).

## Methods (summary)

Pooled OLS (benchmark) → country fixed effects → two-way FE → random effects → **correlated random effects (Mundlak)** decomposing within- vs. between-country variation; Hausman and Mundlak tests; **wild cluster bootstrap (Webb)** for inference with G = 9 clusters; Pesaran CD; TOST equivalence test (bounds ±0.01) for the digitalization coefficient; fractional-response (logit) robustness; leave-one-country-out and exclude-interpolated checks. A descriptive typology (K-means on predictor-side variables only) characterizes structural heterogeneity and is **not** used in the regressions.

## Data sources

Eurostat (`env_wasmun`, `nama_10_pc`, `une_rt_a`), World Bank WDI (urban population), ITU/World Bank (internet use). See `DATA_SOURCES.md` for the unbalanced-panel note (Latvia missing 2023) and the two interpolated observations (Montenegro 2017; Serbia 2020).

## License

Code: MIT. Data compilation: CC BY 4.0 (underlying indicators © their respective providers).

## Citation

See `CITATION.cff`, or cite the manuscript above.
