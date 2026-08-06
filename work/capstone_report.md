# Capstone Report — 

* **Author:** Dharvi Sharma
* **Lane:** Refresh / Content Opportunity Scoring
* **Repo:** https://github.com/dharvi120/flyrank-ml-internship
* **Date:** August 2026

### 1. Problem framing
Our objective is to programmatically identify and rank decaying search content. This acts as a decision-support system for the SEO and Content teams to prioritize editorial refreshes. The unit of analysis is a web page (represented by a row of anonymized telemetry). The output is a risk probability score (0-100) indicating decline risk. A wrong call (False Positive) merely results in reviewing a page that doesn't strictly need it, making this a low-risk, high-reward ML application.

### 2. Data safety
We utilized the anonymized FlyRank dataset. All PII, client names, exact domains, URLs, and raw search queries were deliberately excluded to ensure strict privacy compliance. To prevent label leakage, fields derived directly from the target (like `trend_direction` or future traffic window metrics) were masked during training. 

### 3. Baseline
Our baseline is a deterministic heuristic (`baseline_refresh_score`) built on simple rule-based logic (e.g., declining sessions over 90 days). It serves as a non-leaky, fair comparison point to mathematically prove whether the complex ML architecture actually delivers directional lift over basic human intuition.

### 4. Model / analysis
We engineered an `XGBClassifier` (Gradient Boosted Trees) optimized for imbalanced ranking (`scale_pos_weight=5`). The target is binary: `is_declining_label`. XGBoost was chosen over simpler linear models or LLMs because it natively handles non-linear interactions in tabular performance data and is the industry standard for ranking tasks.

### 5. Evaluation
We implemented a strict **Client Holdout Split** (80/20) grouped by `client_id`. This guarantees that no individual client's data leaks from the training set into the test set, ensuring real-world generalization. We evaluated against the baseline using Area Under the Precision-Recall Curve (AUCPR) and Precision@50, as standard ROC AUC can be misleading on imbalanced datasets.

### 6. Interpretation
The top feature importances driving the model's predictions are strongly tied to visibility and freshness. The model relies heavily on `impressions_90d`, `sessions_90d`, `average_position`, and `content_age_days`. This confirms that severe drops in recent search visibility are the strongest predictors of long-term content decay. 

### 7. Recommendation
The final output is the `refresh_queue.csv` action playbook. FlyRank editors should use this by filtering for rows with a `high` confidence score and the `refresh` or `refresh_and_review_ctr` action labels. This is strictly a decision-support tool; model scores highlight opportunity, but human editorial context is required before initiating a content rewrite.
