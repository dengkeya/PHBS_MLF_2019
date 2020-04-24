
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

Befor using any model to predict, the first step is always explore what data we have. So we visualize the features we have and clean the original data. The relating code is [Part1_Data_Cleaning.ipynb](https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Data_Cleaning/Part1_Data_Cleaning.ipynb).

#### 1.1 Data Visualization

Firstly we visualize *Limit_Bal, Sex, Education, Marriage, Age* these five features. 

#### 1.2 Data Cleaning

Balabala

### Step 2. Features Selection

### Step 3. Variable Correlation Analysis, Standardization & Dumb Coding

#### 3.1 Variable Correlation Analysis

The correlation analysis of single variable mainly analyzes the correlation between each independent variable and the dependent variable, and takes the variables with significant correlation as the input variable set of the model.
Considering that the dependent variable is a fixed-type binary variable, IV and WOE were calculated for the type independent variable, and the independence test of Contingency Analysis was conducted.  For the numerical independent variable, the independence test of Variance Analysis was conducted.

##### (1) Contingency Analysis  & IV-WOE Analysis with Categorical Variables

 ##### Contingency Analysis

For categorical variables, we use Contingency Analysis to justify the relation with default condition. 

When p-value < 0.05,  there is a significant relation between the variable and y. 

##### IV-WOE Analysis

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

### Step 5. Model Evaluation

### Step 6. Model Optimization