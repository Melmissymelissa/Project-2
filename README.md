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
