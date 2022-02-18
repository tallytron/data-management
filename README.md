# data-management

## Project Overview:

This project is about analyzing a dataset from one of big banks to to interpret this analysis through the lens of a business problem/question and draw conclusions/make recommendations to management.   

Business Problem: Improve the NPS score for the organization

The goal is to find a relationship between Net Promoter Score and corresponding customer demographic information that will help improve the customer satisfaction and retention. For this analysis, I have used Fiscal YR 2020 Q3 loyalty survey response data.

Identifying variables that affect or impact the NPS score would help the bank to further focus in the area that improves customer stickiness, satisfaction, and other drivers.

For the analysis I have used different various types of datasets such as i) “NPS Customer Sample Data” that contains customer survey information with around 70K observations. ii) NPS Customer Survey Response information that contains all the customer responses with around 3K observations iii) Customer Demographics data iv) Customer holdings v) Customer Channel behavior vi) Survey Lookup (data dictionary) file, which provides details of the survey questions.

For the dataset, I will provide just anonymous small snippets of what I have done for context, but for data privacy I will not share it in detail.

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

As I have completed testing the model, as next step I have assessed the model using graphical results which provide more information on i) univariate outliers, ii) multivariate outliers, iii) normality curve, iv) heteroskedasticity, v) homoskedasticity, vi) coefficient magnitude direction. I assessed the model using graphical plots of the error term and prediction, as well as the formal testing procedure in R, which included hypothesis and other tests. I evaluated the error term to see if there is any pattern in the graphical findings and to capture any variance that may have been overlooked throughout the model development procedure. 

I tried to see if the model has normal distribution (Normality) or not.

![image](https://user-images.githubusercontent.com/80222038/154730900-d3400199-66fd-467a-b128-f962410c4652.png)

![image](https://user-images.githubusercontent.com/80222038/154730917-87a53945-ef84-4f4f-a26f-827d3c441a06.png)

From the normal curve, we can assume that the curve has a smaller standard deviation and hence its taller and narrower, and the probability is spread over smaller range of values.

Comparing the normal curve and Q-Q plot, it seems the normal distribution has a fat tail which explains why both the ends of the Q-Q plot has deviated from the straight line although the center still follows a straight line. 

If our number of observations or data points are too low, the Q-Q plot might not produce a perfect fit for the distribution and doesn’t give a very concrete information about the type of distribution.

Since our normal curve was too narrow, I subsequently wanted to perform hypothesis test to confirm if this is not a big problem to our model.

![image](https://user-images.githubusercontent.com/80222038/154731143-35614252-8ade-4158-976d-d9c4058269a3.png)

From the results, we can see the p-value is extremely small and we can reject the null hypothesis and conclude all the variables we selected are significant.

Further to our multivariate regression model, we have created few dummy variables that we thought would impact the score and created interactions with other variables in the model.
Using two dummy variables student indicator and service indicator, I created 18 different interactions, and 2 of them appear as significant which are one account indicators and interacts with two questions from the survey differently.

For age, I created dummy variables and divided into three groups. 
I: 18-30 
II: 30-50
III: 50 and above

Upon running the multiple linear regression model the interactions are significant the R-squared and adjusted R-squared values are incredibly low, this does not suggest that the model is not good, it suggests that there is a lot of variances present in the dataset around the regression line. Even though the significance of age variable is high, the coefficient is low, that would impact the overall model in a small way, it might not be the best idea to use age in the model as it would not provide accurate results.

![image](https://user-images.githubusercontent.com/80222038/154732190-cf44ebc2-7fb7-427a-8dee-83a1c81d577f.png)

After that, I ran the analysis on Promoters and Detractors trying to find out what factors might be affecting the end customers to score low on the NPS questionnaire. I created 3 dummy variables, one for each of the category under NPS Category column i.e. Promoter, Detractor and Passive. I then created interactions for Promoters and Detractors with all the questions and ran regression against the questions from the survey.

![image](https://user-images.githubusercontent.com/80222038/154732385-4320918a-34f3-48f7-a945-f5b102272d37.png)

From this analysis, we can conclude that Q5 and Q6 are the main questions the bank needs to focus on this area to plan a strategy to bring the NPS Score up. To make sure that one of these questions under Q6 are not affecting the results of other question, I ran the chow test on these, and found that they are all independent of each other.

## Recommendations

Based on my analysis here are my recommendations for the bank:

Customers who are students are currently showing a negative relationship with NPS score. To resolve this, the bank can build a strategy around improving student customer relationship by either cross selling or upselling with other products and services or further investigate this area to understand their main points that are stopping them from giving a better score.

Primary customers and secondary customers impact the score relatively same level. This demonstrates whether consumers have a strong attachment to the bank or not; if they are dissatisfied with the goods and services, they will not promote the bank to their circle. This poses greater risk of losing these high value customers. Offer loyalty programs for primary customers, as well as special prices depending on the direction of the coefficients.

According to sentiment analysis, clients classified as "Passives" feel satisfied with the bank's goods and services. I recommend the bank to further investigate this area as this would help move lot of customers that were sentimentally feeling good but yet have been rating under “Passive”.

Part of the recommendation, the suggestion is to provide front-end employees should be equipped with the appropriate tools. This will empower them to follow up with customer pain points s and close the loop. This also gives them a full transparency on the branch score, understanding individual branch issues /complaints, tracking score against other markets and regions. Overall, this would allow them to excel in their service and identify areas of skillset gap and provide required training as necessary.
