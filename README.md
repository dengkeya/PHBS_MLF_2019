
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

Now we have a problem. We have two data sets to choose, but it is difficult to make up our mind. We want to ask your advice!

The first choice is a data set related to the historical credit history and historical billing data of credit card users. We want to use this data set to predict the repayment status of users, so as to better grasp the credit risk

The second choice is about users' order information, clicking page information  provided by an e-commerce platform from August to November. This e-commerce platform provides a certain amount of consumption loan like installment. We want to help the platform predict December loan demand information of each user, and provide users with more accurate customized loan business.



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

互联网从急速扩张时期开始慢慢过渡到精准营销的时代，如何对使用者进行用户画像，针对每个用户