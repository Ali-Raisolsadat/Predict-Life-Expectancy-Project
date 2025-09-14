# Predict Life Expectancy Project

## Overview 
The goal is to understand how various socioeconomic, demographic, and environmental factors affect life expectancy across different geographical areas (countries, regions, or groups of countries).  

There is an additional goal which is to **predict life expectancy** using provided features and identify which factors have the strongest influence.

---

## Dataset
The dataset contains socioeconomic variables for each geographical area. Each row corresponds to a country, region, or a provision within a large country.  

| Column | Description |
|--------|-------------|
| `surface_area` | The total area in square kilometers |
| `agricultural_land` | Agricultural land of the total area in square kilometers |
| `forest_area` | Forest area in the total area in square kilometers |
| `armed_forces_total` | Number of armed forces paid by this area |
| `urban_pop_major_cities` | Percent of population dwelling in major cities |
| `urban_pop_minor_cities` | Percent of population dwelling in minor cities |
| `national_income` | National income as an ordinal categorical variable |
| `inflation_annual` | Yearly inflation rate |
| `inflation_monthly` | Average monthly inflation rate (annual/12) |
| `inflation_weekly` | Average weekly inflation rate (annual/52) |
| `mobile_subscriptions` | Number of mobile subscriptions per person |
| `internet_users` | Average number of people using the internet per 100 or 1000 people |
| `secure_internet_servers_total` | Number of secure internet servers in the area |
| `improved_sanitation` | Percent of population with access to improved sanitation |
| `women_parliament_seats_rate` | Percent of parliament seats occupied by women |
| `life_expectancy` | Years of life an average person is expected to live (target variable) |

> Note: Some features may require preprocessing, including handling missing values, scaling, or encoding.

---

## Objective
- Build a model to predict the **life expectancy** for each row in the test dataset.  
- Generate a CSV submission named `submission.csv` with two columns:  
  1. **Index** of the test row  
  2. **Predicted life expectancy**  

**Evaluation Metric**:  
- **Mean Absolute Error (MAE)** between predicted and actual life expectancy.

---

## Workflow
1. Load train and test datasets.
2. Preprocess data (handle missing values, encode categories, scale features).
3. Perform exploratory analysis to understand patterns and feature relationships.
4. Train multiple predictive models and tune hyperparameters.
5. Validate models and select the best-performing model based on MAE.
6. Predict life expectancy for the test dataset.
7. Save predictions in `life_expectancy_test.csv`.
8. Analyze feature importance to understand socioeconomic influences on life expectancy.

---

## Predictive Modeling: Random Forest Regressor

For predicting life expectancy, a **Random Forest Regressor** was employed. The model uses an ensemble of decision trees to capture non-linear relationships between socioeconomic variables and life expectancy.

### Hyperparameter Grid Search

To optimize the model, a **manual grid search** was performed over the following hyperparameters:

| Hyperparameter | Description | Tested Values |
|----------------|-------------|---------------|
| `n_estimators` | Number of trees in the forest | 100, 125, 150, 175, 200 |
| `max_features` | Number of features considered at each split | 2, 5, 7, 10, 13 |
| `max_depth` | Maximum depth of each tree | 5, 6, 7, 8, 9, 10 |
| `min_samples_split` | Minimum samples required to split a node | 2, 3, 4, 5 |
| `min_samples_leaf` | Minimum samples required at a leaf node | 2, 3, 4, 5 |
| `criterion` | Metric for splitting nodes | `absolute_error` (Mean Absolute Error) |
| `random_state` | Seed for reproducibility | 33 |
| `cv` | Number of folds for cross-validation | 4 |

> **Note**: The `absolute_error` criterion was used to directly minimize the MAE, which is the evaluation metric for this competition.

### Model Selection Procedure

1. Iterate over all possible hyperparameter combinations.
2. Train a Random Forest Regressor on the training dataset for each combination.
3. Evaluate model performance using 4-fold cross-validation and **Mean Absolute Error (MAE)**.
4. Record the hyperparameters that achieve the lowest MAE.
5. Use the best hyperparameter combination to train the final model.

