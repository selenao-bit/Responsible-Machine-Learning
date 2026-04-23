# From Accuracy to Accountability: Stress Testing a Predictive Model

## Introduction

This project provides a comprehensive analysis of a predictive model built using the COMPAS (Correctional Offender Management Profiling for Alternative Sanctions) dataset. The primary goal is to assess the model's fairness, generalization capabilities, and robustness across various demographic groups and data conditions. We delve into distribution drift, potential spurious correlations, and the model's sensitivity to key features, concluding with a detailed slice-based evaluation to identify and understand performance disparities.

## Data

The data used in this analysis is the 'compas-scores-two-years.csv' dataset, originally sourced from ProPublica's investigative journalism on racial bias in predictive algorithms. This dataset contains information on criminal defendants in Broward County, Florida, including their demographic attributes, criminal history, and COMPAS risk assessment scores. The raw data is loaded from `https://raw.githubusercontent.com/propublica/compas-analysis/master/compas-scores-two-years.csv`.

After initial loading, the dataset undergoes significant preprocessing, including filtering to ensure data quality and relevance, conversion of data types (e.g., datetime, categorical), and creation of new categorical features (e.g., `crime_factor`, `age_factor`, `race_factor`, `gender_factor`, `score_factor`) with appropriate reference levels for modeling.

## Methodology

The analytical approach involves several key steps:

1.  **Data Preprocessing:** The raw dataset was filtered to remove invalid or irrelevant entries and converted data types for consistency. New categorical features such as `crime_factor`, `age_factor`, `race_factor`, `gender_factor`, and `score_factor` were engineered from existing columns, with appropriate reference levels set for each.
2.  **Train/Test Split:** The preprocessed data was split into training (80%) and testing (20%) sets, stratified by the target variable (`score_binary`) to ensure representative distributions in both sets.
3.  **Model Selection:** A Logistic Regression model was chosen for its interpretability and was implemented within a `scikit-learn` pipeline. The pipeline included a `ColumnTransformer` for one-hot encoding categorical features and a `LogisticRegression` classifier with `solver='liblinear'` and L2 regularization.
4.  **Distribution Drift Analysis:** Population Stability Index (PSI), Kolmogorov-Smirnov (KS) tests, and Maximum Mean Discrepancy (MMD) were used to compare feature and target distributions between training and testing sets, ensuring data consistency.
5.  **Generalization Assessment:** The model's performance was evaluated on both training and testing data using metrics such as accuracy, AUC, Brier score, and log loss. Performance gaps between train and test sets were analyzed to diagnose potential overfitting.
6.  **Spurious-Correlation Probe:** Counterfactual swaps were performed on sensitive attributes (`gender_factor`, `age_factor`, `race_factor`) to measure the mean absolute change in predicted probabilities, identifying features that might lead to spurious correlations.
7.  **Robustness Analysis:** Stress tests were conducted on `priors_count` to understand its impact on predictions, complemented by Individual Conditional Expectation (ICE) curves and a global sensitivity index to visualize and quantify feature influence.
8.  **Slice-based Evaluation:** Model performance metrics (accuracy, AUC, Brier score, FPR, FNR) were calculated for various demographic slices (race, gender, age) to identify and analyze potential disparities in performance.

## Analysis & Results

### Distribution Drift

- PSI and KS tests on numeric features (`priors_count`, `two_year_recid`) showed very low PSI values (0.0104 and 0.0008) and high KS p-values (0.2113 and 0.9862), indicating no significant change or statistical difference between train and test distributions.
- Maximum Mean Discrepancy (MMD) in the encoded feature space yielded a low MMD² value of 0.0023, suggesting high similarity in overall feature distributions between training and test sets.
- Comparison of target score distributions (`score_binary`) showed highly consistent means (0.4458 for train and 0.4453 for test), further confirmed by a side-by-side count plot, indicating successful stratification and data consistency.

### Generalization

- The Logistic Regression model demonstrated strong generalization with highly consistent performance across training and test sets.
- Key metrics were very close: Accuracy (Train: 0.7545, Test: 0.7522) and AUC (Train: 0.8292, Test: 0.8328).
- Performance gaps (Train - Test) were negligible, indicating no evidence of overfitting. The model learned robust patterns that generalize effectively to unseen data.

### Spurious-correlation Probe

- Counterfactual swaps revealed varying model sensitivities to demographic attributes:
  - Most sensitive to `age_factor` (mean absolute probability shifts of ~0.20-0.24).
  - Moderately sensitive to `race_factor` (~0.08-0.12 shifts).
  - Least sensitive to `gender_factor` (~0.025 shift).
- This highlights features where spurious correlations might exist or where the model's reliance is stronger.

### Robustness

- Stress testing on `priors_count` showed high sensitivity; increasing prior convictions from 0 to 10 dramatically raised the mean predicted probability of high-risk from 0.448 to 0.845.
- Individual Conditional Expectation (ICE) curves visually confirmed this strong, positive relationship, and a global sensitivity index quantified `priors_count`'s high influence (sensitivity index of 0.048786, range of mean score 0.71184).

### Slice-based Evaluation

- **Race:** African-Americans had a higher False Positive Rate (FPR) of 0.3540, while Caucasians had a higher False Negative Rate (FNR) of 0.3800. The 'Other' category showed better overall performance.
- **Gender:** Males had slightly better overall metrics (AUC 0.8402, lower Brier) but also higher FPR (0.3366) and FNR (0.3800) compared to Females.
- **Age:** The 'Less than 25' group exhibited a significantly high FPR of 0.6633 and the lowest AUC (0.7077), indicating poor performance. The 'Greater than 45' group had a high FNR of 0.4694.

## Conclusion

This analysis demonstrates that while the Logistic Regression model exhibits strong generalization capabilities and no signs of overfitting, significant disparities in performance exist across different demographic groups. The model's predictions are particularly sensitive to `priors_count` and show varying degrees of sensitivity to `age_factor`, `race_factor`, and `gender_factor`. The slice-based evaluation highlighted concerning biases, such as a high False Positive Rate for African-Americans and individuals under 25, and a high False Negative Rate for Caucasians and individuals over 45. These findings underscore the importance of fairness evaluations in machine learning and suggest that further work is needed to mitigate biases and ensure equitable outcomes.

## Usage

To reproduce the results or run the analysis:
1.  Open this notebook in Google Colab.
2.  Run all cells sequentially.
3.  The data will be automatically downloaded from the provided URL.

## Dependencies

This project relies on the following Python libraries:
-   `pandas`
-   `numpy`
-   `scikit-learn` (sklearn)
-   `statsmodels`
-   `matplotlib`
-   `seaborn`
-   `scipy`
