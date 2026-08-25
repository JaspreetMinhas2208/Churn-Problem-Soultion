# Customer Retention Analysis & Predictive Modeling

---

# Project Objective

Predict whether customers will extend their contracts and identify key drivers of retention. More importantly, Can churn actually be predicted reliably using this dataset.

---

# Dataset Overview

The data used in this study was collected by wlw (Wer liefert was), a leading B2B online marketplace in the DACH region (Germany, Austria, and Switzerland). The platform connects approximately 400,000 monthly buyers with over 600,000 suppliers, manufacturers, and service providers, offering access to hundreds of thousands of products and services. As a widely used platform for professional procurement, wlw serves as a key intermediary for business relationships, making its customer base a valuable source for analyzing factors such as customer behavior and contract retention.

###  Feature Groups

| Category | Variables |
|--------|----------|
|  Target | `Contract_extended` |
|  Customer | `cust_type`, `industry`, `employee_count` |
|  Engagement | `logins`, `traffic`, `customer contacts` |
|  Value | `clv`, `CPC` |
|  Product | `core_product`|

---

#  Data Preprocessing

During the data preprocessing stage, all categorical variables were converted into factors to facilitate easier analysis. Missing values were carefully handled rather than removed indiscriminately. For instance, the variable received_cancel_date contained approximately 71% missing values and was therefore excluded from the dataset. Remaining missing values were minimal (<2%) and were handled via listwise deletion. 
Additionally, customer_id was removed as it did not provide any meaningful contribution to the analysis. The industry variable was also dropped due to its high granularity and large number of categories, with the broader industry_type variable retained instead for better interpretability. Finally, Contract_not_cancelled was excluded because it closely overlapped with the target variable Contract_extended, which could lead to redundancy and potential data leakage.

---

#  Exploratory Data Analysis (EDA)

### Key Findings

Exploratory Data Analysis revealed that customer engagement variables, such as login frequency and interaction levels, show strong differences between retained and non-retained customers. Customers with higher activity levels are significantly more likely to extend their contracts. Additionally, product tier and customer value metrics such as CLV also exhibit notable differences, indicating that both engagement and value play a key role in retention.

It was also revealed that the majority of customers tend to renew their contracts, with approximately 80% classified as renewals and only around 20% as churn cases. This indicates a significant class imbalance within the dataset. Such an imbalance is important to consider, as it can bias predictive models toward the majority class, leading to overly optimistic accuracy while failing to effectively identify customers who are likely to churn.

---

# Phase 1 — Global Modeling

### Models Used:
- Logistic Regression  
- Decision Tree  
- Random Forest  

---

##  Results

| Model | Accuracy | Sensitivity |
|------|---------|------------|
| Logistic | ~80% |  ~6% |        
| Tree | ~80% |  0% |
| RF | ~80% |  Very low |

# Problem Identified

The initial models achieved high overall accuracy; however, they provided little to no business value. A key reason for this limitation is the pronounced class imbalance between customers who renew their contracts and those who do not. As a result, the models tend to predict the majority class which is contract renewals for most observations. While this inflates accuracy metrics, it fails to capture and identify the factors driving customer churn. Consequently, the insights generated are misleading and do not support actionable decision-making or service improvement. Overall, despite appearing statistically strong, these models are not practically useful.

# Phase 3 — Customer Segmentation

A decision was taken at this point, to divide the customer base between new and old customers, since the retention rate between was different i.e. 80% in existing customers while 60% in new customers. It was also understood from a logical reasoning stand point that the requirements for the new customers is different from the already existing customers. New customers are likely more prone to churn then existing ones as highlighted by the evidence above. Segmenting the data in this way enables more targeted analysis and improves the potential to derive meaningful, actionable insights.

##  Model Results

###  Logistic Regression
- Accuracy: **66%**
- Sensitivity: **38%**
- Balanced Accuracy: **59%**
- AUC: 59%
---

###  Decision Tree
- Accuracy: 62%  
- Sensitivity: 30%  
- AUC: 55%
---

###  Random Forest
- Accuracy: 64%  
- Sensitivity: 16%  
- AUC: 54%
---

# Model Comparison

| Model | Accuracy | Sensitivity | Verdict |
|------|---------|------------|--------|
| Logistic | 66% | **38%** |  Best |
| Tree | 62% | 30% |  Moderate |
| RF | 64% | 16% |  Weak |

The logistic regression model demonstrated the strongest performance in detecting churn, correctly identifying approximately 38% of churn cases compared to the other models. However, none of the predictors were statistically significant (p > 0.05), meaning no variable could be conclusively identified as influencing whether a customer extends their contract. As a result, while the model shows some predictive capability, it does not provide clear, reliable insights into the underlying drivers of customer retention.

---


#  Final Insights

Customer churn appears to be a complex and multi-dimensional phenomenon, influenced by a combination of factors rather than a single dominant driver. This complexity is further reflected in the behavior of new customers, who tend to be more unpredictable compared to existing ones. Additionally, the dataset seems to lack critical behavioral signals that could better explain customer decisions, limiting the ability to build models that provide deeper and more actionable insights. 

---

##  Core Conclusion

Customer retention cannot be reliably predicted using the current set of features in the dataset. To uncover the true drivers behind contract extensions, a revised approach is require which incorporates additional, more informative variables and potentially alternative modeling strategies.

---
# Author

Jaspreet Singh Minhas
