# panic-hyperuricemia-prediction

Python code for the analyses reported in:

> Hayashi R, Hayashi S, Sueda K, Sanayama H, Watanabe T, Fukuchi T, Omoto K, Sugawara H. Risk prediction of 72-hour fatality in panic-level hyperuricemia using machine learning and logistic regression. *PLOS ONE* (revised manuscript under review; PONE-D-26-36706).

The study developed prediction models for 72-hour fatality in adult patients with panic-level hyperuricemia (serum uric acid ≥10 mg/dL) using routine laboratory data. The development cohort comprised patients assessed in 2019 (n = 756; 48 fatal events), and the external validation cohort comprised patients assessed in the non-overlapping preceding period of 2017–2018 (n = 1,243; 54 fatal events). Ten models were compared: multivariable logistic regression analysis (MLRA), eight gradient boosting algorithms, and Prediction One.

**Version:** v3.0.1 (R1 revision). This version contains the code used for the revised manuscript. Figure and table numbers below follow the revised manuscript.

## Notebooks

| Notebook | Analyses | Outputs in the revised manuscript |
| --- | --- | --- |
| `01_sample_size.ipynb` | Minimum sample size for the final MLRA model (Riley et al., 2019) using the R package pmsampsize via rpy2; calculation for 43 candidate predictor parameters | S1 Table, S1 Fig |
| `02_uric_acid_epidemiology.ipynb` | 72-hour fatality by serum uric acid stratum, Cochran–Armitage trend test, restricted cubic spline analysis, cutoff analyses (≥11.0 and ≥12.0 mg/dL), low-event stratum exclusion analysis, age-stratified analyses | Fig 2, S4 Fig, S5 Fig, S5 Table, S6 Table (Table 2 reports values from S5 Table) |
| `03_prediction_models.ipynb` | MLRA and eight gradient boosting models (XGBoost Standard and DART; LightGBM GBDT, DART, and GOSS; CatBoost Standard and Ordered; HistGradientBoosting); discrimination, calibration, bootstrap internal validation, IDI/NRI, decision curve analysis, subgroup, threshold variation, outlier influence, and fairness analyses; missing data description and imputation sensitivity analysis; SHAP analysis | Table 4; Figs 3–8; S3, S6, S7, S8, and S9 Figs; S11–S20 Tables |

Prediction One (Sony Network Communications Inc.) is a commercial platform; its models were built outside these notebooks. The notebooks are provided without cell outputs because of GitHub's file size limit; running each notebook from top to bottom reproduces the outputs.

## Methods implemented (summary)

- **Sample size:** Riley et al. criteria (shrinkage ≥0.90, Nagelkerke R² difference ≤0.05, margin of error ≤0.05) with prevalence 6.5%, 3–5 predictor parameters, and Cox-Snell R² 0.10–0.20.
- **Restricted cubic splines:** four knots at the cohort-specific 5th, 35th, 65th, and 95th percentiles; unpenalized maximum likelihood; likelihood ratio tests for overall association and nonlinearity; 95% CIs by the delta method.
- **Discrimination:** AUC with DeLong 95% CIs; classification metrics at the Youden-optimal threshold.
- **Calibration:** logistic calibration slope and calibration intercept (logit of the predicted probability as an offset) with Wald 95% CIs; Brier score.
- **Bootstrap internal validation:** 500 iterations; optimism = mean(performance in the bootstrap sample − performance in the original development cohort) (Harrell).
- **Reclassification:** IDI and NRI (risk thresholds 10% and 20%) with Wald CIs (Pencina et al.), in the validation subset with complete MLRA predictors (n = 1,066).
- **Decision curve analysis:** net benefit = TP/n − FP/n × pt/(1 − pt).
- **Interpretability:** TreeSHAP (`shap.TreeExplainer`) for XGBoost Standard in the validation cohort.

## Environment

All notebooks were run in Google Colaboratory.

| Notebook | Software |
| --- | --- |
| 01 | Python 3.13.18; R 4.6.1; pmsampsize 1.1.3; rpy2 3.5.17 |
| 02 | Python 3.13.18; SciPy 1.16.3 |
| 03 | Python 3.13.16; XGBoost 3.2.0; LightGBM 4.6.0; CatBoost 1.2.10; scikit-learn 1.6.1; NumPy 2.1.3; pandas 2.2.3; SciPy 1.16.3; statsmodels 0.15.0; SHAP 0.54.1; matplotlib 3.10.0; seaborn 0.13.2 |

### Reproducibility

- Run each notebook from top to bottom.
- `01_sample_size.ipynb` installs rpy2 and pmsampsize in its first section. If rpy2 is newly installed, restart the runtime when prompted and run the notebook again. This notebook does not require patient data.
- `03_prediction_models.ipynb` installs XGBoost 3.2.0, LightGBM 4.6.0, CatBoost 1.2.10, and scikit-learn 1.6.1 in its first cell and stops if any installed version differs. XGBoost results depend on the library version.
- A unified random seed (seed = 42) is applied through Python's `random` module, NumPy, and framework-specific parameters.
- Framework-specific settings: LightGBM `deterministic = True`, `force_row_wise = True`; XGBoost `tree_method = hist`; XGBoost DART `sample_type = uniform`; CatBoost `bootstrap_type = Bernoulli`; HistGradientBoosting `early_stopping = False`.
- Thread counts are fixed to 1 (XGBoost `nthread`, LightGBM `num_threads`, CatBoost `thread_count`), with `OMP_NUM_THREADS`, `MKL_NUM_THREADS`, `OPENBLAS_NUM_THREADS`, and `NUMEXPR_NUM_THREADS` set to 1.

## Changes from the previous version

- Figure, table, and output file names were updated to the numbering of the revised manuscript.
- The calibration slope and intercept, bootstrap optimism (Harrell's estimator), and restricted cubic spline analysis were corrected to match the definitions stated in the manuscript.
- The low-event stratum exclusion analysis (Table 2, S5 Table) was corrected.
- Subgroup and fairness analyses use the predictions of the final models; the outlier influence analysis retrains the gradient boosting model with the same predictors, hyperparameters, and native missing-data handling as the final model.
- An approximate missing-completely-at-random test that was not used in the manuscript was removed.
- Software versions were updated as listed above.

## Data availability

Patient-level data are not included in this repository. An anonymized dataset will be deposited after approval by the institutional ethics committee, and its DOI will be added here. Until then, data requests may be directed to the corresponding author (Hitoshi Sugawara, hsmdfacp@jichi.ac.jp).

## Archived version

All archived versions are available on Zenodo (concept DOI: 10.5281/zenodo.XXXXXXX, which resolves to the latest version). Version v1.0.0 (DOI: 10.5281/zenodo.20278296) corresponds to the original submission.

## License

MIT License (see `LICENSE`).

