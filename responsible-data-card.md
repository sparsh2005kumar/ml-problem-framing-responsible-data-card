# Responsible Data Card

## Dataset purpose
**Potential supported decision:** Use churn-risk estimates as a limited, human-reviewed signal to decide which customers may benefit from an optional check-in or support offer.

**Decisions it must not make:** Do not automatically cancel, restrict, penalize, change prices for, or deny service to customers. Do not use the score as the sole basis for consequential decisions. The dataset is too small to justify operational deployment.

## Provenance and permission
- **Provided file:** `customer-churn-training.csv` (12 rows, 7 columns).
- **Source/creator:** Not stated in the supplied files; must be confirmed with the dataset provider.
- **Collection period and method:** Not stated.
- **Consent, license, and permitted users:** Not stated. Confirm permission, purpose limitation, retention, and any contractual or legal restrictions before real-world use.
- **Data type:** Appears to be a small illustrative customer dataset; whether it is synthetic or real is not documented. Treat it as unverified until confirmed.

## Population and representation
- The file contains 12 customer records. It does not document the population, sampling method, geography, customer acquisition channels, or time period.
- Target `churned`: 5 records labelled 1 and 7 labelled 0 (41.7% / 58.3% in this tiny sample).
- Plan types represented: Basic, Standard, and Pro. Group counts are very small; group-level comparisons would be unreliable.
- Missing or underrepresented populations cannot be identified from the supplied file. Do not infer representativeness.

## Features and target
- `customer_id`: Record identifier. Exclude from model features; it may enable memorization and is not a meaningful predictive attribute.
- `tenure_months`: Customer tenure in months.
- `support_tickets`: Number of support tickets; time window and counting rules are undocumented.
- `monthly_spend_inr`: Monthly spend in INR; billing period and calculation rules are undocumented.
- `last_login_days`: Days since last login; reference date and calculation rules are undocumented.
- `plan_type`: Subscription plan category (Basic, Standard, Pro).
- `churned`: Target label (0/1); exact churn definition and observation window are undocumented and must be confirmed.
- **Potential leakage:** Features must be measured strictly before the prediction time and before the churn outcome. Confirm that `support_tickets`, `monthly_spend_inr`, and `last_login_days` are not calculated using post-churn information. `customer_id` must not be used as a predictor.
- **Sensitive proxies:** No explicit sensitive attributes are included, but plan type, spending, engagement, and support usage can correlate with socioeconomic circumstances or access needs. Proxy risk must be assessed in a properly governed dataset.

## Quality checks
- Shape: 12 rows × 7 columns.
- Missing values: 0 in every column.
- Exact duplicate rows: 0.
- Unique customer IDs: 12 of 12.
- Observed numeric ranges: `tenure_months` 1–30; `support_tickets` 0–5; `monthly_spend_inr` ₹499–₹1,499; `last_login_days` 1–30.
- Target balance: 7 non-churn (0), 5 churn (1).
- These checks only describe this file; they do not establish data correctness, representativeness, or absence of outliers.
- For any future evaluation, split by time and/or customer before fitting preprocessing. Never evaluate on training data. This 12-row file is too small for a reliable train/test estimate.

## Risks and safeguards
- **False positive:** A retained customer is flagged as likely to churn; could cause unwanted contact or discounts. Mitigation: respectful, optional outreach and human review.
- **False negative:** A customer who churns is not flagged; support needs may be missed. Mitigation: keep normal support channels available and monitor recall.
- **Bias / representation:** Tiny, undocumented sample can hide group disparities. Mitigation: obtain documented, representative data and evaluate errors across appropriate groups where lawful and ethical.
- **Privacy:** Customer IDs and behavior/spend data may be personal or linkable. Mitigation: minimize fields, restrict access, define retention, and use de-identified data for development.
- **Misuse:** A churn score could be used for punitive pricing or service denial. Mitigation: prohibit such uses and document approved purpose.
- **Automation risk:** A model trained on 12 rows can overfit and fail on new customers. Mitigation: do not deploy; gather more data and validate prospectively.
- **Fallback:** Use a transparent, non-ML rule or normal customer-support workflow, with human review, while the model is unvalidated.
- **Monitoring / rollback:** Pause use if input distributions change, labels become unreliable, errors or complaints rise, or meaningful group disparities appear. Roll back to the documented non-ML fallback.

## Intended evaluation
Before training, define:
- **Baseline:** Majority-class classifier and a simple, documented business rule; compare both with any ML model.
- **Performance:** Confusion matrix, precision, recall, F1, balanced accuracy, and PR-AUC where sample size permits. Report uncertainty and class counts.
- **Threshold / costs:** Select a threshold based on the relative cost of false positives and false negatives; do not assume 0.5 is optimal.
- **Calibration:** Use reliability plots / Brier score only with enough independent data; this dataset is too small for meaningful calibration conclusions.
- **Fairness:** Compare error rates and selection rates across relevant groups only when lawful, appropriate attributes and adequate sample sizes are available. Do not draw conclusions from tiny groups.
- **Error analysis:** Manually inspect false positives and false negatives, data timing, label definition, and possible leakage.
- **Decision:** Treat all notebook results as a demonstration only. No deployment decision is justified by 12 rows.
