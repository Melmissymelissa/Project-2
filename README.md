<div align="center"><h1>Project 2: Statistical Significance - Pricing Factors - House Prices</h1> </div>

<!-- TABLE OF CONTENTS -->
   ## Table of Contents
   - [Introduction](#introduction)
   - [About The Dataset](#aboutthedataset)
   - [Installation](#installation)
   - [EDA](#EDA)
   - [Analysis](#analysis)
   - [Results](#results)

## Introduction
This project aims to analyze the housing market in Ames Iowa. We will explore the key factors affecting property values, such as location, quality, and neighborhood characteristics. We'll analyze how  we should allocate reserved dollars for investment into mortgage-backed securities using statistical analysis.

## About the Dataset
Widely recognize far is high quality and accuracy, the Ames, Iowa housing dataset includes a wide range of variables making it the ideal choice when predicting housing prices, understanding market trends and exploring the impact of different factors on property values.

This dataset contains <u>1,460 rows</u> and <u>81 main columns</u>
The columns we'll pay close attention to are:
- ID: A unique identifier for each house (integer).
- OverallQual: Overall material and finish quality (integer).
- OverallCond: Overall condition rating (integer).
- 1stFlrSF: First Floor square feet (integer).
- GarageArea: Size of garage in square feet (integer).
- GarageCars: Size of garage in car capacity (integer).
- SalePrice: Sale price in dollars (integer).
- TotalBsmtSF: Total square feet of basement area (integer).
- LotArea: Lot size in square feet (integer).
- Neighborhood: Physical locations within Ames city limits (object).
- YearBuilt: Original construction date (integer).
- GrLivArea: Above grade (ground) living area square feet (integer).

I also created a new field called 'Price per sq ft'
```python
# Creating a new field
hp_df['Price per sq ft'] = hp_df['SalePrice'] / hp_df['LotArea'] # calculating the price by sq ft
  ```
<div align="center"><h2>Hypotheses</h2> </div>
<p></p>
<p>1. Reject the null that there is no significant difference between above average quality homes and below average quality homes. </p>
<p>2. Reject the null  that there is no significant difference in the average price per square foot between the top neighborhoods and the other neighborhoods with above average homes</p>
<p>3. Reject the null that there is not a significant difference in the price per square foot between above average quality 1 story and 2 homes in the top neighborhoods. </p>
<div align="center"><h2>Results</h2> </div>
<h3>Hypothesis Analysis 1: </h3>
<p>Reject the null that there is no significant difference between above average quality homes and below average quality homes.</p>
<p>This difference is significant at the <.05 level. With 95% confidence, the difference is between 7.53 and 9.76 means.</p>
<h3>Hypothesis Analysis 2: </h3>
<p>Reject the null  that there is no significant difference in the average price per square foot between the top neighborhoods and the other neighborhoods with above average homes.</p>
<p>This difference is significant at the <.05 level. With 95% confidence, the difference is between 29.81 and 38.69 means.</p>
<h3>Hypothesis Analysis 3: </h3>
<p>Reject the null that there is not a significant difference in the price per square foot between above average quality 1 story and 2 homes in the top neighborhoods. </p>
<p>TThis difference is significant at the <.05 level. With 95% confidence, the difference is between 1.00 and 7.16 means.</p>
<h3>Recommendations</h3>
<p>To increase your chances of success and property investment, I recommend we focus on above average quality two-story homes located in the top neighborhoods, develop a robust marketing strategy to attract potential buyers, and diligently track both your invested properties in market trends. By implementing these recommendations, you can optimize your investment allocation and maximize returns in the competitive real estate market</p>
