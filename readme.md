# Ames Housing Data and Kaggle Challenge

## Problem Statement 
Develop a prediction model that can assist real estate agents to giving the best price prediction of the house sale price. This model can also be used for real estate agents to advice homeowners on how to increase their home's sale price.
This model aims to allow real estate agents and ultimately homeowners get the best sale price for their houses.

## Introduction to Dataset

Data set used is Ames Iowa Housing Data Set.
- Observations: 2051
- Features: 80
- Target: Sale Price

The Ames Iowa Housing Data Set has a variety of features of the property and ultimately what it sold for.

Detailed information on the [dataset](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt).

## Directory Structure
| Notebook  | Description  | 
|---|---|
|Cleaning|Cleaning of original train and test dataset and dealing with missing values.|
|EDA|Exploratory Data Analysis of the train data set.|
|Preprocessing & Feature Engineering|Removal of unncessary features and adding Polynomial Features. Train test split and standard scalar the train dataset. Create the 1st basic model.|
|Model Tuning| |
|Prediction Model| |
|Insights| |

## Datasets
| csv File  | Description  |
|---|---|
|test|Original Test Dataset|
|train|Original Train Dataset|
|sample_sub_reg|Sample submission file for Kaggle|
|test_cleaned|Test Dataset with no missing values|
|train_cleaned|Train Dataset with no missing values|
|test_with_dummies|Test Dataset with categorical data converted to dummies|
|train_with_dummies|Train Dataset with categorical data converted to dummies|
|test_feature_engineer|Test Dataset with unwanted features removed|
|train_feature_engineer|Train Dataset with unwanted features removed|
|result1|Test result for kaggle submission|
|final|Final result for kaggle submission|

## Models
| Model  | Description  |
|---|---|
|test_org|Test Model on original sale price|
|test_log|Test Model on log sale price|
|gs_tune1|Grid Search Tuning Model 1|
|gs_tune2|Grid Search Tuning Model 2|
|pred_model|Final Model for Prediction|
|presentation_model|See Additional Section below|
|test_model|See Additional Section below|

## Brief Summary of Analysis

The dataset was first checked and cleaned to remove or fill up any missing values. Exploratory data analysis was done to get a rough feel of the data and to check for correlations. Unnecessary features were removed. Polynomial Features was used to check for interactions between features. The model was then created using linear regression with elastic net as penalty. Grid search was used to find the best hyperparameters. This model was then deployed to the test dataset to predict sale price.

## Cleaning

### Missing Data
Missing data was studied first. Checked whether there was any 'bad' rows - these are rows which have too much data missing that making a prediction on them was not possible. The row which had the most features missing had 16 out of 80 features missing. This was not that many and shows that the row still has some useful data. Hence, no rows were dropped.

Next, on to columns or features. Features like Pool QC and Misc Feature had greater than 95% of the data missing. These columns were removed as too much data is missing for us to effectively make predictions.

Next, some data was 'missing' simply because the feature did not exist in that house, for example the house did not have a basement or garage. For these cases, '0' or 'None' was added into the dataframe. These features should not be dropped as it is useful in seeing whether a basement or garage affects the property price.

### String Data
Linear Regression models cannot predict based on string data, hence these data must be converted to numerical values.
There were 2 types to be converted: Ordinal and Categorical.

**Ordinal**: These data were in strings but had an ordinal relationship, for example (Good, Average and Poor). These strings were converted to numbers with 1 being the best score and the largest number being the worst score.

**Categorical**: These data had no visible relationship between one another. To prevent the machine learning model from assuming such a relationship, these features were dummified.

### Test and train
The test and train datasets both underwent the same processing method.

## EDA
A brief exploratory data analysis was done.

### Correlations
**Correlation of Sale Price and Features**
There were some features that showed almost 0 relation to the sale price while some were highly correlated. Features showing an almost 0 correlation to the sale price can be dropped.

**Correlation between Features**
Some features showed very high correlation with one another. These features can be removed to prevent multicollinearity.

**Housing Price**
Housing price histogram was plotted and it showed that it was not normally distributed with a huge right tail. This means that there were many outliers that sold for an exceptionally high price. The logarithmic function was taken for housing price data was taken to get a more normally distributed sale price. There were still outliers on both ends. Outliers were not removed as they could provide useful data - we are also not sure if the test dataset has any outliers in terms of sale price.

**Categorical Data**
A stripplot was used to check out categorical data (I used it instead of boxplot because you can see the number of sales made). Some features were plotted, for e.g. neighbourhood vs sale price. Which neighbourhood the house was in affected saleprice. For example, houses in OldTown sold for very low prices.

**Ordinal Data**
From the EDA, it can been seen that overall quality has a very high relation to sale prices. The higher the overall quality, the higher the sale price. Most houses were rated between 6-8.

## Preprocessing & Feature Engineering

**Features with low correlation**
Features with very low correlation to sale price was removed. If the correlation was less than absolute 0.03, it was removed. These features will most likely have no impact on sale price.

**Features with high correlation to each other**
Features that were highly correlated with one another were removed to reduce model complexity and to prevent multicollinearity. If the feature had higher than 0.8 correlation score, it was removed.

**Polynomial Features**
With the features left, polynomial features was applied. This will help us understand if there are any interactions between the features and if any of the features are not linearly related.

**Train Test Split**
The train dataset was split into train (.7) and test (.3) to test model effectiveness.

**Standard Scalar**
Data was scaled so that the magnitude of the data will not affect the coefficients of the model. The data was scaled only on the train split to prevent data leakage from the test split.

**ElasticNet Penalty**
ElasticNet allows us to test both Lasso and Ridge Regularization or a combination of them. For l1_ratio = 0 the penalty is an L2 penalty (Ridge). For l1_ratio = 1 it is an L1 penalty (Lasso). 

**Basic Model**
Trial model with original sale price data and the log function of sale price data. Log function of sale price data model had a better RSME of 23,573 compared to 26,106. Decided to built a model based on log function of sale price. Submit basic test file to kaggle.

## Model Tuning
Performed GridSearchCV with ElasticNet as a penalty on train data set's train portion. Tested RSME on train test split. Performed GridSearchCV again random another train test split with more defined parameters. Tested RSME on train test split. Both RSME for train and test portion of train's data set was good.

## Prediction Model & Kaggle
Further refine the model's hyperparameters using GridSearchCV with ElasticNet as a penalty on whole train data set instead of only 70% of the train data set. Submit to Kaggle. RSME score on full train data set 20,776. Kaggle RSME score for full test data set 26,096.

RSME score on final test dataset means that there is an average error of $26,096 compared to the actual sale price. Given that an average house sold for $181,469 (from train dataset), this mean the error is about 14.4% which seems reasonable.

## Insights & Recommendations

The below features have the greatest impact on sale price.
|feature|	coef|
|--|--|
|Overall Qual Year Built|	0.069313|
|Year Built Gr Liv Area|	0.058370|
|Overall Cond Gr Liv Area|	0.058281|
|Year Built Year Remod/Add|	0.036493|

Features are interaction features as polynomial features was used. Overall, the features that impacted sale price the most will be Overall Quality, Year Built, Gr Liv Area, Overall Condition, Year Remod/Add.

These items are hard to change. Recommendations for simple things that homeowners could do to improve their homes would be: improve their basement finished area, improve heating and improve kitchen quality. (No major renovations.)

From our model, if owners improve these items to maximum quality, sale price is predicted to increase by around 20%.

## Conclusion

This model can be used by real estate agents to give a good price estimate for the sale. This model also tells us what features are highly valued by buyers. Real Estate agents can advice homeowners on what are some simple things they can do to increase their home's value.

