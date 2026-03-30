# COMPAS Recidivism Risk Analysis

This repository contains an end‑to‑end analysis of the COMPAS recidivism dataset, focusing on model
performance, interpretability, and fairness across racial groups. The project implements two machine
learning models—Logistic Regression and Gradient‑Boosted Trees—and evaluates their behavior using
SHAP, LIME, and DiCE. A governance memo is included to summarize risks, limitations, and recommended
monitoring practices for judicial use.


## Project Overview

The goal of this assignment is to examine how predictive models behave when applied to criminal
justice data and to assess whether their decisions are equitable across demographic groups. The
analysis follows four major steps:

### **1. Data Preprocessing and Feature Engineering**
- Numeric feature: `priors_count`
- Categorical features: `gender_factor`, `age_factor`, `race_factor`, `crime_factor`, `score_factor`
- One‑hot encoding for categorical variables
- Standardization for numeric variables
- Unified preprocessing pipeline to prevent data leakage

### **2. Model Training**
Two models were trained using the same preprocessing pipeline:
- **Logistic Regression** (interpretable baseline)
- **Gradient‑Boosted Trees** (non‑linear, higher performance)

### **3. Fairness Evaluation**
Group‑level metrics (accuracy, FPR, FNR, AUC) were computed for each racial group.  
The results reveal:
- Higher **false positive rates** for African‑American defendants  
- Higher **false negative rates** for Caucasian defendants  
- Smaller groups (Asian, Native American) produce unstable AUC values due to limited sample size

### **4. Model Interpretability**
Three complementary explanation methods were used:

#### **SHAP**
- Identifies global feature importance  
- `priors_count`, `score_factor_High`, and age categories are the strongest drivers  
- Individual waterfall plots show how features push predictions up or down  

#### **LIME**
- Provides local, case‑specific explanations  
- Confirms that priors, age, and charge severity dominate individual predictions  
- Race indicators appear but contribute minimally, reflecting one‑hot encoding structure  

#### **DiCE Counterfactuals**
- Shows what changes would flip a prediction  
- Large increases in `priors_count` or shifts in `age_factor` are typically required  
- Patterns are similar across racial groups, though thresholds differ based on baseline features  


## Key Findings

- The model relies heavily on prior criminal history, COMPAS score category, age group, and charge severity.
- Race is not used directly, but disparities emerge indirectly through correlated variables.
- African‑American defendants face higher false positive rates; Caucasian defendants face higher false negative rates.
- SHAP and LIME show consistent feature importance patterns.
- DiCE reveals that substantial changes in priors or age category are needed to flip predictions.

## Repository Structure

