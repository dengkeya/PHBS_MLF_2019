
# PHBS_MLF_2019_project
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
