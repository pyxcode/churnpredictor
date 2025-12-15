Project Overview: 

This project addresses the problem of user churn prediction for a music streaming service using historical interaction logs. The primary objective is not only to achieve good predictive performance, but to design a model that generalizes reliably despite strong structural biases in the data.
The work focuses on:
* fair behavioral comparison between users,
* mitigation of train–test distribution differences,
* and prediction stability under uncertainty.

Problem Formulation
* Task: Binary classification (churn / no churn)
* Churn definition: A user is considered churned if they visit the Cancellation Confirmation page.
* Data nature: Time-dependent user activity logs.
* Constraint: Training and testing sets differ substantially in user tenure and behavior distributions.

Data Challenges
Three major issues were identified during exploratory analysis:
* Lifetime Bias
Aggregating user behavior over their entire history causes features such as total activity counts to be strongly correlated with account age. This leads to unfair comparisons between long-tenure and recent users and degrades generalization.
* Distribution Shift
Key behavioral features exhibit significantly different distributions between training and testing sets. Adversarial validation confirmed this issue, indicating that many features allowed near-perfect separation of train and test data.
* Label Imbalance Mismatch
While the churn rate in the training set is moderate, black-box evaluation revealed that the testing set is close to balanced. This required a modeling strategy emphasizing robustness and recall rather than accuracy alone.

Methodology: Snapshot Window Construction
To eliminate lifetime bias, all behavioral features are computed within a fixed snapshot window:
* the last 14 days of user activity before the prediction point.
This ensures that all users are compared over the same observation horizon, independently of their total account age.

Feature Engineering
Feature design prioritizes relative, temporal, and stability-based indicators rather than absolute counts.
Features are grouped into four categories:
* Basic and Tenure Features User attributes and account age.
* Activity Trends and Stability Measures of acceleration, deceleration, recency, and regularity.
* Ratio-Based Usage Metrics Indicators of experience quality and interaction efficiency.
* Behavioral Change Indicators Composite features capturing abrupt slowdowns, reduced exploration, or sustained negative signals.
This structure allows the model to focus on behavioral evolution instead of cumulative usage.

Feature Selection and Drift Control
To reduce overfitting and improve generalization:
* highly correlated features were removed,
* adversarial validation was used to detect drift-sensitive variables,
* features strongly tied to train–test separation were excluded.
Although this reduced training fit, it significantly improved model stability.

Modeling Strategy
Two complementary models were used:
* Gradient-boosted decision trees with restricted depth,
* Logistic regression as a linear baseline.
Tree depth was deliberately limited to prevent learning dataset-specific artifacts.

Ensemble Design
The final prediction combines:
* multiple model runs with different random seeds (variance reduction),
* a weighted average of tree-based and linear models.
This ensemble favors consistency and robustness over single-model optimization.

Experimental Results
* Snapshot-based features significantly improved performance compared to full-history aggregation.
* Removing drift-prone features improved generalization on unseen data.
* Behavioral change indicators ranked among the most informative features.
* Ensemble predictions showed lower variance and improved stability.

Conclusion

This study demonstrates that careful data construction and bias-aware feature design are essential for churn prediction in real-world settings. By enforcing temporal fairness, controlling distribution shift, and prioritizing robust modeling choices, the proposed approach achieves reliable performance despite substantial dataset imperfections.
