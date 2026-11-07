# From Product Text to Better Product Selection

This project evaluates whether product descriptions and structured product
attributes can predict archival customer ratings and help identify higher-rated
products within comparable choice sets.

## Project overview

The prediction target is a product's mean customer review rating on a 1–5 star
scale. The models use product titles, features, descriptions, category, price,
and text representations.

This is an offline prediction and product-selection evaluation. Review ratings
measure satisfaction expressed in the available review records; they are not
measures of sales, purchase intention, or profit. This repository does not
report results from a human-subject experiment.

## Approach

We compare simple baselines with several predictive models:

- Mean-rating and category-average baselines
- Linear regression and Ridge regression using structured features
- Ridge regression using structured features and TF-IDF text features
- Models using pretrained MiniLM sentence embeddings
- Random forest, gradient boosting, and neural-network models

Hyperparameters are selected using grouped cross-validation. Product families
are kept together across data splits to reduce the chance of related products
appearing in both training and evaluation data.

For the selection evaluation, eligible products are grouped into comparable
choice sets. Each model selects the product with the highest predicted rating.
Selection gain is the selected product's observed mean rating minus the mean
rating of a uniformly random choice from the same set.

## Final test results

The final test evaluation included 121 choice sets.

| Model | Test MAE | Test RMSE | Test OSR² | Gain over random selection |
|---|---:|---:|---:|---:|
| Ridge: structured features + TF-IDF text | 0.278 | 0.375 | 0.437 | +0.164 stars (90% CI: 0.114–0.212) |
| Ridge: structured features only | 0.322 | 0.429 | 0.263 | +0.007 stars |
| Category-average baseline | 0.325 | 0.436 | 0.237 | −0.018 stars |

The primary model's MAE was 0.044 lower than the structured Ridge baseline
(90% CI: −0.049 to −0.040).

These are offline results on held-out products. They do not show that the model
causes product ratings, purchases, or business outcomes to improve.

![Final test selection performance](figures/test_selection_gain.png)

## Repository contents

- `notebooks/modeling.ipynb` — model comparison, diagnostics, and final test evaluation
- `figures/` — project figures
- `Amazon/` — required input data files, if permitted to include or link
- `requirements.txt` — Python dependencies

## Setup and usage

Run the notebook from the repository root.

1. Create and activate a Python environment.
2. Install the dependencies:

   ```bash
   pip install -r requirements.txt