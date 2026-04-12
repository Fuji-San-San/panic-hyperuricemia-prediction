# panic-hyperuricemia-prediction
Python code for "Predicting 72-hour fatality in panic-level hyperuricemia: Comparative evaluation of machine learning and logistic regression models." Implements MLRA, XGBoost, LightGBM, CatBoost, SHAP, DCA, and fairness assessment. PLOS ONE (under review).

# Predicting 72-hour fatality in panic-level hyperuricemia

## Overview
This repository contains Python code for the analyses reported in:

> Hayashi R, Hayashi S, Sanayama H, Fukuchi T, Watanabe T, Omoto K,
> Sugawara H. Predicting 72-hour fatality in panic-level hyperuricemia:
> Comparative evaluation of machine learning and logistic regression
> models, a retrospective cohort study. *PLOS ONE* (under review).

## Notebooks

| File | Contents | Outputs |
|------|----------|---------|
| `01_sample_size.ipynb` | Sample size calculation (Riley et al. 2019) | S1 Table, S1 Fig |
| `02_uric_acid_epidemiology.ipynb` | Uric acid concentration-response and age analyses | Fig 3A, 3B, S2 Fig, S3 Fig, S6 Table |
| `03_prediction_models.ipynb` | MLRA, gradient boosting (×8), SHAP, DCA, fairness | Tables 3–6, S9–S17, Figs 4–10 |

## Environment
- Google Colaboratory (Python 3.12.13)
- See `requirements.txt` for all library versions

## Data Availability
Patient data are not included in this repository owing to
institutional data governance requirements. Anonymized data
supporting the conclusions will be made available upon publication
through the Jichi Medical University data repository. Requests
should be directed to the corresponding author:
hsmdfacp@jichi.ac.jp

## License
MIT License
