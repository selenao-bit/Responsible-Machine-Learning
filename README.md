# COMPAS Recidivism and Violent‑Recidivism Analysis

This repository contains a full statistical analysis of the COMPAS risk‑assessment system using the public datasets released by ProPublica. The project reproduces and extends key components of ProPublica’s methodology, including data cleaning, exploratory data analysis, logistic regression, Cox proportional hazards modeling, and survival analysis. The goal is to evaluate whether COMPAS scores are distributed fairly across demographic groups and whether the scores meaningfully predict recidivism.

## Overview

COMPAS (Correctional Offender Management Profiling for Alternative Sanctions) is a proprietary algorithm used in courts to estimate a defendant’s likelihood of reoffending. Because these scores influence sentencing, bail, and supervision decisions, it is essential to examine whether the system behaves equitably across racial, gender, and age groups.

This project analyzes:

- General recidivism scores  
- Violent‑recidivism scores  
- Racial disparities in score assignment  
- Predictive accuracy using Cox proportional hazards models  
- Survival curves by race, gender, and COMPAS score level  

All analyses are performed in Python using `pandas`, `statsmodels`, `lifelines`, and `seaborn`.

## Data Sources

The analysis uses the following public datasets from ProPublica:

- `compas-scores-two-years.csv`  
- `compas-scores-two-years-violent.csv`  
- `cox-parsed.csv`  
- `cox-violent-parsed.csv`  

These datasets include demographic information, charge severity, prior history, COMPAS scores, and two‑year recidivism outcomes.

## Data Cleaning and Filtering

Following ProPublica’s methodology, the data is filtered to:

- Include only cases screened within ±30 days of arrest  
- Exclude invalid recidivism indicators  
- Remove “N/A” COMPAS scores  
- Remove charge degree “O”  
- Convert jail in/out dates to durations  
- Remove rows where end < start  

After filtering, the general recidivism dataset contains **6,172** individuals, and the violent‑recidivism dataset contains **4,020** individuals.

From the document:  
“Most of the data points (3,532) are concentrated in the 25–45 age range… The dataset indicates that 3,175 individuals identify as African‑American…”  

## Exploratory Data Analysis

### Demographic Imbalance

The dataset is not demographically balanced:

- **51.4%** African‑American  
- **34.1%** Caucasian  
- **8.3%** Hispanic  
- **0.5%** Asian  
- **0.18%** Native American  

Gender distribution is also skewed:

- **81%** Male  
- **19%** Female  

These imbalances matter because COMPAS is trained primarily on African‑American male defendants.

From the document:  
“The dataset contains substantially more males than females… which may reduce its accuracy when making predictions for female individuals.”

### Score Distribution

- Low: **3,421**  
- Medium: **1,607**  
- High: **1,144**  

Black defendants show a more uniform spread across decile scores, while white defendants are concentrated in the lowest deciles.

## Logistic Regression: Racial Bias in Score Assignment

A logistic regression model was fit to predict whether a defendant receives a Medium/High score vs. Low, controlling for:

- Age  
- Gender  
- Race  
- Prior count  
- Charge severity  
- Two‑year recidivism  

### Key Findings

- African‑American defendants have significantly higher odds of receiving a higher COMPAS score (**coef = 0.4772**, p < 0.001).  
- Black defendants are **~45% more likely** than white defendants to receive a higher score, even after controls.  
- Individuals under 25 are **~2.5× more likely** to receive a higher score.  
- Women are **~19% more likely** than men to receive a higher score.

From the document:  
“The logistic regression results show that race remains a significant predictor… reinforcing evidence of racial disparities in COMPAS risk assessments.”

## Violent‑Recidivism Score Analysis

A parallel logistic regression was run for violent‑risk scores.

### Key Findings

- Defendants under 25 are **~7.4× more likely** to receive a higher violent‑risk score.  
- African‑American defendants do not receive the protective effects seen in other racial groups.  
- Hispanic, Asian, and “Other” groups have significantly lower odds of receiving high violent‑risk scores.

From the document:  
“Black defendants continue to receive higher risk classifications at disproportionate rates.”

## Predictive Accuracy: Cox Proportional Hazards Models

To evaluate whether COMPAS scores predict actual recidivism, Cox PH models were estimated using counting‑process survival data.

### General Recidivism

- Medium risk: **HR ≈ 2.22**  
- High risk: **HR ≈ 3.49**  
- Concordance: **~0.60** (lower than Northpointe’s reported 0.68)

### Decile Score Model

- Each 1‑point increase in decile score increases hazard by **~22%**  
- Concordance: **~0.62**

### Race‑Specific Models

- White defendants: High‑risk HR ≈ **3.61**  
- Black defendants: High‑risk HR ≈ **2.99**  

This means COMPAS separates risk levels **more sharply for white defendants**.

From the document:  
“The separation between low‑ and high‑risk individuals is sharper for white defendants.”

## Survival Analysis

Kaplan–Meier curves were generated for:

- Full sample  
- By race  
- By gender  
- By COMPAS score  

### Key Findings

- Black defendants have lower survival probabilities at every score level.  
- White defendants show clearer separation between Low, Medium, and High curves.  
- Women have substantially higher survival probabilities than men.  
- A High‑risk woman’s curve resembles a Medium‑risk man’s curve.

From the document:  
“As these plots show, the COMPAS score treats a High‑risk woman the same as a Medium‑risk man.”

## Confusion Matrices and Error Rates

Using ProPublica’s truth‑table logic:

### General Recidivism

- Overall false positive rate: **32.35%**  
- Black defendants: **44.85%**  
- White defendants: **23.45%**

Black defendants are **1.91× more likely** to be incorrectly labeled high‑risk.

### Violent Recidivism

- Black false positive rate: **38.14%**  
- White false positive rate: **18.46%**

Black defendants are **~2× more likely** to be false positives for violent risk.

From the document:  
“African Americans are 1.91 times as likely to be incorrectly labeled as high risk…”

## Conclusions

This analysis reproduces and extends ProPublica’s findings:

- COMPAS scores show **systematic racial disparities**.  
- Black defendants are more likely to receive higher scores, even after controlling for relevant factors.  
- COMPAS overpredicts risk for Black defendants and underpredicts risk for White defendants.  
- Predictive accuracy is modest (concordance ~0.60–0.64).  
- Score separation is weaker for Black defendants, meaning the algorithm is less informative for the group it scores most harshly.

Overall, the results raise concerns about fairness, calibration, and disparate impact in algorithmic risk assessment.

## Requirements

- pandas
- numpy
- matplotlib
- seaborn
- statsmodels
- lifelines

## Acknowledgments

Data provided by ProPublica:  
https://github.com/propublica/compas-analysis

