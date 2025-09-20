![R](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)
![Status](https://img.shields.io/badge/Stage-Portfolio--Project-blue)

# heart-attack-policyholder-risk
Logistic regression analysis of heart attack policyholder risk with initiative proposal.
# Heart Attack Policyholder Risk Analysis Proposal

**Author:** Rachel Goldsbury  
**Course:** SNHU DAT 510  
**Instructor:** Professor Kuchibhotla  
**Date:** September 3, 2023  

---

## Project Overview
This project applies **logistic regression** in R to analyze health insurance policyholder data and predict the risk of **second heart attacks**. Using demographic and clinical features such as age, weight, cholesterol, stress, and anxiety, the model identifies high-risk patients and supports insurers in targeting interventions (e.g., stress or weight management programs).  

The goal is to extend patient lives, improve outcomes, and reduce costs for the insurance company. The work follows the **Data Analytics Life Cycle**, ensuring structured, high-quality results that align with organizational goals and enable more effective allocation of healthcare resources.

---

## Goals
- Identify **policyholders at high risk** for second heart attacks.  
- Provide **community education resources** to reduce risks.  
- Improve patient outcomes while **lowering costs and premiums**.  

## 📦 Data
- **Source:** Medical claims database.  
- **Features (available):** age, marital status, gender, weight category, cholesterol, stress management, trait anxiety.  
- **Gaps (to consider adding):** lifestyle (diet, exercise), family history, blood pressure.  
- **Preparation:** remove missing values, handle outliers, standardize/encode categoricals, sanity checks.

---

## 🔬 Methods
- **Model:** Logistic Regression (classification of second-event risk).  
- **Environment:** **R / RStudio**.  
- **Key packages:** `caret` (training/tuning, resampling), optional `pROC` (AUC), `ggplot2` (viz), optional `shiny` (stakeholder demo).  
- **Workflow:**  
  1) **EDA**: distributions, correlations, leakage checks.  
  2) **Split/Resampling**: hold-out or k-fold CV via `caret`.  
  3) **Modeling**: logistic regression with feature scaling; compare simple baselines.  
  4) **Calibration**: check probability calibration if used for thresholds.  
  5) **Thresholding**: select operating point to prioritize **recall** (catch more high-risk) or **precision** (avoid false alarms), depending on clinical/cost trade-offs.

---

## Tools & Packages
- **RStudio** for analysis and visualization.  
- **Caret**: for model training and evaluation.  
- **Shiny**: to build potential stakeholder dashboards.  

---

## Full Paper
You can read the complete project write-up here:  
➡️ [Heart Attack Policyholder Risk Proposal (PDF)](./heart_attack_risk_proposal.pdf)

---

## Visualization Plan
Planned visuals include:  
- **Line charts**: tracking risk outcomes over time.  
- **Bar charts**: comparing categorical features (e.g., weight groups).  
- **Heatmaps**: correlations between variables.  

---
---

## 📏 Evaluation
- **Primary metrics:** Precision · Recall · F1-score · ROC-AUC.  
- **Monitoring plan:** Track drift in inputs/outcomes; periodic re-training; human-in-the-loop review of flagged cases.  
- **Business lens:** Sensitivity analysis on intervention capacity (who we can enroll vs. who we must defer).

---

## 🚀 Deployment Concept
1) Score eligible policyholders monthly/quarterly.  
2) **Route high-risk** members to care-management programs (stress/weight).  
3) **Close the loop:** record enrollment/engagement/outcomes to retrain and improve the model.  
4) **Governance:** document model card, approvals, and privacy controls.

---

## 🔄 Data Analytics Life Cycle (Milestones 1–4)
- **Milestone 1 — Problem & Data**  
  Define second-event risk; confirm available claims features; acknowledge lifestyle gaps.
- **Milestone 2 — Goals & DALC**  
  Problem → Data collection → Prep/EDA → Model → Evaluate → Deploy → **Monitor**.
- **Milestone 3 — Tools**  
  **R/RStudio**; `caret` (multi-algo testing, feature selection, tuning); **Shiny** (interactive stakeholder views).
- **Milestone 4 — Conclusion/Value**  
  Targeted care improves patient outcomes, **reduces second events**, and **lowers cost** via focused resource allocation.

---

## 🧪 Reproducibility (R sketch)
> *Illustrative; adapt column names to your dataset.*

```r
# packages
library(caret); library(pROC); library(ggplot2)

# data
df <- read.csv("claims.csv", stringsAsFactors = TRUE)
df <- na.omit(df)

# split
set.seed(510)
idx <- createDataPartition(df$second_event, p = 0.8, list = FALSE)
train <- df[idx, ]; test <- df[-idx, ]

# model
ctrl <- trainControl(method = "repeatedcv", number = 5, repeats = 3,
                     classProbs = TRUE, summaryFunction = twoClassSummary)
fit <- train(second_event ~ ., data = train,
             method = "glm", family = binomial(),
             trControl = ctrl, metric = "ROC")

# evaluate
pred_prob <- predict(fit, test, type = "prob")[, "Yes"]
pred_lab  <- ifelse(pred_prob >= 0.5, "Yes", "No")
confusionMatrix(factor(pred_lab, levels = c("No","Yes")),
                factor(test$second_event, levels = c("No","Yes")))
roc_obj <- roc(response = test$second_event, predictor = pred_prob)
auc(roc_obj)
```

---

## Citation
North, M. (2012). *Data Mining for the Masses*.  

---
