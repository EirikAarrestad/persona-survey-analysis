# Persona Survey Analysis

Exploratory data analysis and predictive modelling of a survey dataset, combined into one data science pipeline with a final report. Project in **INF161 – Data Science** at the University of Bergen (autumn 2026).

## Problem

[One or two sentences: what are you predicting, and why is it interesting?]

## Approach

1. **Exploratory data analysis:** distributions, missing values and relationships between variables.
2. **Preprocessing:** cleaning, encoding of categorical variables, train/validation/test split.
3. **Modelling:** [models you compared, e.g. logistic regression, random forest] with hyperparameters chosen on the validation set.
4. **Evaluation:** final model evaluated once on the held-out test set.

## Results

| Model | [Metric] (validation) | [Metric] (test) |
| --- | --- | --- |
| Baseline | | |
| [Model] | | |
| [Best model] | | |

[One or two sentences on the main finding and how reliable it is.]

## Repository structure

```
├── del3_final.ipynb     # full pipeline: EDA, preprocessing, modelling, evaluation
├── rapport3_final.qmd   # final report (Quarto)
└── README.md
```

## Data

The dataset (`persona_survey.csv`) was provided through the course and is not included in this repository. [Remove this line if you are allowed to publish it.]

## How to run

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
jupyter notebook del3_final.ipynb
```

Place `persona_survey.csv` in the same folder as the notebook first.

## Author

Eirik Aarrestad – BSc Statistics and Data Science, University of Bergen
