# COMPAS Fairness Analysis (AIR, ME, SMD, Error Rates, Intersectionality)

## Project Overview
This project evaluates fairness in the COMPAS recidivism risk‑scoring system using the ProPublica two‑year dataset. The analysis follows a structured pipeline to compute group‑based disparities, intersectional impacts, and error‑rate differences across demographic groups. Metrics include Adverse Impact Ratio (AIR), Mean Difference (ME), Standardized Mean Difference (SMD), False Positive Rate (FPR), and False Negative Rate (FNR). The goal is to assess whether COMPAS predictions are distributed equitably across race and sex.

## Data Source
The dataset is loaded directly from ProPublica’s public repository:

> “compas-scores-two-years.csv” (ProPublica COMPAS analysis dataset)  
> Filtered to remove invalid screening windows, missing recidivism labels, and out‑of‑scope charge types.

A binary **high_risk** label is created using the standard threshold:  
`high_risk = 1 if decile_score ≥ 7 else 0`.

## Analysis Steps

### 1. Compute AIR, ME, and SMD for Race and Sex
Group‑level high‑risk assignment rates were compared to reference groups:

- **Race reference:** Caucasian  
- **Sex reference:** Male  

From the analysis:  
> African‑American and Native American defendants show the largest disparities, with AIR values of 2.34 and 3.41, respectively.

These metrics consistently show that African‑American and Native American groups receive high‑risk labels at substantially higher rates than Caucasian defendants.

### 2. Intersectional Analysis (Race × Sex)
Intersectional groups were constructed (e.g., “Native American – Female”). AIR was computed relative to **Caucasian – Male**.

From the analysis:  
> Native American – Female… AIR ≈ 6.31… classified as high‑risk more than six times as often as the reference group.

This reveals compounded disparities not visible in single‑axis analysis.

### 3. Error‑Rate Disparities (FPR & FNR)
Confusion‑matrix components were computed for each racial group to derive:

- **False Positive Rate (FPR)**  
- **False Negative Rate (FNR)**  
- **Accuracy**

Two‑proportion z‑tests were used to test significance between African‑American and Caucasian groups.

From the analysis:  
> FPR z = 10.392, p < 0.001… FNR z = –10.780, p < 0.001.

The model over‑predicts risk for Black defendants and under‑predicts risk for White defendants.

### 4. Visualization
A grouped bar chart was produced showing FPR and FNR by race, with dashed reference lines for Caucasian error rates. The visualization highlights substantial disparities, particularly the elevated false positive rates for African‑American and Native American defendants.

## AI Assistance Disclosure
Portions of this README and supporting documentation were drafted with the assistance of Microsoft Copilot. All analysis, code execution, and interpretations were reviewed and verified by the author to ensure accuracy, methodological transparency, and academic integrity.
