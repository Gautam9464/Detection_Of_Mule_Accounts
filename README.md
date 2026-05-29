🛡️ Enterprise UPI Fraud & Mule Account Detection Platform
An end-to-end, production-ready Machine Learning Microservice designed to detect organized money mule accounts and financial fraud within highly imbalanced (111:1) UPI transaction data.Unlike standard competitive notebooks, this project implements a Zero-Trust Hybrid Architecture, combining an automated target-leakage guard, a 5-fold Out-Of-Fold (OOF) deep stacking ensemble, and a live MLOps Concept Drift monitor.

📈 Business Impact & Key Results
In financial fraud, False Positives (freezing innocent customers' accounts) are the most expensive operational failure. This model was strictly calibrated to optimize for customer retention.
False Positive Rate (FPR): 0.0000% (Zero legitimate accounts incorrectly flagged).
Precision on Legitimate Accounts: 1.0000
Test Set F1-Score: 0.8000
10-Fold Cross-Validation AUC: 0.9785 ± 0.0198
The system successfully catches 66%+ of sophisticated fraud rings without triggering a single false alarm on legitimate customers, making it highly deployable for strict financial compliance environments.

🧠 Core Architecture & ML Pipeline
1. Automated Target-Leak Guard
Real-world fraud data is notoriously dirty, often containing post-transaction investigator flags that act as "cheat codes." The pipeline implements a programmatic Leakage Guard that computes the vectorized absolute correlation of all features, automatically banning linear leaks (> 0.75 correlation) and utilizing SHAP to ban non-linear leaks.

2. Imbalance Handling (111:1 Ratio)
Tomek Links: Applied to the majority class to clean overlapping boundary noise.
SMOTE: Applied to synthesize the minority class, ensuring the tree models have enough signal to learn complex mule behaviors without overfitting.

3. The 7-Model Hybrid Stacking Ensemble
A 5-Fold OOF architecture ensures zero data leakage during meta-learning.

Base Models: XGBoost, LightGBM, CatBoost (utilizing Oblivious Trees), Random Forest, Extra Trees, and a Deep Neural Network (128→64→32 MLP).
Meta-Learner: A Logistic Regression model learns the mathematically optimal fusion weights of the base models' predictions.
Hybrid Fusion: The supervised ensemble output dictates 75% of the final risk score, while an unsupervised Isolation Forest contributes 25% to catch novel, "zero-day" anomalies.

4. Bayesian Feature Pruning (Optuna & SHAP)
Feature dimensionality was distilled from 3,900+ raw columns down to the top 102 predictors using a combination of RandomForest SelectFromModel and XGBoost TreeExplainer SHAP pruning.
Optuna was utilized for hyperparameter tuning across 50 trials for gradient boosters.

💻 Tech Stack
Machine Learning: scikit-learn, xgboost, lightgbm, catboost, imbalanced-learn
Mathematical Optimization & Explainability: scipy (Nelder-Mead), optuna, shap
Backend API: fastapi, uvicorn, pydantic
Frontend UI & Graph Theory: streamlit, networkx, pyvis

🚀 MLOps Dashboard & Features
The project ships with a fully decoupled FastAPI backend and Streamlit frontend, designed for live fraud analyst review.

⚡ Real-Time Scoring & SHAP Explainability:
Every API payload returns a 0-100 Risk Score, alongside real-time SHAP attribution (e.g., "Feature 'F3898' increased risk profile"), complying with AI explainability regulations.

🕸️ Mule Ring Isolation (Network Graphs):
Utilizes high-dimensional Cosine-Similarity matrices to cluster visually identical bot behaviors, outputting interactive HTML network graphs to expose latent Mule Rings.

📉 Concept Drift Tripwire:
Computes statistical Z-score deviations of incoming API payloads against the original training distribution. If an adversarial attack or "Concept Drift" occurs, the system automatically triggers an MLOps retraining alert.
Here are the updated Installation & Prerequisites and Repository Structure sections tailored exactly to the files shown in your screenshot. You can swap these directly into your README.md file.

🛠️ Installation & Prerequisites
To replicate this environment and run the main pipeline notebook (BOI_IITH.ipynb), ensure you have Python 3.10+ installed.

1. Clone the repository:

Bash
```text
git clone https://github.com/Gautam9464/Detection_Of_Mule_Accounts
cd Detection_Of_Mule_Accounts
```

2. Install the required Data Science and ML libraries:
You can install all required dependencies using pip. This includes the core modeling libraries, the imbalanced-data handlers, and the hyperparameter tuning/explainability frameworks:

Bash
```text
pip install pandas numpy scikit-learn imbalanced-learn matplotlib seaborn xgboost lightgbm catboost scipy optuna shap joblib jupyter
```
(Note: If you intend to run the live UI and API deployment blocks at the end of the notebook, you will also need to install fastapi, uvicorn, pydantic, streamlit, networkx, and pyvis).

📁 Repository Structure
Based on your project files, here is the exact breakdown of the repository and the generated ML artifacts:

```text
├── BOI_IITH.ipynb                 # Core Jupyter/Colab Notebook containing the end-to-end ML pipeline & deployment code
├── README.md                      # Project documentation
├── cross_validation.png           # Visualization of the 10-Fold CV AUC scores across folds
├── evaluation_dashboard.png       # 6-panel evaluation dashboard (ROC, Precision-Recall, Confusion Matrix, Distribution, etc.)
├── metrics.json                   # Serialized JSON file containing final test metrics (F1, AUC, Recall, FPR, etc.)
├── risk_tier_report.csv           # Business output grouping test-set accounts into High Risk, Medium Risk, etc.
├── shap_beeswarm.png              # Visual SHAP summary plot demonstrating the impact of the top features
└── shap_importance.csv            # Tabular export of the mean absolute SHAP values for the pruned feature space
```

🤝 Contributing & License
Contributions, issues, and feature requests are welcome! Feel free to check the issues page.
Built for high-stakes financial hackathons and enterprise risk-management portfolios.
