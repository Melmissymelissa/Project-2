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
## Installation
Import and alias modules
```python
import math
import pandas as pd
from scipy import stats
import seaborn as sns
import matplotlib.pyplot as plt
import plotly.express as px
import numpy as np
import statsmodels.api as sm
from google.colab import drive
from statsmodels.formula.api import ols
drive.mount('/content/gdrive')
sns.set()
pd.set_option('display.max_colwidth', None)
  ```
Bringing in the dataset
```python
hp_df = pd.read_csv("/content/gdrive/MyDrive/My Data Analyst Portfolio/Project 2: Statistical Significance - Pricing Factors - House Prices/raw_data.csv")
hp_df.info()
  ```
Running Descriptive Statistics
```python
hp_df.describe().round()
  ```
![Screen Shot 2024-01-10 at 12 21 23 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/83e7c517-8fa5-450e-b67f-410451ccc13b)

## EDA
To first start off my exploratory data analysis, I first wanted to take a closer look at the catagories that are highly associated with a higher sale price. By utilizing the 'saleprice_corr' variable, I examined the categories that exhibit a strong correlation with a higher sale price, allowing me to gain a deeper understanding of the factors that significantly influence the sale price in the dataset. 
```python
corr_matrix = hp_df.corr()

# the correlation values of SalePrice with other columns
saleprice_corr = corr_matrix['SalePrice']

# sorting the correlation values in descending order
saleprice_corr_sorted = saleprice_corr.sort_values(ascending=False)

sns.heatmap(saleprice_corr_sorted.to_frame(), annot=True, cmap='seismic', cbar=False, annot_kws={"size": 20})

plt.xticks(fontsize=17)
plt.yticks(fontsize=17)

plt.show()
  ```
![Screen Shot 2024-01-10 at 12 29 22 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/c3669f3f-dc96-4823-a234-5d745ecdd59d)

I found that a higher over quality score, larger living areas, a larger garage that can accomadate more cars, a larger basement area and a larger first floor area tends to be associated with a higher sale price

(recap of catagories)

- 'OverallQual': This represents the overall material and finish quality of the house.
- 'GrLivArea': This refers to the above ground living area in square feet.
- 'GarageCars': This indicates the size of the garage in terms of car capacity.
- 'GarageArea': This represents the size of the garage in square feet.
- 'TotalBsmtSF': This refers to the total square footage of the basement.
- '1stFlrSF': This represents the total square footage of the first floor.

Next, I wanted to take a look at the Top 10 Neighborhoods with Above-Average Property Features. This chart shows the count of occurrences for each neighborhood that has properties with above-average values in various features such as OverallQual, GrLivArea, GarageCars, GarageArea, TotalBsmtSF, and 1stFlrSF.

![Screen Shot 2024-01-10 at 12 40 53 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/3d424cbc-5018-4fa1-b824-d149096eb13f)
