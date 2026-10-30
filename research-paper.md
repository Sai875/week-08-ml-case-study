# Content Page Prioritization Using Machine Learning

## Abstract

Content teams often need to decide which pages deserve human review when search performance signals suggest possible decline. This case study evaluates whether a Logistic Regression model can help prioritize content pages for review compared with a transparent rule-based baseline. The experiment uses an anonymized dataset of 30,000 content records across 32 client groups. The model achieved Precision@50 of 0.620, compared with 0.320 for the rule baseline. Its ROC-AUC was 0.563, indicating modest overall discrimination. The results suggest that the model may support a first-pass review queue, but should not be used as an autonomous decision-maker.

## 1. Introduction

Large content portfolios can contain more pages than editors can manually inspect at once. A prioritization method can help reviewers focus attention on pages showing signs of search-performance decline.

The objective of this project was to test whether a machine-learning ranking approach could help identify pages for human review. The task is content-page prioritization, not sales lead scoring.

## 2. Dataset and Target

The experiment used an anonymized dataset containing 30,000 content records, 56 columns, and 32 client groups. The target represented an observed decline proxy. It was not a verified future outcome.

The evaluated feature set contained five numeric features:

- `days_since_last_update`
- `impressions_90d`
- `ctr`
- `avg_position_model`
- `avg_position_no_data`

The categorical feature was `content_type`.

The target-derived field `trend_pct` was excluded from the final model to prevent leakage.

## 3. Methodology

A Logistic Regression model was trained and compared with a transparent rule-based baseline. The data was split using GroupShuffleSplit with a test size of 20%, random seed 42, and `client_id` as the grouping variable.

The resulting training set contained 23,837 rows across 25 clients. The test set contained 6,163 rows across 7 clients. There was no client overlap between the training and test sets.

Evaluation included Precision@10, Precision@20, Precision@50, Precision@100, and ROC-AUC. This paper highlights Precision@50 and ROC-AUC as compact summary measures.

## 4. Results

| Metric | Result |
|---|---:|
| Model Precision@50 | 0.620 |
| Rule baseline Precision@50 | 0.320 |
| Test base rate | 0.511 |
| Model ROC-AUC | 0.563 |

At the top 50 recommendations, the model's precision was higher than the rule-based baseline. However, the ROC-AUC indicates that the model's overall ability to distinguish positive from negative cases was modest.

A deliberate leakage experiment using `trend_pct` produced artificially inflated results. This reinforced the need to exclude target-derived information from the final feature set.

## 5. Practical Use

The model is best treated as a candidate-ranking aid for creating a first-pass review queue. Human reviewers should validate search visibility, impressions, click-through rate, search intent, content freshness, indexation, and measurement quality before deciding whether to refresh, monitor, or leave a page unchanged.

## 6. Limitations

This experiment uses an anonymized snapshot and an observed decline proxy. It does not establish causality, predict future Google rankings, or prove that content updates will recover traffic.

The test set contains seven held-out client groups, so evidence about generalization to entirely new clients remains limited. The modest ROC-AUC also means the ranking signal should be interpreted cautiously.

## 7. Conclusion

The experiment shows that a simple Logistic Regression model can outperform the selected rule baseline at the evaluated top-50 cutoff in this dataset. The results are not strong enough to justify fully automated decisions. The appropriate use is to support human reviewers with a ranked list, while keeping final content decisions under human judgment.

## Supporting artifacts

- [Executed capstone notebook](https://github.com/Sai875/flyrank-ml-internship/blob/main/work/notebooks/capstone.ipynb)
- [Evaluation metrics JSON](https://github.com/Sai875/flyrank-ml-internship/blob/main/work/outputs/capstone_metrics.json)
- [Precision-at-K chart](https://github.com/Sai875/flyrank-ml-internship/blob/main/work/figures/capstone_precision_at_k.png)
- [Presentation outline](https://sai875.github.io/flyrank-ml-internship/#presentation)
- [Published project page](https://sai875.github.io/flyrank-ml-internship/)
