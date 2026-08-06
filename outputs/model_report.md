# FlyRank Refresh Opportunity Model Report

This report is generated from the bundled anonymized starter dataset (`data/raw/content_refresh_anonymized.csv`).
The model ranks existing content for refresh review. It does not use titles, URLs, client names, domains, or keywords.

## Data

- Rows scored: 30,000
- Declining-label rows: 16,262
- Declining-label rate: 0.542
- Split strategy used for validation: client_holdout
- Target: `is_declining_label`

## Model Comparison

Best model: `xgboost_advanced` selected by `precision_at_50`.

| Model | ROC AUC | Avg precision | Precision@50 | Recall | F1 |
|---|---:|---:|---:|---:|---:|
| decision_tree | 0.742 | 0.575 | 0.620 | 0.716 | 0.634 |
| logistic_regression | 0.700 | 0.522 | 0.400 | 0.567 | 0.566 |
| random_forest | 0.747 | 0.610 | 0.680 | 0.741 | 0.638 |
| xgboost_advanced | 0.778 | 0.692 | 0.920 | 0.950 | 0.628 |
| baseline_rules | 0.627 | 0.468 | 0.240 | - | - |

## Final Queue

- High-confidence items: 4,331
- Medium-confidence items: 10,669
- Low-confidence items: 15,000
- `refresh` items: 13,226
- `refresh_and_review_ctr` items: 9,145
- `monitor` items: 4,862
- `refresh_and_review_engagement` items: 2,685
- `expand_and_refresh` items: 82

## Top Features

- `age_tier_365+`: 0.1608
- `days_with_impressions`: 0.0691
- `impression_tier_low`: 0.0479
- `position_tier_top_3`: 0.0454
- `position_tier_deep`: 0.0272
- `avg_position`: 0.0257
- `content_age_days`: 0.0240
- `content_type_comparison article`: 0.0234
- `word_count_tier_1000-2000`: 0.0232
- `log_impressions_90d`: 0.0206

## Top 10 Queue Preview

| Rank | Score | Model probability | Action | Reasons | Impressions | Sessions | Trend |
|---:|---:|---:|---|---|---:|---:|---|
| 1 | 96.8 | 0.969 | refresh_and_review_ctr | declining_with_demand, low_ctr_visible_page, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, ctr_review_candidate, engagement_review_candidate | 24260 | 44 | down |
| 2 | 96.7 | 0.983 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 26287 | 685 | down |
| 3 | 96.5 | 0.969 | refresh_and_review_ctr | declining_with_demand, low_ctr_visible_page, model_decline_risk, visible_model_opportunity, ctr_review_candidate | 34100 | 18 | down |
| 4 | 96.5 | 0.979 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 12029 | 119 | down |
| 5 | 96.3 | 0.963 | refresh_and_review_ctr | declining_with_demand, low_ctr_visible_page, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, ctr_review_candidate, engagement_review_candidate | 43650 | 69 | down |
| 6 | 95.9 | 0.977 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 9594 | 89 | down |
| 7 | 95.7 | 0.976 | refresh_and_review_ctr | declining_with_demand, page_one_decay_risk, low_ctr_visible_page, model_decline_risk, visible_model_opportunity, ctr_review_candidate | 9848 | 8 | down |
| 8 | 95.6 | 0.958 | refresh_and_review_ctr | declining_with_demand, low_ctr_visible_page, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, ctr_review_candidate, engagement_review_candidate | 32038 | 72 | down |
| 9 | 95.5 | 0.980 | refresh_and_review_ctr | declining_with_demand, low_ctr_visible_page, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, ctr_review_candidate, engagement_review_candidate | 12834 | 66 | down |
| 10 | 95.5 | 0.969 | refresh_and_review_ctr | declining_with_demand, page_one_decay_risk, low_ctr_visible_page, model_decline_risk, visible_model_opportunity, ctr_review_candidate | 12331 | 8 | down |

## Generated Files

- `outputs/refresh_queue.csv`
- `outputs/model_results.json`
- `outputs/summary.json`
- `outputs/charts/action_mix.svg`
- `outputs/charts/confidence_mix.svg`
- `outputs/charts/top_reason_codes.svg`
- `outputs/charts/top_feature_importance.svg`
- `outputs/charts/trend_distribution.svg`

## Practical Use

Use the ranked queue as a reviewer aid, not as an automatic publishing decision.
The safest first production use is to inspect high-confidence rows, verify the page manually, and compare the recommendation against editorial context.
