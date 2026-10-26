# Portuguese Bank Marketing Prediction 📊

## 📌 Project Overview

This project analyzes bank marketing campaign data and develops machine learning models to predict whether a customer will subscribe to a term deposit. The goal is to help the bank identify potential customers, improve campaign targeting, and use marketing resources more effectively.

The project includes exploratory data analysis (EDA), data preprocessing, feature engineering, comparison of multiple classification algorithms, and threshold tuning to improve the identification of potential subscribers.

## 🎯 Business Objective

Bank marketing campaigns can be expensive and time-consuming, especially when most contacted customers do not subscribe.

This project aims to:

- Identify customer characteristics associated with term-deposit subscriptions.
- Analyze previous campaign outcomes and customer contact patterns.
- Build predictive models to identify potential subscribers.
- Compare machine learning algorithms using relevant classification metrics.
- Recommend a model and decision threshold based on the bank's marketing objectives.

## 📂 Dataset

**Dataset:** `bank-additional-full.csv`

The dataset contains information about bank clients, campaign contacts, previous marketing interactions, and economic indicators.

| Property | Details |
|---|---|
| Original records | 41,188 |
| Records after duplicate removal | 41,176 |
| Original features | 20 input features + target |
| Target variable | `y` |
| Target classes | `yes` / `no` |
| Missing values | No explicit null values; some categorical columns contain `unknown` |

**Target variable:**
- `yes` — The customer subscribed to a term deposit.
- `no` — The customer did not subscribe.

The target distribution is imbalanced: approximately **88.73% no** and **11.27% yes**. Therefore, accuracy alone is not sufficient to evaluate model performance.

## 🛠️ Technologies Used

- Python
- Jupyter Notebook
- Pandas and NumPy
- Matplotlib and Seaborn
- Scikit-learn
- XGBoost

## 🔍 Project Workflow

### 1. Data Cleaning and Preparation

- Loaded and inspected the dataset.
- Identified and removed duplicate records.
- Checked missing values and analyzed `unknown` categorical values.
- Examined numerical and categorical feature distributions.
- Prepared the data for exploratory analysis and model development.

### 2. Exploratory Data Analysis (EDA)

- Analyzed the target variable and class imbalance.
- Studied subscription rates across job, marital status, education, housing loans, and personal loans.
- Examined the impact of contact method, campaign month, and previous campaign outcomes.
- Investigated numerical features using histograms and boxplots.
- Used correlation analysis to understand relationships between numerical variables.

### 3. Feature Engineering

- Created `previously_contacted` from `pdays` to indicate whether the customer had previously been contacted.
- Removed `pdays` after creating the indicator feature.
- Excluded `duration` because call duration is only known after a customer has been contacted. Using it in a pre-call prediction system would introduce data leakage.
- Applied `StandardScaler` to numerical features.
- Applied `OneHotEncoder` to categorical features.
- Used a `ColumnTransformer` and Scikit-learn pipelines to organize preprocessing and model training.

### 4. Machine Learning Models

Seven classification algorithms were evaluated:

1. Logistic Regression
2. Decision Tree
3. Random Forest
4. K-Nearest Neighbors (KNN)
5. Support Vector Machine (SVM)
6. Gradient Boosting
7. XGBoost

### 5. Model Evaluation

The models were assessed using:

- **Accuracy:** Overall proportion of correct predictions.
- **Precision:** Proportion of predicted subscribers who actually subscribed.
- **Recall:** Proportion of actual subscribers correctly identified.
- **F1-score:** Balance between precision and recall.
- **ROC-AUC:** Ability to distinguish between subscribers and non-subscribers.
- **Confusion matrix:** Breakdown of true positives, true negatives, false positives, and false negatives.

Because the positive class represents only about 11.27% of the dataset, recall and F1-score are particularly important for evaluating the bank's ability to identify potential subscribers.

## 📈 Model Comparison

The following results are from the initial model-comparison evaluation.

| Model | Accuracy | Precision (Yes) | Recall (Yes) | F1-score (Yes) | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 83.05% | 0.36 | 0.65 | 0.46 | 0.8002 |
| Decision Tree | 84.76% | 0.39 | 0.63 | 0.48 | 0.7902 |
| Random Forest | 89.36% | 0.55 | 0.29 | 0.38 | 0.7835 |
| KNN | 89.55% | 0.57 | 0.31 | 0.40 | 0.7407 |
| SVM | 85.38% | 0.40 | 0.63 | 0.49 | 0.7852 |
| Gradient Boosting | 89.96% | 0.65 | 0.23 | 0.34 | 0.8097 |
| XGBoost | 89.91% | 0.65 | 0.23 | 0.34 | 0.8107 |

