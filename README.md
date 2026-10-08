# A Quantum-Enhanced Feature-Selection Framework for Solar Photovoltaic Generation Forecasting

**Repository:** [QEFSF-for-Solar-Photovoltaic-Generation-Forecasting](https://github.com/SunawarKhan/QEFSF-for-Solar-Photovoltaic-Generation-Forecasting/tree/main)
· `https://github.com/SunawarKhan/QEFSF-for-Solar-Photovoltaic-Generation-Forecasting.git`

Reproducibility package for the manuscript *"A Quantum-Enhanced Feature-Selection
Framework for Solar Photovoltaic Generation Forecasting"* (Ms. No. **RINENG-D-26-19456**,
*Results in Engineering*).

The work formulates wrapper feature selection for PV generation as a
**relevance–redundancy QUBO** with a cardinality constraint, maps it to an Ising
Hamiltonian, and solves the **identical** objective with six optimizers spanning three
paradigms — classical (GA, PSO), quantum-inspired (QPSO, QIEA, QGA), and gate-based
quantum (QAOA, simulated in PennyLane) — benchmarked against the exact brute-force
optimum. Selected subsets are validated on a HistGradientBoosting forecaster and a
four-qubit Variational Quantum Classifier (VQC).

## Author

**Sunawar Khan** — author of this work and maintainer of this repository
([@SunawarKhan](https://github.com/SunawarKhan)).

## Repository layout

```
QEFSF-for-Solar-Photovoltaic-Generation-Forecasting/
├── README.md                     this file
├── CHANGES_IN_REVISION.md        reviewer-by-reviewer map of every revision change
├── requirements.txt              Python dependencies
├── manuscript/
│   ├── Revised_Manuscript_highlighted.docx   revised paper; every change highlighted
│   │                                         and tagged [Reviewer n, Comment n]
│   └── Response_Letter_Filled.docx           point-by-point response (Author Response
│                                             + Author Action) to all 19 comments
├── notebook/
│   └── notebook.ipynb            end-to-end pipeline; regenerates every reported number
│                                 (Section 13 holds the new revision analyses)
├── data/
│   └── solar_weather.csv         196,776 x 17, 15-min records, 2017-01-01 to 2022-08-31
├── results/
│   └── results.json              all metrics, including `revision_analyses`
└── figures/
    ├── fig1_pipeline_architecture.png
    ├── fig2_eda_distribution_correlation.png
    ├── fig3_optimizer_convergence.png
    ├── fig4_multicriteria_radar.png
    ├── fig5_forecast_and_importance.png
    ├── fig6_vqc_loss_confusion.png
    └── fig7_ablation_summary.png
```

## Dataset

A public residential-PV record (Kaggle *Solar Energy Power Generation Dataset*) pairing
a rooftop system's energy output (`Energy delta[Wh]`, the target) with co-located
**OpenWeatherMap** meteorology (temperature, pressure, humidity, wind speed, hourly rain
and snow, cloud cover, categorical weather type) plus engineered solar-geometry features
(`isSun`, `sunlightTime`, `dayLength`, `SunlightTime/daylength`). 196,776 fifteen-minute
rows, 2017-01-01 → 2022-08-31, no missing values or duplicate timestamps.
Target: mean 573.0 Wh, std 1044.8 Wh, max 5020 Wh; 51.3% night-time zeros, 52.0% daylight,
daytime median 524 Wh.

## Reproducing the results

```bash
git clone https://github.com/SunawarKhan/QEFSF-for-Solar-Photovoltaic-Generation-Forecasting.git
cd QEFSF-for-Solar-Photovoltaic-Generation-Forecasting
pip install -r requirements.txt
cd notebook
jupyter notebook notebook.ipynb     # Run All
```

All randomness is seeded (`SEED = 42`); the heavy quantum steps are state-vector
simulations (QAOA ≈ 48 s, VQC ≈ 113 s). Running the notebook rewrites
`results/results.json`, including the `revision_analyses` block.

## Headline results

| Item | Value |
|---|---|
| Exact QUBO optimum | E\* = −7.6314, subset {GHI, temp, humidity, cloud cover, daylight} |
| QAOA approximation ratio | 1.000 (shift-invariant η_s = 1.000) |
| Hit-rate (10 seeds) | GA / PSO / QGA 10/10; QIEA / QPSO 9/10; QAOA 1/5 restarts |
| Forecasting (5-feature subset) | RMSE 405.7 Wh, R² 0.850 (94% of the 10-feature R² 0.905) |
| VQC (4 qubits, 3 layers) | accuracy 0.875, F1 0.872, AUC 0.947 |

## Revision analyses (new in this version)

Added under `results.json → revision_analyses` and in **Section 13** of the notebook:

* **Leakage check (R5.2):** recomputing the Stage-1 screen and QUBO on the **training
  partition only** changes correlations by at most **0.0087**, and yields the **identical**
  retained set and **identical** optimal subset — the pipeline is leakage-free.
* **Conventional feature selection (R5.4):** mRMR, mutual information, RFE, L1/Lasso at
  matched k = 5 (R² ≈ 0.90); the QUBO matches them at its redundancy-neutral setting
  (β = 0, R² 0.899) while giving lower-collinearity subsets at β = 1.
* **Naive baselines (R5.1):** persistence (R² 0.909), seasonal persistence (0.515),
  climatology (−0.001), contextualizing the task as contemporaneous estimation.
* **Shift-invariant approximation ratio (R5.3):** η_s = (E_max − E)/(E_max − E_min),
  offset-invariant; optimizer ranking unchanged.

See `CHANGES_IN_REVISION.md` for the full comment-by-comment map.

## How to cite

If you use this code, data pipeline, or results in your research, please cite the
manuscript and/or this repository.

**Plain text**

> Khan, S. (2026). *A Quantum-Enhanced Feature-Selection Framework for Solar Photovoltaic
> Generation Forecasting*. Results in Engineering (under review, Ms. No. RINENG-D-26-19456).
> Code: https://github.com/SunawarKhan/QEFSF-for-Solar-Photovoltaic-Generation-Forecasting

**BibTeX — manuscript**

```bibtex
@article{khan2026qefsf,
  author  = {Khan, Sunawar},
  title   = {A Quantum-Enhanced Feature-Selection Framework for Solar Photovoltaic Generation Forecasting},
  journal = {Results in Engineering},
  year    = {2026},
  note    = {Under review, Ms. No. RINENG-D-26-19456}
}
```

**BibTeX — code repository**

```bibtex
@misc{khan2026qefsf_code,
  author       = {Khan, Sunawar},
  title        = {{QEFSF-for-Solar-Photovoltaic-Generation-Forecasting}: Reproducibility package for a quantum-enhanced feature-selection framework for solar PV forecasting},
  year         = {2026},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/SunawarKhan/QEFSF-for-Solar-Photovoltaic-Generation-Forecasting}}
}
```

> Once the paper is published, please update the manuscript entry above with the final
> volume, pages and DOI.
