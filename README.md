# week-08-ml-case-study
# Week 8 Showcase: Content Page Prioritization Using Machine Learning

**Author:** Saideepak Kulkarni  
**Organization:** FlyRank AI  
**Project:** Search Traffic Decay & Content Action Playbook  
**Category:** Applied Machine Learning

## 1. The Problem

Content teams often have more web pages to review than editors can manually inspect.

The challenge was to identify which content pages should receive attention first when there are signals of declining search performance.

I wanted to evaluate whether a machine-learning model could help prioritize these pages more effectively than a transparent rule-based approach.

The project used an anonymized dataset containing 30,000 content records across 32 client groups.

The target represented an observed decline proxy, not a verified future outcome.

## 2. What I Did

I developed a Logistic Regression model and compared it with a transparent rule-based baseline.

My workflow included:

- Examining and preparing the anonymized dataset.
- Defining the target using the observed decline signal.
- Checking for data leakage and excluding target-derived fields.
- Preparing five numeric features and one categorical feature.
- Creating a client-grouped 80/20 train-test split.
- Ensuring zero client overlap between training and testing.
- Training and evaluating Logistic Regression.
- Comparing the model with a rule-based baseline.
- Measuring performance using Precision@10, Precision@20, Precision@50, Precision@100 and ROC-AUC.

The validation used GroupShuffleSplit with random seed 42.

I also performed a deliberate leakage experiment using `trend_pct`. This produced artificially high performance, demonstrating why target-derived information must be excluded from the final model.

## 3. What Came of It

The executed experiment produced these results:

| Evaluation metric | Result |
|---|---:|
| Model Precision@50 | 0.620 |
| Rule baseline Precision@50 | 0.320 |
| Test base rate | 0.511 |
| Model ROC-AUC | 0.563 |

The model performed better than the transparent rule baseline at the evaluated top-K cutoffs.

However, the ROC-AUC indicates that overall discrimination remains modest.

The results support using the model as a candidate ranking aid, not as an autonomous decision-making system.

## 4. Practical Recommendation

The model should be used to create a first-pass review queue.

Human reviewers should inspect:

- Search visibility and impressions.
- Click-through rate.
- Search intent.
- Content freshness.
- Indexation and measurement issues.
- Whether the content needs updating.

The final decision to refresh, monitor, or leave a page unchanged should remain with a human reviewer.

## 5. Limitations and Learning

This project uses an anonymized snapshot and an observed decline proxy.

It does not establish causality, predict future Google rankings, or guarantee that content updates will recover traffic.

The experiment also has limited evidence of generalization to new clients, with only seven held-out client groups.

My main learning was that a useful machine-learning project requires more than a trained model. It requires careful data validation, leakage prevention, meaningful evaluation, and honest communication of limitations.

## 6. Supporting Evidence

**Executed notebook:**  
[Open Capstone Notebook](https://github.com/Sai875/flyrank-ml-internship/blob/main/work/notebooks/capstone.ipynb)

**Evaluation metrics:**  
[View Capstone Metrics JSON](https://github.com/Sai875/flyrank-ml-internship/blob/main/work/outputs/capstone_metrics.json)

**Precision-at-K visualization:**  
[View Evaluation Chart](https://github.com/Sai875/flyrank-ml-internship/blob/main/work/figures/capstone_precision_at_k.png)

**Week 8 presentation outline:**  
[Open Presentation Outline](https://sai875.github.io/flyrank-ml-internship/#presentation)

**Published research paper:**  
[View Research Paper](https://sai875.github.io/flyrank-ml-internship/)

---

**Conclusion**

This project demonstrates how machine learning can support content prioritization through evidence-based ranking, careful validation, and human review.

The objective is not to replace human expertise, but to help reviewers make better-informed decisions about where to focus their attention.
