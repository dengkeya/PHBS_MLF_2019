
# PHBS_MLF_2019_project
final_project(～￣▽￣)～  
## 0. Group Infomation
Student ID  | Student Name |  Github ID 
 :-: | :-: | :-:
1901212571| Keya Deng| [dengkeya](https://github.com/dengkeya)
1901212561| Jizhong Cao| [1901212561](https://github.com/1901212561)
1901212657| Xuan Yang | [xuanyang274](https://github.com/xuanyang274)
1901212615| Liu Xin | [rebeccaliu922](https://github.com/rebeccaliu922)
## **Dear Prof Choi,** 

We are facing a problem. We have two data sets to choose, but it is difficult to make up our mind. We want to ask for your advice!  

The first choice is about predicting the default of credit card. The dataset is about historical credit records,  bill data and payment records. We want to use this data set to predict the repayment status of users, so as to better grasp the credit risk. This dataset is quite direct for Machine Learning. We can practice what we have learned directly. However, we are afraid that this dataset is too normal.  

The second choice is from JD finance, to help them predict the borrow demand of customers so that they can match the amount of funds and demand to improve the utilization.  The dataset is about basic user information, mobile behavior data, shopping history and historical lending information. We want to help JD finance predict December loan demand information of each user and provide users with more accurate customized loan business. This dataset is a little bit complex. It may involve some new methods that we have not covered in the class. But we are afraid that we cannot handle it in the limited time >_<.



## **For our choice_1**:

## 1. Background

Credit card is an important part of personal banking business. All major banks are using customer personal information to establish credit scoring systems. The bank has to decide what kind of information to be collected, how to identify information authenticity and how to use the information to evaluate customer's credit. Based on credit card information of Taiwan in 2005 (data resource: http://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients), this research aims to predict customers’ repayment behavior with multiple machine learning models and explore the key factors affecting customer credit. 
## 2. Data Description
Our dataset contains 30,000 samples with no missing data. There are 23 features in total, which can be divided into five categories.
Categories                |    Features
---------------------     | ----------------------
Credit card information   | Credit limit
Indentity information     | Gender, education, marriage, age
Historical credit record  | Whether the client has defaulted in each month (April to September)
Historical bill amount    | The bill amount in each month (April to Semptember)
Historical payment record | How much the client has paied in each month (April to September)
## 3. Our Aim
* The model is built based on the existing basic population attribute data, historical credit history, historical billing data and historical repayment data. According to the model, it can predict whether the credit card holder will repay the money on time next month. Essentially, it is a typical dichotomous problem.

* Determine which variables are strong predictors of whether a customer is overdue, and compare the differences of prediction effects between different data processing methods and mathematical models.

  

## **For our choice_2**:

## 1. Background

To develop credit business, not only do we need to assess the risk level of customers, but also need to forecast the loan demand of customers and make a good match between the amount of funds and the demand to improve the utilization rate of funds. JD can make full use of its transaction data as an e-commerce platform to describe user portraits, so that they can provide customized service for each user. Our data set from https://www.pkbigdata.com/common/cmpt/京东金融全球数据探索者大赛_赛体与数据.html

## 2. Data Description

The entire dataset consists of five files, each containing the type of information characteristic. There are 8,027 attributes in the entire data set, more than 90,000 users in total, and only about 20,000 users will borrow money every month. So the whole data set is very unbalanced. This problem needs to be dealt with by us.

| Data file name | Type of features                                             |
| -------------- | ------------------------------------------------------------ |
| t_user         | users' ID, age, sex, user activation date, initial loan limit |
| t_order        | users' ID, purchase time, price, buying quality, category, discount |
| t_click        | users' ID, click time, page user clicking, params of page    |
| t_loan         | users' ID, loan time, loan amount, repayment periods         |
| t_loan_sum     | users' ID, month, loan sum                                   |