*Precision, recall, and F1-score refer to the positive `yes` class. The initial comparison uses the default classification decision threshold.*

## 🏆 Final Model and Threshold Tuning

XGBoost was further evaluated using a validation-based threshold-tuning workflow. Instead of relying only on the default threshold of 0.50, different thresholds were tested to improve the balance between precision and recall.

**Selected threshold: 0.20**

| Metric | Final evaluation |
|---|---:|
| Accuracy | 87.98% |
| Precision (Yes) | 0.47 |
| Recall (Yes) | 0.59 |
| F1-score (Yes) | 0.53 |
| ROC-AUC | 0.8121 |

At the selected threshold, the model identifies more potential subscribers than the default-threshold XGBoost model, with a corresponding trade-off in precision and overall accuracy.

The threshold should ultimately be selected according to the bank's campaign capacity, the cost of contacting non-subscribers, and the value of identifying additional subscribers.

*Evaluation note: The final threshold was selected using validation data. The reported test results are from the project's existing workflow, in which the test set was also used during earlier model comparisons. A fresh untouched test set or cross-validation would provide a stronger independent estimate of generalization performance.*

## 💡 Key Business Insights

The exploratory analysis highlighted several patterns:

- **Previous campaign outcomes:** Customers with a successful previous campaign outcome showed stronger subscription tendencies.
- **Customer profile:** Subscription rates varied across job categories, education levels, and other demographic groups.
- **Contact method:** Subscription patterns differed between cellular and telephone contacts.
- **Campaign timing:** Subscription rates varied across months, suggesting that campaign timing may be worth investigating further.
- **Repeated contacts:** A higher number of campaign contacts was associated with lower subscription rates in the observed data.
- **Economic indicators:** Several economic variables were strongly correlated, indicating that multicollinearity should be considered when interpreting model coefficients.

These are observed associations, not proof of causation. Business decisions should also consider sample sizes, campaign costs, and customer experience.

## 📣 Marketing Recommendations

1. **Prioritize likely subscribers:** Use model scores to create a prioritized contact list.
2. **Balance reach and cost:** Choose the prediction threshold based on available marketing resources and the cost of unsuccessful contacts.
3. **Learn from previous campaigns:** Analyze previous successful outcomes to improve future targeting.
4. **Review campaign timing:** Test whether different months or contact methods improve subscription rates.
5. **Avoid excessive repeated contact:** Investigate the relationship between repeated contacts and subscription outcomes.
6. **Monitor model performance:** Track precision, recall, conversion rate, and campaign return on investment after deployment.

The model should support marketing decisions rather than automatically exclude customers from campaigns.

## ⚠️ Challenges and Solutions

| Challenge | Approach |
|---|---|
| Imbalanced target classes | Evaluated precision, recall, F1-score, and ROC-AUC instead of relying only on accuracy |
| Categorical `unknown` values | Identified and analyzed them as category values rather than treating them as explicit nulls |
| Special `pdays` value | Created a `previously_contacted` indicator |
| Data leakage risk | Excluded `duration` from pre-call prediction features |
| Different feature scales | Used `StandardScaler` for numerical features |
| High-dimensional categorical data | Used `OneHotEncoder` with unknown-category handling |
| Threshold selection | Compared validation-set thresholds to balance precision and recall |

## 🚀 How to Run the Project

### Prerequisites

Install Python and Jupyter Notebook, then install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost jupyter
```

### Steps

1. Clone or download this repository.
2. Download `bank-additional-full.csv` from the dataset source and place it in the location expected by the notebook.
3. Open the project notebook in Jupyter Notebook or VS Code.
4. Run the cells in order, from data loading and EDA through model evaluation.

**Dataset source:** [UCI Machine Learning Repository — Bank Marketing](https://archive.ics.uci.edu/dataset/222/bank+marketing)

## 🔮 Future Improvements

- Evaluate class-weighting and resampling strategies using training data only.
- Perform cross-validation and evaluate the final model on a fresh holdout set.
- Tune hyperparameters systematically.
- Compare precision-recall curves and evaluate business-specific threshold costs.
- Use feature importance or SHAP to improve model interpretability.
- Deploy the model with a simple Streamlit interface.
- Monitor conversion rates and model drift as new campaign data becomes available.

## 👨‍💻 Author

**Surya Vallala**

- LinkedIn: [linkedin.com/in/vallala-surya](https://www.linkedin.com/in/vallala-surya/)
- GitHub: [github.com/vallalasurya](https://github.com/vallalasurya)

---

*This project demonstrates an end
