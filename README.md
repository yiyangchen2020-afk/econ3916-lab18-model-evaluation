# Model Evaluation: The Curve, and What It Cannot Tell You

## Objective

I compared logistic regression and boosted trees on a rare outcome to see how the cost of missed positives and false alarms affects which model and threshold I would choose.

## Methodology

- I simulated 8,000 cases and plotted ROC and precision-recall curves for logistic regression.
- I compared the models’ ROC AUC scores and used 500 paired bootstrap resamples to estimate a 95% confidence interval for the difference.
- I built an expected-cost curve with a missed positive costing ten times as much as a false alarm. I selected the lowest-cost threshold using cross-validated predictions on the training data and evaluated it once on the test set.
- I repeated the threshold selection for both models at cost ratios from 2:1 to 50:1.
- I wrote `roc_point()` to calculate the false positive rate and true positive rate at a given threshold.

## Key Findings

- Positive cases made up 11.8% of my test set. Logistic regression had a ROC AUC of 0.8468 and an average precision of 0.4651. Boosted trees had a ROC AUC of 0.8431.
- I found an AUC difference of +0.0038 for logistic regression minus boosted trees. The 95% bootstrap interval was [-0.0020, +0.0094]. Because it included zero, I found insufficient evidence of an AUC difference.
- At the 10:1 cost ratio, my chosen threshold of 0.0905 had a test cost of 0.4692 per case, compared with 0.9437 at the default threshold of 0.5. At the chosen threshold, the false positive rate was 0.296 and the true positive rate was 0.823.
- Boosted trees had the lower test cost at 5:1. Logistic regression had the lower test cost at every other ratio I tested.
