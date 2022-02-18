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

Screenshot_1.png

### 3. Data Exploration and Feature Engineering

To begin with data exploration, I used summary statistics to get to know our data and used various other techniques to visualize them to uncover any insights. 

Since I am using few dummy variables for the modelling, I am changing the data type for categorical variables from Character to Numeric binary variable (0/1) and applying feature engineering to create other dummy variables


