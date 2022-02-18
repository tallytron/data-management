# data-management

## Project Overview:

This project is about analyzing a dataset from one of big banks to to interpret this analysis through the lens of a business problem/question and draw conclusions/make recommendations to management.   

Business Problem: Improve the NPS score for the organization

The goal is to find a relationship between Net Promoter Score and corresponding customer demographic information that will help improve the customer satisfaction and retention. For this analysis, I have used Fiscal YR 2020 Q3 loyalty survey response data.

Identifying variables that affect or impact the NPS score would help the bank to further focus in the area that improves customer stickiness, satisfaction, and other drivers.

For the analysis I have used different various types of datasets such as i) “NPS Customer Sample Data” that contains customer survey information with around 70K observations. ii) NPS Customer Survey Response information that contains all the customer responses with around 3K observations iii) Customer Demographics data iv) Customer holdings v) Customer Channel behavior vi) Survey Lookup (data dictionary) file, which provides details of the survey questions.

## Methodology:

1) Merging/joining datasets in SQL/R 

2) Data Cleaning 

3) Data exploration and Feature Engineering 

4) Predictive modelling and Hypothesis tests 


### 1. Cleaning: 
 
Before I built our model, I did several data cleaning procedures in R. 

As part of cleaning of the survey response data from the vendor. I have done a few steps of cleaning the data like removing unwanted variables (operational data fields), converting from numeric to character for categorical analysis, creating duplicate variables with both formatting one is used for quantitative, and another variable is used for qualitative analysis.  I joined the multiple datasets used in the analysis, I have used inner join on them as required. For example, I filtered out the customers who have finished the survey partially by selecting the “survey finished” using FILTER to remove the customer with  “survey partially finished.” 

### 2. Merging datasets

I added City and Province in addition to “postalcode” using LEFT_JOINT by “postalcode” into the dataset to help me find whether the NPS score has any geographic features. (The cc file is another file including postcode, city, province, latitude, and longitude.) 

![image](https://user-images.githubusercontent.com/80222038/154724916-9cb777cd-8503-493f-a7d7-25441a555896.png)


### 3. Data Exploration and Feature Engineering

To begin with data exploration, I used summary statistics to get to know our data and used various other techniques to visualize them to uncover any insights. 

Since I am using few dummy variables for the modelling, I am changing the data type for categorical variables from Character to Numeric binary variable (0/1) and applying feature engineering to create other dummy variables

![image](https://user-images.githubusercontent.com/80222038/154723968-8c675687-345b-438a-a1bc-f9f688009fbd.png)

For example, I feature engineered for the data field customer “title” to create a new field  for “gender” to see whether gender is one of the features impact NPS score.  

![image](https://user-images.githubusercontent.com/80222038/154725122-9f6a0d02-2240-442c-ad6a-26a6821917d2.png)

I realized customer tenure may impact the NPS score. So, I calculated the tenure using “customer since date” (tenure = “survey date” - “customer since date”) 

![image](https://user-images.githubusercontent.com/80222038/154725333-7042ecd6-5713-42d0-9d5a-529aede96785.png)

There are some missing data within the NPS dataset. Since these data are MAR in nature, I imputed them using MICE technique for generating responses for some of these questions. Also, I checked if there are any NA in the imputed data set.  

### 4. Predictive modelling and hypothesis testing

Multiple Linear Regression Model

For the multiple regression model (i.e multiple independent variables used to infer one dependent variable), I wanted to find what independent variables would correlate with the dependent variable NPS Score.  To identify that relationship, I first built the model to contain all the numeric and dummy variables that I assumed are significant. I used the Test Test Test (TTT) method with all the possible variables included in the first iteration of the model and subsequently ‘tested the model down’ in every other iteration until I find the optimum or satisfactory model.

![image](https://user-images.githubusercontent.com/80222038/154725888-a017db42-2984-437e-bb80-83bbc77d4d39.png)

From the regression summary statistics shown below, I found that some indicators did not significantly perform as expected. Primary customers (based on banks primary customer definition) tend to provide higher score compared to secondary customers. Similarly, if a customer has larger customer balances, they are more engaged with the bank and more likely to recommend the Bank to their friends and relatives as they have a deeper relationship with the bank already. However, the regression results show these two variables as non-significant in predicting the score. 

![image](https://user-images.githubusercontent.com/80222038/154726188-28dce743-083c-4b13-8ca7-38906031a075.png)

In the second iteration of running the model, I removed all the non-significant variables from the model. 

As we can see the summary statistics of the second run output listed below, indicates that the responses to questions 5 and 6 series strongly correlate with the NPS score except for questions related to credit card product and online banking service. Also, the balance of credit card has a strong positive correlation with the NPS score. It may imply that the customers with higher credit limits authorized by the bank, are given higher NPS score. We can also find that the R-square is 0.8133, which is very close to 1, suggests this model is reasonable in general.

![image](https://user-images.githubusercontent.com/80222038/154726538-6e54ec72-7382-4e48-9e6a-9a362433c1ff.png)


