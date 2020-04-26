
# Predictig Credit Card Users' Repayment Behavior
final_project(～￣▽￣)～  
## 0. Group Infomation
Student ID  | Student Name |  Github ID 
 :-: | :-: | :-:
1901212571| Keya Deng| [dengkeya](https://github.com/dengkeya)
1901212561| Jizhong Cao| [1901212561](https://github.com/1901212561)
1901212657| Xuan Yang | [xuanyang274](https://github.com/xuanyang274)
1901212615| Liu Xin | [rebeccaliu922](https://github.com/rebeccaliu922)


## 1. Background

Credit card is an important part of personal banking business. All major banks are using customer personal information to establish credit scoring systems. The bank has to decide what kind of information to be collected, how to identify information authenticity and how to use the information to evaluate customer's credit. Based on credit card information of Taiwan in 2005 (data resource: http://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients), this research aims to predict customers’ repayment behavior with multiple machine learning models and explore the key factors affecting customer credit. 
## 2. Data Description
This dataset uses a binary variable, Default Payment (Yes = 1, No = 0), as the response variable. It contains 30,000 samples with no missing data. There are 23 features in total, which can be divided into five categories. 

<table>
	<tr>
	    <th>Categories</th>
	    <th>Features</th>
	    <th>Details</th>  
	</tr >
 <tr>
	    <td >Credit Card Information</td>
	    <td>Credit Limit</td>
	    <td>Amount of the given credit (NT dollar)</td>
	</tr>
	<tr >
	    <td rowspan="4">Indentity Information</td>
	    <td>Gender</td>
	    <td>1 = male; 2 = female</td>
	</tr>
	<tr>
	    <td>Education</td>
	    <td>1 = graduate school; 2 = university; 3 = high school; 4 = others</td>
	</tr>
	<tr>
	    <td>Marriage</td>
	    <td>1 = married; 2 = single; 3 = others</td>
	</tr>
	<tr>
	    <td>Age</td>
	    <td>Years</td>
	</tr>
	<tr>
	    <td >Historical Credit Record</td>
	    <td >PAY_0, ..., PAY_6 </td>
	    <td >History of past payment :<br /> -N = Pay N months early; <br />0 = Pay on time<br />N = Payment delay for N month<br />N = 1,2, ..., 6 </td>
	</tr>
	<tr>
	    <td >Historical Bill Amount</td>
	    <td >BILL_AMT1,..., BILL_AMT6 </td>
	    <td >Amount of bill statement (NT dollar)</td>
	</tr>
	<tr>
	    <td >Historical Payment Amount</td>
	    <td >PAY_AMT1,..., PAY_AMT6 </td>
	    <td >Amount of Previous Payment (NT dollar)</td>
	</tr>
</table>  


## 3. Our Aim

* The model is built based on the existing basic population attribute data, historical credit history, historical billing data and historical repayment data. According to the model, it can predict whether the credit card holder will repay the money on time next month. Essentially, it is a typical dichotomous problem.

* Determine which variables are strong predictors of whether a customer is overdue, and compare the differences of prediction effects between different data processing methods and mathematical models.

## 4. Modeling

### Step 1. Data Visualization and Data Cleaning

Befor using any model to predict, the first step is always explore what data we have. So we visualized the features we have and clean the original data. The related code is [Part1_Data_Cleaning.ipynb](https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Data_Cleaning/Part1_Data_Cleaning.ipynb).

#### 1.1 Data Visualization

To understand the distribution better, firstly we visualize *Limit_Bal, Sex, Education, Marriage, Age* these five features.  As is shown in the following figure, the balance limit varies from some thousand NTD to as much as a million. There are more female customers. The majority have a bachelor degree. 

<div align=center><img width="500" height="600" src="https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/charts/DataVisualization.jpg"/></div>


#### 1.2 Data Cleaning

Also we noticed some outliers. For example, education 4,5,6 and marriage statues, 0,3. They don’t have a realistic meaning and they only account for a very small proportion. Therefore we choose to delete them. Also, we delete sample older than 70 years.

Considering some continuous variables such as balance limit and age do not make much difference from the discrete intervals. That is to say, the behaviors of customers in age 23 may not have too much difference from the behaviors of age 24. In order to avoid overfitting, we choose to discrete these two features. We set the bar width 50 thousand for balance limit and 10 years for age.

 The following chart shows how the features look after data cleaning.

<div align=center><img width="500" height="600" src="https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/charts/AfterDataCleaning.jpg"/></div>

### Step 2. Features Selection

After data visualizing and cleaning, we want to select some features correlated to the real problem background.  The related code is [Part2_Feature_Selection.ipynb](https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Feature_Selection/Part2_Feature_Selection.ipynb).

#### 2.1 Historical Credit Record

As for the faeture Historical Credit Record (PAY_0, ..., PAY_6), if it’s a negative number, it means users pay early; 0 means users pay for the month bill on time. If positive, it means delayed payment. From the following point plot we can see that even if the bill is payed with 1 month delay, the risk of default is pretty low. However the relation between default and greater than or equal to 2 month delay pay is relatively high. Therefore, we tansform the original features into new binary variables: if PAY_i is smaller than 2, we divide it into *Paid*; otherwise *Delay*.

<div align=center><img width="600" height="300" src="https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/charts/DefaultWithPaymentDelay.png"/></div>

####  2.2 Historical Bill Amount

Based on the understanding of credit card business, we think the figures of Historical Bill Amount (*BILL_AMT1*,..., *BILL_AMT6*) themselves contains not so much information. However,  the ratio of last month's amount of bill statement to the amount of the given credit can influence this month's default probability. So we take this ratio into consideration and create a new feature: *Limit_Usage*. Since we are going to predict October's default probability, so we only take September's ratio.

#### 2.3 Historical Payment Amount

As for the feature Historical Payment Amount (*PAY_AMT1*,..., *PAY_AMT6*), we think they are highly correlated with the default status. The more stable of previous  repayment, the less likely default in the future owing to the consistent repayment behavior. In order to describe the fluctuation among different level, we use the variation coefficient instead of standard deviation. 

The intermediate result of data cleaning and selection is saved in [Feature_Selection.csv](https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Feature_Selection/Feature_Selection.csv).

### Step 3. Variable Correlation Analysis, Standardization & Dumb Coding

#### 3.1 Variable Correlation Analysis

The correlation analysis of single variable mainly analyzes the correlation between each independent variable and the dependent variable, and takes the variables with significant correlation as the input variable set of the model.
Considering that the dependent variable is a fixed-type binary variable, IV and WOE were calculated for the type independent variable, and the independence test of Contingency Analysis was conducted.  For the numerical independent variable, the independence test of Variance Analysis was conducted.

##### (1) Contingency Analysis  & IV-WOE Analysis with Categorical Variables

- **Contingency Analysis**

For categorical variables, we use Contingency Analysis to justify the relation with default condition. 

When p-value < 0.05,  there is a significant relation between the variable and y. 

- **IV-WOE Analysis**


IV is information value，and WOE is weight of evidence. They can be used to measure the predictive power of independent variables, such as Information Gain, Gini Coefficient and so on. Since this was mentioned in Professor's class and we had never use it, we decided to use IV-WOE to measure the predictive power of variables. 

This is the calculation result:

| categorical variable | IV       | WOE       | p-value       |
| -------------------- | -------- | --------- | ------------- |
| MARRIAGE             | 0.005798 | 0.008859  | 8.582062e-08  |
| SEX                  | 0.008381 | 0.033757  | 3.148419e-11  |
| EDUCATION            | 0.019753 | 0.050561  | 5.176524e-22  |
| LIMIT_BAL_GROUP      | 0.156865 | -2.983032 | 1.260074e-161 |
| PAY_6_TEST           | 0.281866 | 1.127477  | 0.000000e+00  |
| PAY_5_TEST           | 0.329529 | 1.249417  | 0.000000e+00  |
| PAY_4_TEST           | 0.348062 | 1.123734  | 0.000000e+00  |
| PAY_3_TEST           | 0.407420 | 1.028289  | 0.000000e+00  |
| PAY_2_TEST           | 0.546607 | 1.113845  | 0.000000e+00  |
| PAY_0_TEST           | 0.716262 | 1.750558  | 0.000000e+00  |

Through Contingency Analysis, there is no indication that we need to eliminate variables. 

Here we select variables according to IV. The smaller IV is, the weaker the prediction power is. We will first consider deleting it. According to the calculation, we will delete MARRIAGE and SEX in dataset 4.

##### (2) one-way ANOVA with Numerical Variables

For numerical variables, we use one-way ANOVA to justify the relation with default condition.

| numerical variable | p_value      |
| ------------------ | ------------ |
| PAY_AMT1           | 1.771477e-39 |
| PAY_AMT2           | 4.625058e-25 |
| PAY_AMT4           | 7.646590e-23 |
| PAY_AMT3           | 1.156218e-22 |
| PAY_AMT5           | 1.510917e-21 |
| PAY_AMT6           | 1.519015e-20 |
| BILL_AMT1          | 1.125713e-03 |
| BILL_AMT2          | 1.977091e-02 |
| BILL_AMT3          | 2.030948e-02 |
| BILL_AMT4          | 1.032026e-01 |
| BILL_AMT5          | 2.936431e-01 |
| BILL_AMT6          | 4.064043e-01 |

Under the significance level of 0.05, we kick out BILL_AMT4, BILL_AMT5, and BILL_AMT6 three continuous variables.

#### 3.2 Data Standardization

In order to eliminate the influence of dimension between variables and on model results, continuous variables  BILL_AMT1-6 & PAY_AMT1-6 are standardized.

#### 3.3 Dumb Coding

After significant correlation test + standardization + discretization,we get dummy coding of categorical data.

The result of Step 3 are stored in data_standardized.csv & data_standardized_dumb.csv

### Step 4. Model Comparison

We choose logistic regression, naive Bayes, decision tree and random forest to do the modeling. Grid search is used in order to find the optimal hyperparameters. The related code is [Part4_Model_Comparison.ipynb](https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Model%20comparison/Model%20Comparison.ipynb).

The sample data is divided by stratified sampling. 80% is used as training data, and 20% is used as test data.  Given the imbalance of the sample data, we use resample to make the dataset balanced, which can help to improve the performance of models. 

Since our aim is to recognize the clients who are likely to default, we mainly focus on the recall rate as well as F1 score when we evaluate the effectiveness of the models. From the charts below, it is shown that random forest works best among these four models.

|                 |  Logistic Regression  |   Naive Bayes    |   Decision Tree   |   Random Forest    |
|-----------------|-----------------------|------------------|-------------------|--------------------|
|Accuracy (Train) |0.70                   |0.69              |0.86               |0.92                |
|Accuracy (Test)  |0.70                   |0.69              |0.87               |0.93                |
|Precision        |0.80                   |0.76              |0.83               |0.91                |
|Recall           |0.53                   |0.55              |0.95               |0.96                |
|F1-score         |0.64                   |0.64              |0.88               |0.93                |
|Parameters       |{'C':0.01}             |{}                |{'criterion': 'gini', 'max_depth': 25}|{'criterion': 'gini', 'max_depth': 25, 'n_estimators': 30}|

<div align=center><img width="600" height="300" src="https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/charts/AUC%20of%20Algorithms.png"/></div>

<div align=center><img width="500" height="600" src="https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/charts/Comparison%20of%20Algorithms.png"/></div>

### Step 5. Model Evaluation

#### 5.1 Variable Input Space
In order to investigate the influence of different variable input spaces (i.e. different data processing methods) to the model, we set up 4 different input datasets. The related code is [Model evaluation+Model optimization.ipynb]( https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Model%20evaluation%20%2B%20Model%20optimization/Model%20evaluation%2BModel%20optimization.ipynb). See the following table for specific data processing methods:

|   Input   |                   Data processing methods                    |
| :-------: | :----------------------------------------------------------: |
| Dataset 1 | Kick out irrelevant features + standardize  numerical features. |
| Dataset 2 | Kick out irrelevant features + standardize  numerical features + discretization. |
| Dataset 3 | Kick out irrelevant features + standardize  numerical features + discretization + selected 2 features. |
| Dataset 4 | Kick out irrelevant features + standardize  numerical features + discretization + selected 2 features – features eliminated  by IV-WOE analysis. |

#### 5.2 Comparison

Based on the AUC value indicators, the comparison of modeling with different variable inputs is shown in the table below:

<table class="tg">
  <tr>
    <th class="tg-9e4f"></th>
    <th class="tg-gnn6" colspan="3">Dataset 1</th>
    <th class="tg-gnn6" colspan="3">Dataset 2</th>
    <th class="tg-t87r" colspan="3">Dataset 3</th>
    <th class="tg-t87r" colspan="3">Dataset 4</th>
  </tr>
  <tr>
    <td class="tg-9e4f"></td>
    <td class="tg-gnn6">precision</td>
    <td class="tg-gnn6">recall</td>
    <td class="tg-gnn6">f1_score</td>
    <td class="tg-gnn6">precision</td>
    <td class="tg-t87r">recall</td>
    <td class="tg-t87r">f1_score</td>
    <td class="tg-t87r">precision</td>
    <td class="tg-t87r">recall</td>
    <td class="tg-t87r">f1_score</td>
    <td class="tg-t87r">precision</td>
    <td class="tg-t87r">recall</td>
    <td class="tg-t87r">f1_score</td>
  </tr>
  <tr>
    <td class="tg-gnn6">Logistic&nbsp;&nbsp;&nbsp;Regression</td>
    <td class="tg-9e4f">0.81</td>
    <td class="tg-9e4f">0.51</td>
    <td class="tg-9e4f">0.63</td>
    <td class="tg-9e4f">0.8</td>
    <td class="tg-8h9k">0.51</td>
    <td class="tg-8h9k">0.63</td>
    <td class="tg-8h9k">0.8</td>
    <td class="tg-8h9k">0.52</td>
    <td class="tg-8h9k">0.63</td>
    <td class="tg-8h9k">0.79</td>
    <td class="tg-8h9k">0.52</td>
    <td class="tg-8h9k">0.63</td>
  </tr>
  <tr>
    <td class="tg-gnn6">Naive Bayes</td>
    <td class="tg-9e4f">0.76</td>
    <td class="tg-9e4f">0.57</td>
    <td class="tg-9e4f">0.65</td>
    <td class="tg-9e4f">0.76</td>
    <td class="tg-8h9k">0.56</td>
    <td class="tg-8h9k">0.65</td>
    <td class="tg-8h9k">0.76</td>
    <td class="tg-8h9k">0.56</td>
    <td class="tg-8h9k">0.65</td>
    <td class="tg-8h9k">0.76</td>
    <td class="tg-8h9k">0.56</td>
    <td class="tg-8h9k">0.65</td>
  </tr>
  <tr>
    <td class="tg-t87r">Decision Tree</td>
    <td class="tg-8h9k">0.81</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.88</td>
    <td class="tg-8h9k">0.81</td>
    <td class="tg-8h9k">0.95</td>
    <td class="tg-8h9k">0.87</td>
    <td class="tg-8h9k">0.81</td>
    <td class="tg-8h9k">0.94</td>
    <td class="tg-8h9k">0.87</td>
    <td class="tg-8h9k">0.82</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.88</td>
  </tr>
  <tr>
    <td class="tg-t87r">Random Forest</td>
    <td class="tg-8h9k">0.9</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.93</td>
    <td class="tg-8h9k">0.9</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.93</td>
    <td class="tg-8h9k">0.9</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.93</td>
    <td class="tg-8h9k">0.89</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.93</td>
  </tr>
</table>

#### 5.3 Conclusion

1. When comparing the four models, Random Forest has the best prediction performance, with the highest precision score, recall rate, and F1 score. Even though Logistic Regression and Decision Tree have similar precision, recall rate of LR is much lower than that of Tree, indicating LR categorizes more customers to not default and Tree categorizes more customers to default. 
2. When comparing the first 3 datasets, different data processing methods have no significant difference in prediction credit card default. There are 2 possible reasons: one is that the different features between datasets have poor ability to interpret the dependent variable; second is that input features are redundant. We will explain in detail in Step 6.
3. The score drop from dataset 3 to dataset 4 is quite small, so removing *SEX* and *MARRIAGE* by IV analysis works.

### Step 6. Model Optimization

#### 6.1 Feature Selection with Random Forrest

In Step 5, we find out that different data processing methods have little influence on model performance. To further understand the reasons, we deploy Random Forest to calculate the importance of features  for feature selection to reduce dimensions and eliminate noises. The related code is [Model evaluation+Model optimization.ipynb]( https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Model%20evaluation%20%2B%20Model%20optimization/Model%20evaluation%2BModel%20optimization.ipynb) (The running time of this piece of code will be a little bit long, slightly more than one hour. Sorry about that.). We sort the importance of features in dataset 3.  

<div align=center><img width="500" height="600" src="https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/charts/Feature importances for RF.png"/></div>

Feature importance adds up to 1. We can see from the picture of cumulative importance that the least important 20 features only account for less than 20% importance, which indicates feature redundant.

<div align=center><img width="500" height="300" src="https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/charts/cumulative importance.png"/></div>

We set different quantiles for cumulative importance, corresponding to different numbers of features, and examine prediction performance, to test how much we can reduce the features without losing too much accuracy. Because Random Forest model outperform other models in our previous trial, we only examine the performance of the Random Forest model on decreasing features. The result is shown in the table below:

<table class="tg">
  <tr>
    <th class="tg-amwm">cumulative importance</th>
    <th class="tg-t87r">precision_score</th>
    <th class="tg-t87r">recall</th>
    <th class="tg-8h9k">f1_score</th>
    <th class="tg-8h9k">num of features</th>
  </tr>
  <tr>
    <td class="tg-baqh">0.1</td>
    <td class="tg-61zd">0.77</td>
    <td class="tg-8h9k">0.95</td>
    <td class="tg-8h9k">0.85</td>
    <td class="tg-8h9k">2</td>
  </tr>
  <tr>
    <td class="tg-baqh">0.2</td>
    <td class="tg-61zd">0.82</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.88</td>
    <td class="tg-8h9k">3</td>
  </tr>
  <tr>
    <td class="tg-baqh">0.3</td>
    <td class="tg-61zd">0.84</td>
    <td class="tg-8h9k">0.95</td>
    <td class="tg-8h9k">0.89</td>
    <td class="tg-8h9k">5</td>
  </tr>
  <tr>
    <td class="tg-baqh">0.4</td>
    <td class="tg-61zd">0.85</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.9</td>
    <td class="tg-8h9k">6</td>
  </tr>
  <tr>
    <td class="tg-baqh">0.5</td>
    <td class="tg-61zd">0.86</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.91</td>
    <td class="tg-8h9k">8</td>
  </tr>
  <tr>
    <td class="tg-baqh">0.6</td>
    <td class="tg-8h9k">0.87</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.92</td>
    <td class="tg-8h9k">10</td>
  </tr>
  <tr>
    <td class="tg-baqh">0.7</td>
    <td class="tg-8h9k">0.9</td>
    <td class="tg-8h9k">0.97</td>
    <td class="tg-8h9k">0.93</td>
    <td class="tg-8h9k">12</td>
  </tr>
  <tr>
    <td class="tg-baqh">0.8</td>
    <td class="tg-8h9k">0.89</td>
    <td class="tg-8h9k">0.97</td>
    <td class="tg-8h9k">0.93</td>
    <td class="tg-8h9k">15</td>
  </tr>
  <tr>
    <td class="tg-baqh">0.9</td>
    <td class="tg-8h9k">0.9</td>
    <td class="tg-8h9k">0.97</td>
    <td class="tg-8h9k">0.93</td>
    <td class="tg-8h9k">24</td>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-8h9k">0.9</td>
    <td class="tg-8h9k">0.96</td>
    <td class="tg-8h9k">0.93</td>
    <td class="tg-8h9k">41</td>
  </tr>
</table>

#### 6.2 Conclusion

1. Both the 2 selected features in Step 2, *Limit_Usage* and *Pay_Amt_std* , are among the most important features,  with importance value of 0.079926 and 0.065781 respectively. Therefore, in Step 5, that the modeling with dataset 3 fails to outperform modeling with dataset 2 can be attributed to the fact that redundant features nibble away the explanatory power of the 2 selected features. As for *LIMIT_BAL_GROUP*, its feature importance is the lowest. Neither the inclusion nor the exclusion of  *LIMIT_BAL_GROUP* affects model performance. Thus we can understand the outcome of Step 5 from the the perspective of feature importance.

2. We continuously reduce the number of features according to their feature importance from the smallest to the largest and find that at the 0.7 quantile, corresponding to 12 features, the model's prediction performance doesn’t significantly deteriorate, so we successfully removed more than half of the features. According to this model, some important features to predict customers repayment behaviors are: 

<table class="tg">
  <tr>
    <th class="tg-t87r">Features</th>
    <th class="tg-t87r">Explanation</th>
  </tr>
  <tr>
    <td class="tg-61zd">Limit_Usage</td>
    <td class="tg-8h9k">How much credit customer used in the most recent month.</td>
  </tr>
  <tr>
    <td class="tg-61zd">Pay_Amt_std</td>
    <td class="tg-8h9k">Whether the repayment behaviour of the customer is consistent.</td>
  </tr>
  <tr>
    <td class="tg-61zd">BILL_AMT1, BILL_AMT2,&nbsp;&nbsp;&nbsp;BILL_AMT3</td>
    <td class="tg-8h9k">Bill amount of last 3 months. </td>
  </tr>
  <tr>
    <td class="tg-61zd">PAY_AMT1, …, PAY_AMT6 </td>
    <td class="tg-8h9k">Repayment amount of last 6 month.</td>
  </tr>
  <tr>
    <td class="tg-61zd">PAY_0_TEST</td>
    <td class="tg-8h9k">Whether customer delayed his most recent bill.</td>
  </tr>
</table>

3. There is a very interesting finding of our project. Identity information, such as gender, marriage status and education level, doesn’t determine customer’s credit, while their past credit records make the difference. We can learn a lesson from this - don’t judge a man by his appearance  as only time reveals his heart. ^_^

    

