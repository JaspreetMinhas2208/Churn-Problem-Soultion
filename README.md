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

### Product type
<img width="1920" height="992" alt="1" src="https://github.com/user-attachments/assets/3c105fd9-91fb-4d00-8f11-0a69a8ab7d05" />
Most products have a high proportion of “true” (contract extended) with some variation between basic and premium products. It can be interpreted that product type does influence retention slightly, but not strongly. The differences are visible but not dramatic and no product clearly separates churn vs retention.

### Traffic on Customer contact (average)
<img width="1920" height="992" alt="2" src="https://github.com/user-attachments/assets/85bb3dad-d4e7-4cb3-b6ff-db306cbc83d5" />
Most values are low and extremely right-skewed with some outliers. There is also high overlap between values so separating the classes is difficult. Overall, this factor is highly noisy and not useful in analysis. 


### How often did the company sign in
<img width="1920" height="992" alt="3" src="https://github.com/user-attachments/assets/178a027d-0b17-4629-a883-353344fad498" />
Most values are low and extremely right-skewed. There are more higher outliers in the case of customer's who extended the contract. However the effect is weak and there is still a lot of overlap.


### Cost per Customer
<img width="1920" height="992" alt="8" src="https://github.com/user-attachments/assets/cf890d42-9974-44a1-b803-a3416cb1309a" />
Both groups (true vs false) have very similar distributions i.e heavy right skew and lots of outliers. It can be interpreted that CPC does not differentiate churn vs retention because customers who stay and leave have roughly similar costs. 


### Number of employees in the company:
<img width="1240" height="797" alt="20" src="https://github.com/user-attachments/assets/7117d8fc-01a5-43c7-a2ee-62933fca965e" />
Retention rates are fairly similar across all company sizes with some variation. It can be confidently said that company size has only a weak relationship with retention as there are no clear trend like “bigger company = higher retention”.


### Number of picture uploaded by customer on website:
<img width="1257" height="842" alt="19" src="https://github.com/user-attachments/assets/38f8c21e-e449-4bca-bea5-962b0170f4bd" />
Retention rates are quite consistent across categories with slight fluctuations. For this case as well, it can be interpreted that number of pictures does not influence retention. The variable has minimum predictive power. 

### Key Findings

The exploratory analysis revealed that most variables exhibit substantial overlap between customers who extended their contracts and those who did not, limiting their usefulness as strong predictors of churn. Customers with higher interaction levels were more likely to extend their contracts, suggesting that engagement plays a role in customer loyalty. However, the significant overlap between groups indicates that this effect is not strong enough to independently predict outcomes. structural variables such as employee count and number of pictures online showed minimal variation in retention rates across categories. This indicates that company size and content volume do not meaningfully influence contract extension decisions. Additionally, cost-related variables such as CPC demonstrated no clear differentiation between retained and churned customers.

Across most numerical features, the data exhibited strong right-skewness and the presence of extreme outliers, further complicating the identification of clear patterns. The high degree of overlap between classes suggests that the available features do not capture the full complexity of customer churn behavior.

It was also revealed that the majority of customers tend to renew their contracts, with approximately 80% classified as renewals and only around 20% as churn cases. This indicates a significant class imbalance within the dataset. Such an imbalance is important to consider, as it can bias predictive models toward the majority class, leading to overly optimistic accuracy while failing to effectively identify customers who are likely to churn.

Overall, the analysis indicates that while engagement-related metrics provide some insight into retention, customer churn appears to be driven by additional unobserved factors, which are not included in the dataset. This limitation helps explain the relatively weak performance of the predictive models developed in this study later on.

---

# Phase 1 — Global Modeling

### Models Used:
- Logistic Regression  
- Decision Tree  

---

##  Results

| Model | Accuracy | Sensitivity |
|------|---------|------------|
| Logistic | ~80% |  ~6% |        
| Tree | ~80% |  0% |

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
