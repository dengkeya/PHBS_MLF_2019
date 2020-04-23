
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

I’m not sure if you still remember this feature, pay_i（回到github）, if it’s a negative number, it means users pay early, 0 means user pay for the month bill on time. If positive, delayed payment. See this point plot. Even if the bill is payed with 1 month delay, the risk of default is pretty low. However the relation between default and more than 2 month delay pay is high. Therefore, we set pay-I as paid if it s smaller than 2 and delay if it larger than or equal to 2.

####  2.2 Historical Bill Amount

Based on the understanding of credit card business, we think the figures of Historical Bill Amount (*BILL_AMT1*,..., *BILL_AMT6*) themselves contains not so much information. However,  the ratio of last month's amount of bill statement to the amount of the given credit can influence this month's default probability. So we take this ratio into consideration and create a new feature: *Limit_Usage*. Since we are going to predict October's default probability, so we only take September's ratio.

#### 2.3 Historical Payment Amount

As for the feature Historical Payment Amount (*PAY_AMT1*,..., *PAY_AMT6*), we think they are highly correlated with the default status. The more stable of previous  repayment, the less likely default in the future owing to the consistent repayment behavior. In order to describe the fluctuation among different level, we use the variation coefficient instead of standard deviation. 

The intermediate result of data cleaning and selection is saved in [Feature_Selection.csv](https://github.com/dengkeya/PHBS_MLF_2019/blob/master/project/Feature_Selection/Feature_Selection.csv).

### Step 3. Variable Correlation Analysis, Standardization & Dumb Coding

### Step 4. Model Comparison

### Step 5. Model Evaluation

### Step 6. Model Optimization
