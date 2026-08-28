# Structural Determinants of Landfill Dependency in Emerging and Transition European Economies

Replication package for:

> Myftaraj (Tomori), E., Fata, I., & Plasari, E. — *Structural Determinants of Landfill Dependency in Emerging and Transition European Economies: A Fixed-Effects Panel Analysis.* Manuscript under review at **Economies** (MDPI), ID `economies-4451193`.

Country–year panel of **9 emerging and transition European economies** (Albania, Bulgaria, Estonia, Latvia, Lithuania, Montenegro, Romania, Serbia, Slovenia), **2015–2023** (N = 80). The dependent variable is the **landfill rate** (share of municipal waste landfilled, scaled to [0, 1]).

## Headline results

- Landfill dependency is overwhelmingly a **between-country, structural** phenomenon (between-country share of variance ≈ 0.93), associated with the level of economic development.
- **Within countries** over 2015–2023 there is **no robust association** with waste generation, income, urbanization, or digitalization under wild-bootstrap inference (a suggestive negative link with unemployment is the exception, p ≈ 0.06).
- Internet penetration shows **no detectable direct within-country effect** (equivalence/TOST test); digitalization reads as a marker of structural development, not a direct lever.
- No evidence of an Environmental Kuznets Curve (quadratic GDP insignificant; turning point outside the relevant range).

## Repository contents

| Path | Description |
|---|---|
| `FINAL_Panel_Analysis.ipynb` | Complete analysis notebook (data prep → descriptive typology → FE/CRE panel models → wild cluster bootstrap → diagnostics → robustness → figures → export). |
| `data/processed/panel_landfill_2015_2023.csv` | Harmonized panel dataset (variable dictionary in `DATA_SOURCES.md`). |
| `outputs/` | Figures (workflow + F1–F4) and all result tables (`all_tables.xlsx`, one sheet per table). |
| `DATA_SOURCES.md` | Variable dictionary, sources, and sample notes. |
| `CITATION.cff` | Citation metadata. |

## How to reproduce

**Google Colab (recommended):**
1. Open `FINAL_Panel_Analysis.ipynb` in Colab.
2. **Runtime → Run all.** When prompted, upload `data/processed/panel_landfill_2015_2023.csv`, or set `BASE_URL` in the setup cell to
   `https://raw.githubusercontent.com/Endriplasari/landfill-dependency-panel/main/data/processed/` to load it directly.
3. All tables are written to `outputs/` and bundled automatically (`all_tables.xlsx` + `landfill_outputs.zip`).

**Locally:** `pip install -r requirements.txt`, then run the notebook with Jupyter. Python ≥ 3.10.

All stochastic steps use a fixed seed (**42**): K-means (`n_init = 25`), wild cluster bootstrap (**Webb weights, B = 1999**).

## Methods (summary)

Pooled OLS (benchmark) → country fixed effects → two-way FE → random effects → **correlated random effects (Mundlak)** decomposing within- vs. between-country variation; Hausman and Mundlak tests; **wild cluster bootstrap (Webb)** for inference with G = 9 clusters; Pesaran CD; equivalence (TOST) test for the digitalization coefficient; fractional-response (logit) robustness; leave-one-country-out and exclude-interpolated checks. A descriptive typology (K-means on predictor-side variables only) characterizes structural heterogeneity and is **not** used in the regressions. Cross-validated against independent Stata implementations (`xtreg`, `boottest`, CRE).

## Data sources

Eurostat (`env_wasmun`, `nama_10_pc`, `une_rt_a`), World Bank WDI (urban population), ITU/World Bank (internet use). See `DATA_SOURCES.md` for details, the unbalanced-panel note (Latvia missing 2023), and the two interpolated observations (Montenegro 2017; Serbia 2020).

## License

Code: MIT. Data compilation: CC BY 4.0 (underlying indicators © their respective providers).

## Citation

See `CITATION.cff`, or cite the manuscript above.
