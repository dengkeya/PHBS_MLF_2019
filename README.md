
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

##### **(1) Contingency Analysis  & IV-WOE Analysis with Categorical Variables**

- **Contingency Analysis**

For categorical variables, we use Contingency Analysis to justify the relation with default condition. 

When p-value < 0.05,  there is a significant relation between the variable and y. 

- ##### **IV-WOE Analysis**


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

##### **(2) one-way ANOVA with Numerical Variables**

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

We choose logistic regression, naive Bayes, decision tree and random forest to do the modeling. Grid search is used in order to find the optimal hyperparameters. The related code is [Part4. Model Comparison.ipynb](https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Model%20comparison/Model%20Comparison.ipynb).

The sample data is divided by stratified sampling. 80% is used as training data, and 20% is used as test data.  Given the imbalance of the sample data, we use resample to make the dataset balanced, which can help to improve the performance of models. 

Since our aim is to recognize the clients who are likely to default, we mainly focus on the recall rate as well as F1 score when we evaluate the effectiveness of the models. 



### Step 5. Model Evaluation

#### 5.1 Variable Input Space
In order to investigate the influence of different variable input spaces (i.e. different data processing methods) to the model, we set up 5 different input datasets. See the following table for specific data processing methods:

|   Input   |                   Data processing methods                    |
| :-------: | :----------------------------------------------------------: |
| Dataset 1 | Kick out irrelevant features + standardize  numerical features. |
| Dataset 2 | Kick out irrelevant features + standardize  numerical features + discretization. |
| Dataset 3 | Kick out irrelevant features + standardize  numerical features + discretization + selected 2 features. |
| Dataset 4 | Kick out irrelevant features + standardize  numerical features + discretization + selected 2 features – features eliminated  by IV-WOE analysis. |

#### 5.2 Comparison

Based on the accuracy and AUC value indicators, the comparison of modeling with different variable inputs is shown in the table below:

#### 5.3 Conclusion

### Step 6. Model Optimization

#### 6.1 Feature Selection with Random Forrest

Random forest can calculate the importance of features and thus can be used for feature selection to reduce dimensions and eliminate noises. We use random forest to sort the importance of features in dataset 3. 22 indicators with feature importance greater than 0.01 are selected.

Performance of the four models are shown in the figure below:

#### 6.2 Conclusion
