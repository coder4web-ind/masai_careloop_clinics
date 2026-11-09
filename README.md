# CareLoop Clinics — No-Show Guard
Repository Link: [https://github.com/coder4web-ind/masai_careloop_clinics](https://github.com/coder4web-ind/masai_careloop_clinics)

## Executive Summary
* **Part 1 (Data Foundations):** Ingested 1,020 raw booking rows, cleaning and validating them down to 990 trustworthy records with 16 standardized features.
* **Part 2 (SQL Analysis):** Loaded the cleaned dataset into MySQL and verified that Hadapsar Care Centre (Pune) has the highest no-show rate among clinics with at least 95 appointments at 27.3%.
* **Part 3 (Machine Learning):** Evaluated models via 5-fold cross-validation on training data, selecting Logistic Regression (ROC-AUC ≈ 0.79, test recall ≈ 0.57 at a 0.35 threshold) as the optimal risk scorer.
* **Part 4 (GenAI Review):** Processed call-centre notes using Gemini Flash with Pydantic validation, confirming that 100% of missed appointments categorized under `forgot_or_no_reminder` lacked prior SMS/call reminders.

## Business Recommendation
To minimize empty doctor slots and optimize front-office capacity, CareLoop Clinics should deploy the logistic regression risk model the evening before appointments using a probability threshold of 0.35. Based on our analytical findings, management should focus immediate interventions on high-risk segments: specifically targeting bookings with lead times exceeding 14 days (which suffer a 30.0% to 60.5% no-show rate compared to 16.2% for 1–7 day lead times), ensuring 100% reminder coverage across all branches (as reminders reduce no-show odds by ~69% holding other variables constant), and prioritizing follow-up calls for patients booked at high-volume clinics like Hadapsar Care Centre (27.3% no-show rate).

## Model Selection & Threshold Rationale
* **Selected Model:** Logistic Regression. It achieved the highest cross-validated training ROC-AUC (≈ 0.77) compared to decision trees and random forests, while providing interpretable feature coefficients.
* **Threshold:** 0.35. Evaluated via out-of-fold training probabilities, this threshold maximized F1-score (~0.49) and achieved a higher recall (~0.47 on training, ~0.57 on test). In a clinical setting, recall is critical because a missed warning results in a costly empty slot, whereas an extra verification call costs only a minute of front-desk time.

## Limitations
1. **Observational Data:** The dataset is observational; therefore, estimated effects (such as the impact of reminders) represent statistical associations rather than proven causal relationships.
2. **Random Split Limitations:** The train/test split is random rather than chronological (past-to-future), and individual patients can appear across both training and test partitions.
3. **Sample Size Constraints:** The GenAI analysis relies on an illustrative sample of 10 call-centre notes, which cannot fully capture the diverse root causes of no-shows across all eight clinics at scale.