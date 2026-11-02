# What does a language model believe about historical personas?

A language model answered 62 survey questions *as if it were* each of 10,000 famous people and fictional characters. This project explores what those answers reveal about the model's picture of history, and whether the answers alone can predict **when a person lived** and **what they did**.

Final project in **INF161 – Data Science** at the University of Bergen (autumn 2026). The full report is in Norwegian (`rapport3_final.pdf`); this README summarises it in English.

> The answers are the language model's idea of how each person would respond, not the people's own views. Every pattern found here describes the language model, not the historical figures.

## Results

| Task | Final model | Test score | Baseline |
| --- | --- | --- | --- |
| **A:** birth year (regression) | Random Forest on all answers, text via TF-IDF (300 most common words) | MAE **285 years**, median error 94 years | MAE 452 years, median error 90 years |
| **B:** occupation category (10 classes) | Logistic regression (C = 1) on all answers, text via TF-IDF | Balanced accuracy **0.692**, macro-F1 0.662 | Balanced accuracy 0.100 |

## Data

`persona_survey.csv` has 10,000 rows and 74 columns: 9,000 real people from the Pantheon database and 1,000 fictional characters. The 62 answer columns cover Big Five personality, the political compass, morality and belief, personal preferences, "moral exchange rates" and seven free-text answers. Ground-truth columns include birth year, occupation and gender.

The dataset was provided through the course and is not included in this repository.

## Method

- **Validation:** checked data types, value ranges against the data catalogue, duplicates and missing values. Extreme values (e.g. 500 cm height) mostly belong to fictional characters and were kept, with log10 transforms and tree models to handle them.
- **Split:** 70/15/15 train/validation/test, stratified on occupation category, `random_state=42`. All EDA after the split uses only the training set.
- **Features:** Big Five scores, two political axes, log-transformed exchange rates, word counts, number of missing answers, and all free text combined for TF-IDF.
- **Missing data:** median imputation for numbers, a separate `"missing"` category for categorical answers, empty string for text.
- **Pipeline:** everything runs inside one scikit-learn `Pipeline` (`FunctionTransformer` → `ColumnTransformer` → model), so imputation, scaling and the TF-IDF vocabulary are learned only from training data.
- **Models:** linear/logistic regression, linear models fitted with stochastic gradient descent (absolute-error loss for task A), and Random Forest, all compared against a dummy baseline.
- **Metrics:** MAE and median error for task A (birth years range from −10,000 to 10,190, so RMSE would be dominated by a few fictional characters). Balanced accuracy and macro-F1 for task B, because the classes are very imbalanced.
- **Robustness:** 5-fold cross-validation on the training set for the top three models in each task, plus permutation importance (task A) and coefficients (task B).

## Key findings

**Exploratory analysis**
- The model has a fixed image of a "famous person": high agreeableness and conscientiousness, a self-reported IQ around 145, and 93 % prefer a lasting legacy over a happy life.
- People born before 1800 are portrayed as religious and authoritarian; modern people as liberal, less religious and more open to the simulation hypothesis.
- Fictional characters give a higher probability that we live in a simulation (Welch's t-test, p ≈ 10⁻²³), and their free-text answers separate them from real people with ~0.77 balanced accuracy.

**Prediction**
- Free text is the most informative input in both tasks.
- `last_meal` is by far the most important answer for birth year: the model lets historical figures choose food from their own era.
- The "joke question" `favorite_data_science_fact` is the best single answer for occupation, because the model phrases it in the style of the person's field.
- Switching from squared to absolute error loss, with the same linear model, cut MAE from 509 to 438 years.
- Errors follow meaningful boundaries: fictional and ancient people have the largest errors, and related occupations (writing, philosophy, visual arts) are confused with each other.

## Limitations

- One validation set of 1,500 rows was too noisy to separate the best models. In task B the validation winner was not best in cross-validation, and the test score matched the CV estimate rather than the validation score.
- Fictional characters (10 % of the data) account for a large share of the error in task A.
- The task A model does not beat the baseline on median error.
- The models predict what *this* language model believes, so they cannot be used to date or classify real people.

## Repository structure

```
├── main.ipynb           # full pipeline: data validation, EDA, modelling, evaluation
├── rapport3_final.pdf   # final report (Norwegian)
├── requirements.txt     # library versions
├── img/                 # figures produced by the notebook
└── README.md
```

## How to run

```bash
pip install -r requirements.txt
jupyter notebook main.ipynb
```

Place `persona_survey.csv` in the same folder as the notebook. It runs top to bottom with no manual steps, and all random steps use `random_state=42`.

## Author

Eirik Aarrestad – BSc Statistics and Data Science, University of Bergen
