# Artea: A/B Testing and Targeted Coupon Strategy

## Overview

Analysis of a 5,000-user A/B experiment conducted by Artea, an online retailer of handmade clothing and accessories. Half of recently active non-purchasing users were randomly assigned a 20% discount coupon. This project analyzes the causal effect of the coupon, identifies which users respond to it, and builds a targeting model to maximize campaign efficiency on a new pool of 6,000 users.

**Key result:** A T-learner uplift model reduced coupon volume by 83.9% while capturing 73.6% of incremental conversions, improving net profit per coupon from USD 1.00 to USD 4.60, a 4.6x efficiency gain.

## Methods

- Two-proportion z-test and Welch t-test for A/B test analysis
- Bonferroni correction for multiple subgroup comparisons
- T-learner meta-learner uplift model using logistic regression
- Cross-validated AUC for model evaluation (AUC = 0.80)
- Qini curve analysis for uplift model comparison
- Diagnostic comparison against X-learner and causal forest via 5-fold cross-validated Qini coefficients

## Key Findings

1. The coupon increased conversion from 10.7% to 12.4% (p = 0.060), falling just short of statistical significance under a blanket send strategy.

2. Revenue per user was unchanged between treatment and control (p = 0.718), consistent with coupon cannibalization, where discounts erode margin on purchases that would have occurred regardless.

3. Facebook-acquired users showed the strongest coupon response (5.0 pp lift) and cart abandoners showed 4.6 pp lift, both approaching but not crossing the Bonferroni-adjusted significance threshold.

4. Heavy prior purchasers (6 or more purchases) showed negative coupon lift, confirming the Sure Thing problem: frequent buyers do not need a discount.

5. Targeting the top 16.1% of users by uplift score captures 73.6% of available incremental conversions at 4.6x the profit efficiency of a blanket campaign.

## Uplift Model

The T-learner trains two separate logistic regression models. Model T is trained on the treatment group and predicts purchase probability with a coupon. Model C is trained on the control group and predicts purchase probability without a coupon. The uplift score is P(purchase | coupon) minus P(purchase | no coupon).

Users are segmented into Persuadables, Sure Things, Sleeping Dogs, and Lost Causes based on uplift score and baseline purchase probability. Only Persuadables are targeted in the recommended campaign strategy.

A diagnostic comparison showed the T-learner (cross-validated Qini: 6.70) performs comparably to the X-learner (6.20) and tuned causal forest (7.15), with differences falling within one standard deviation of noise across folds. The T-learner is preferred for its interpretability.

## Setup

python -m venv venv
.\\venv\\Scripts\\Activate.ps1
pip install -r requirements.txt
jupyter lab

## Data

Source: Harvard Business School case 9-521-021, Artea: Designing Targeting Strategies (Ascarza and Israeli, 2020). The dataset is not included in this repository. Place AB Data.xlsx in the project root before running the notebook.

## References

Kunzel, S., Sekhon, J., Bickel, P., Yu, B. (2019). Metalearners for estimating heterogeneous treatment effects using machine learning. Proceedings of the National Academy of Sciences.
