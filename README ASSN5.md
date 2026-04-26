# Fairness, Poisoning Robustness, and Membership‑Inference Analysis

This repository contains a full evaluation of fairness, robustness, and privacy risks in machine‑learning models trained on the COMPAS dataset. The project implements baseline fairness analysis, fairness‑targeted poisoning attacks, and shadow‑model membership‑inference (MI) attacks. The goal is to assess how model performance, fairness metrics, and privacy vulnerabilities interact, and to identify the highest‑risk failure mode across all experiments.

## AI Disclosure

Portions of this project’s documentation and narrative explanations were developed with assistance from Microsoft Copilot. Copilot was used to help draft text, refine wording, and improve clarity and organization. All coding, data processing, statistical modeling, metric computation, and interpretation of results were independently completed and verified by the author.

## Overview

This project evaluates three major dimensions of model reliability:

- **Fairness:** Whether predictions and error rates differ across racial groups  
- **Robustness:** Whether targeted poisoning attacks can manipulate fairness without affecting accuracy  
- **Privacy:** Whether the model leaks membership information through confidence‑based MI attacks  

The analysis uses Logistic Regression (LR) and Gradient Boosted Trees (GBT) trained on a cleaned subset of the COMPAS dataset.

## Data Sources

The analysis uses the ProPublica COMPAS two‑year recidivism dataset:

- `compas-scores-two-years.csv`

This dataset includes demographic attributes, prior history, COMPAS decile scores, and two‑year recidivism outcomes.

## Baseline Fairness Analysis

The baseline evaluation examines whether the trained models exhibit disparities across racial groups. Metrics such as AUC, AIR, FPR/FNR gaps, and PSI are used to assess predictive performance, fairness, and feature stability. This establishes the reference point for later robustness and privacy experiments.

## Fairness‑Targeted Poisoning Attacks

A label‑flip poisoning attack is applied at varying poison rates to determine whether fairness metrics can be manipulated without affecting accuracy. Only labels are modified; feature distributions remain unchanged. The analysis evaluates how poisoning affects AIR, AUC, and PSI, and whether the attack can evade standard monitoring tools.

## Membership‑Inference (MI) Analysis

Shadow‑model MI attacks are used to evaluate whether the models leak membership information through confidence scores. MI AUC is computed for both LR and GBT, and the relationship between generalization gap and MI vulnerability is examined. Additional experiments vary L2 regularization strength to assess whether regularization improves privacy.

## Highest‑Risk Finding

Across all experiments, the fairness‑targeted poisoning attack emerges as the highest‑risk vulnerability. The attack can substantially distort fairness metrics while leaving accuracy and PSI unchanged, making it both high‑impact and difficult to detect. This vulnerability directly affects racial fairness and introduces severe disparate impact.

## Mitigations

### Proactive Mitigation: Fairness‑Aware Training

Fairness‑aware regularization or constraints can limit disparities such as AIR or FPR gaps during model fitting. This reduces the model’s sensitivity to label‑based manipulation and helps maintain equitable outcomes across racial groups.

### Reactive Mitigation: Fairness‑Specific Monitoring

Monitoring AIR, FPR gaps, or group‑based error metrics in real time allows for rapid detection of fairness degradation. Unlike PSI or accuracy, fairness metrics respond directly to label manipulation, making them essential for detecting poisoning attacks.

### Disparate Impact Considerations

Both the attack and the mitigations have implications for racial groups. Poisoning disproportionately harms the targeted group, while fairness‑based monitoring may trigger more frequently for groups with higher base rates. Careful calibration is required to ensure equitable operational impact.

## Requirements

- pandas  
- numpy  
- matplotlib  
- seaborn  
- scikit‑learn  

## Acknowledgments

Data provided by ProPublica:  
https://github.com/propublica/compas-analysis
