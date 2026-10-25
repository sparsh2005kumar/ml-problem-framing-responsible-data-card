# ML Problem-Framing Memo: Customer Churn

## 1. Decision and user
**Decision to support:** Whether to offer a voluntary, helpful customer check-in or support intervention to customers who may be at risk of leaving.

**Decision owner:** A customer-success or support team member, with human review.

**Action window:** Not specified in the supplied dataset. Before real use, define a prediction date, a future churn window (for example, the next 30 days), and a clear rule for when an intervention may occur.

## 2. Prediction target and unit of observation
- **Unit of observation:** One customer record.
- **Target column:** `churned`, encoded as 0 or 1.
- **Label meaning:** The exact definition of churn and its observation window are not provided. Confirm whether churn means cancellation, non-renewal, or another event before modeling.
- **Prediction timing:** Features must be available at the prediction date and must not include information from after the outcome.

## 3. Is machine learning justified?
Not yet for a real operational decision. The supplied file has only 12 rows, no documented provenance, no churn definition, and no documented sampling method. This is sufficient for a learning exercise, but not for reliable generalization. A simple rule and ordinary customer-support process should be retained as the fallback. ML may be reconsidered when there are enough representative, permissioned, time-stamped records and an evaluation plan.

## 4. Features and exclusions
Potential candidate features: `tenure_months`, `support_tickets`, `monthly_spend_inr`, `last_login_days`, and `plan_type`.

Exclude `customer_id` from predictors because it is an identifier and may cause memorization. Confirm that all candidate features are measured before the prediction date. The feature definitions and time windows need documentation.

## 5. Non-ML baseline
Use a majority-class baseline (predict the most common training label) and, separately, a simple business rule agreed with domain experts. A rule might flag recent inactivity for a voluntary check-in, but the threshold must be chosen with stakeholders and tested; it should not be represented as validated by this dataset.

## 6. Evaluation plan
Use a customer- and time-aware evaluation on a larger dataset. Avoid random splitting if records from the same customer or future information could cross the split. Report confusion matrix, precision, recall, F1, balanced accuracy, and precision-recall performance as appropriate. Decide thresholds based on false-positive and false-negative costs. Assess calibration and group-level performance only when there is sufficient data.

## 7. Error costs and safeguards
- **False positive:** Unnecessary outreach, customer annoyance, or inappropriate discounting.
- **False negative:** Missed opportunity to offer useful support.
- **Safeguards:** Human review, optional non-coercive outreach, no service denial or punitive pricing, documented purpose limitation, access controls, data minimization, and a clear rollback plan.

## 8. Monitoring and rollback
Before any pilot, define an owner, review cadence, label-quality checks, drift thresholds, complaint monitoring, and criteria to suspend the model. Roll back to the non-ML rule / standard support workflow if performance degrades, data or labels are unreliable, privacy permission is unclear, or unacceptable disparities emerge.

## 9. Recommendation
**Do not deploy this model.** Use the provided dataset only to demonstrate responsible problem framing and a baseline notebook. Obtain documented data provenance, permission, label definitions, timestamps, and a larger representative dataset before further validation.
