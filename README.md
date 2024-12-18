# E-Commerce Customer Churn Prediction

This project builds a machine learning model to predict customer churn for an e-commerce company. The goal is to improve customer retention by identifying high-risk customers and implementing targeted retention strategies.


## Project Overview

- **Data Source**
The data used for this project is publicly available on Kaggle: [E-Commerce Customer Churn Analysis and Prediction](https://www.kaggle.com/datasets/ankitverma2010/ecommerce-customer-churn-analysis-and-prediction).
- **Presentation**: [E-Commerce Churn Story in Tableau](https://public.tableau.com/views/E-COMMERCECHURNDASHBOARD/Story?:language=de-DE&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
- **Dashboard**: [E-Commerce Churn Dashboard in Tableau](https://public.tableau.com/views/E-COMMERCECHURNDASHBOARD/Dashboard1?:language=de-DE&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
- **Model Final**: []()
- **Data with predict**: []()


## Table of Contents

1. [Project Overview](#project-overview)
2. [Data Source](#data-source)
3. [Features](#features)
4. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis)
5. [Preprocessing and Feature Engineering](#preprocessing-and-feature-engineering)
6. [Modeling](#modeling)
7. [Results and Evaluation](#results-and-evaluation)
8. [Conclusion and Recommendations](#conclusion-and-recommendations)

## Project Overview

### Business Context
Customer churn poses a significant challenge to the e-commerce industry, where customer acquisition is costly, and competition is intense. Retaining existing customers is often more profitable than acquiring new ones. The goal of this project is to predict which customers are likely to churn and understand the factors contributing to churn, helping the company make proactive retention efforts.

### Problem Statement
Our company aims to reduce customer churn by focusing on retaining high-risk customers. This model helps identify these customers based on various behavioral and demographic factors.

### Objectives
- **Primary Objective**: Predict customers who are likely to churn with high recall.
- **Secondary Objectives**: Understand factors influencing churn, optimize retention strategies, and manage resource allocation effectively.

### Evaluation Metrics
- **Recall**: Chosen as the primary metric to ensure we minimize false negatives, i.e., the number of actual churners that go undetected.
- **Financial Impact Analysis**: A calculation based on **Customer Acquisition Cost (CAC)** and **Customer Retention Cost (CRC)** allows us to estimate the economic impact of false positives and negatives.

## Data Source

The dataset, sourced from [Kaggle](https://www.kaggle.com/datasets/ankitverma2010/ecommerce-customer-churn-analysis-and-prediction), includes customer data with attributes such as demographics, transaction history, and engagement metrics. 

### Data Overview
- **Rows**: 5,630
- **Columns**: 20 features including the target variable `Churn`

### Target Variable
- `Churn`: A binary feature where **1** indicates a churned customer, and **0** represents a retained customer.

## Features

The dataset includes the following features:

| Feature                  | Description                                                   |
|--------------------------|---------------------------------------------------------------|
| CustomerID               | Unique identifier for each customer                           |
| Tenure                   | Duration in months that a customer has been with the company  |
| PreferredLoginDevice     | Device most frequently used to log into the platform          |
| CityTier                 | Indicates the city category based on purchasing power         |
| WarehouseToHome          | Distance from the warehouse to the customer's home            |
| PreferredPaymentMode     | Most commonly used payment method                             |
| Gender                   | Gender of the customer                                        |
| HourSpendOnApp           | Average number of hours spent on the app per day              |
| NumberOfDeviceRegistered | Number of devices registered by the customer                  |
| PreferedOrderCat         | Customer's preferred order category                           |
| SatisfactionScore        | Satisfaction rating provided by the customer                  |
| MaritalStatus            | Marital status of the customer                                |
| NumberOfAddress          | Number of addresses associated with the customer's account    |
| Complain                 | Indicates if a complaint was made in the last month           |
| OrderAmountHikeFromLastYear | Percentage increase in order amount from last year       |
| CouponUsed               | Number of coupons used in the last month                      |
| OrderCount               | Total orders placed in the last month                         |
| DaySinceLastOrder        | Number of days since the last order                           |
| CashbackAmount           | Average cashback received in the last month                  |

## Exploratory Data Analysis

EDA was conducted to understand feature distributions, detect missing values, and analyze the relationship between features and the target variable, `Churn`.

### Key Observations
1. **Churn Rate**: Approximately 17% of customers are flagged as churned, indicating a relatively low but impactful churn rate.
2. **High-Risk Factors**:
   - Customers with shorter tenure are more likely to churn.
   - Lower satisfaction scores and complaints are associated with higher churn rates.
   - Mobile phone users tend to churn more often than desktop users.
3. **Demographics and Behavior**:
   - Single customers show a higher churn tendency.
   - Users who engage with specific categories, such as mobile phones, have a higher churn risk.

### Correlation Analysis
- **Positive Correlations**:
  - Cashback Amount and Order Count are positively correlated, suggesting that higher cashback may encourage more orders.
- **Negative Correlations**:
  - Tenure has a strong negative correlation with churn, meaning longer-tenured customers are less likely to leave.

## Preprocessing and Feature Engineering

### Data Cleaning
- **Handling Missing Values**: Imputed missing values using median for numerical features and mode for categorical features.
- **Outlier Detection**: Used boxplots and z-scores to detect and handle outliers in features like `OrderCount` and `WarehouseToHome`.

### Encoding Categorical Variables
- Applied one-hot encoding to categorical variables, ensuring that the model could interpret features such as `PreferredPaymentMode` and `MaritalStatus`.

### Feature Transformation
- Standardized numerical features to a common scale and created additional interaction features where relevant, such as `PreferredLoginDevice` with `PreferedOrderCat`.

### Train-Test Split
- **Training Set**: 80% of the dataset, used for model training.
- **Test Set**: 20% of the dataset, held out for final model evaluation.

## Modeling

### Model Selection
The following models were evaluated:
1. **Decision Tree Classifier**
2. **Random Forest Classifier**
3. **Gradient Boosting Classifier**
4. **XGBoost Classifier**
5. **LightGBM Classifier**

### Addressing Class Imbalance
Given the imbalanced nature of the dataset, we used:
- **SMOTE (Synthetic Minority Over-sampling Technique)**: To increase the representation of churned customers.
- **Random Oversampling (ROS)**: A simpler technique that duplicates samples in the minority class.

### Hyperparameter Tuning
Grid search was performed on the top models (XGBoost and LightGBM) to find the best parameters. The **LightGBM Classifier** was selected as the final model, achieving high recall and accuracy after hyperparameter tuning.

## Results and Evaluation

### Performance Metrics
The **LightGBM model** with Random Oversampling achieved the following key metrics:

- **Recall**: The model achieved a recall of **0.91** on the test data, which successfully captures over 90% of churned customers. This high recall aligns with the business goal of minimizing false negatives, thus reducing the number of actual churners missed by the model.
- **Precision**: While recall was prioritized, the model’s precision was also high, indicating that most predicted churns were accurate.
- **Confusion Matrix**:
  - **True Positives (TP)**: Correctly identified churned customers.
  - **False Positives (FP)**: Incorrectly identified retained customers as churned. These result in unnecessary retention efforts.
  - **True Negatives (TN)**: Correctly identified retained customers, helping avoid excess spending on retention.
  - **False Negatives (FN)**: Missed churned customers, representing a potential loss in revenue as no retention efforts are applied to these cases.

### Feature Importance Analysis
Using SHAP and LIME for interpretability, the most influential features for churn prediction included:
- **Tenure**: Longer tenure reduced churn probability.
- **Complain**: Filing a complaint was strongly correlated with churn.
- **Number of Address**: Customers with multiple addresses showed a higher churn tendency.
- **Cashback Amount**: Higher cashback correlated with lower churn.

### Error Analysis
The cost of misclassification was calculated using CAC and CRC:
- **False Negatives** (missed churners) represent lost revenue as no retention efforts are made.
- **False Positives** incur retention expenses for customers not likely to churn.


### Error Analysis and Financial Impact

The **LightGBM model** was chosen as the final model for deployment due to its high recall rate of 0.91, successfully capturing the majority of churned customers. Here, we evaluate the financial implications of different types of errors in the model predictions:

1. **False Negatives (FN)**: 
   - **Definition**: A false negative occurs when the model incorrectly predicts a customer as "not churned" when they are actually at risk of churning.
   - **Impact**: The business loses these customers without any retention intervention, which results in an estimated **$20 loss per customer** (based on the Customer Acquisition Cost, or CAC, needed to replace a lost customer).
   - **Total FN Cost**: With a relatively small false negative count in the confusion matrix, the financial impact remains manageable. However, this cost is critical to monitor, as each missed churner can lead to significant revenue loss.

2. **False Positives (FP)**: 
   - **Definition**: A false positive occurs when the model incorrectly flags a customer as "churned" when they are not actually at risk of leaving.
   - **Impact**: The company allocates unnecessary retention resources to these customers, incurring a **$3 cost per customer** (Customer Retention Cost, CRC).
   - **Total FP Cost**: Although lower than the cost associated with false negatives, these retention costs can add up, especially if the false positive count is high. The business should balance this cost against the benefits of retaining true churners.

3. **True Positives (TP)**: 
   - **Definition**: A true positive occurs when the model correctly identifies a churned customer.
   - **Impact**: Interventions can be applied to retain these customers, which is crucial for reducing churn and increasing revenue. Retaining high-risk customers has a potentially positive financial impact, particularly in preserving customer lifetime value.

4. **True Negatives (TN)**:
   - **Definition**: A true negative is when the model correctly identifies a non-churned customer.
   - **Impact**: Accurately predicting retained customers helps avoid unnecessary spending on retention, thereby optimizing resource allocation.

## Conclusion and Recommendations

### Key Findings
1. **High Recall Model**: The LightGBM model, with a recall score of 0.913, successfully identifies at-risk customers, meeting the business requirement of capturing over 90% of potential churners.
2. **Feature Insights**:
   - **Tenure** and **Complaint** were major predictors of churn.
   - Customers preferring mobile devices and single customers were identified as high-risk segments.

### Business Recommendations
1. **Targeted Retention Campaigns**: Focus on customers with low tenure, low satisfaction scores, and those who have filed complaints. Offer tailored discounts or incentives to retain them.
2. **Enhanced User Engagement**: Increase customer engagement on mobile devices by improving the mobile experience and offering exclusive mobile perks.
3. **Cashback Strategies**: Cashback incentives could be expanded for high-risk customers, as higher cashback correlated with lower churn.
4. **Monitor Customer Complaints**: A dedicated complaints resolution team can help address and mitigate issues for high-risk customers before they churn.

### Limitations and Future Work
- **Model Generalization**: Due to the dataset's relatively small size, model performance might vary on larger, more diverse datasets.
- **Data Imbalance**: While oversampling techniques improved recall, additional methods (e.g., cost-sensitive learning) could be explored to further balance the model's performance.




