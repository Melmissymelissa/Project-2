<div align="center"><h1>Project 2: Statistical Significance - Pricing Factors - House Prices</h1> </div>

<!-- TABLE OF CONTENTS -->
   ## Table of Contents
   - [Introduction](#introduction)
   - [About The Dataset](#aboutthedataset)
   - [Installation](#installation)
   - [EDA](#EDA)
   - [Analysis](#analysis)
   - [Results](#results)
   - [Recommendations](#recommendations)

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
To first start off my exploratory data analysis, I first wanted to take a closer look at the categories that are highly associated with a higher sale price. By utilizing the 'saleprice_corr' variable, I examined the categories that exhibit a strong correlation with a higher sale price, allowing me to gain a deeper understanding of the factors that significantly influence the sale price in the dataset. 
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

![Screen Shot 2024-01-10 at 12 52 48 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/5216047a-c464-406e-beee-e7a5f051bc80)

After seeing the neighborhoods that have the most above-average property features, I wanted to compare them to the top 10 neighborhoods with the highest average price per sq ft

![Screen Shot 2024-01-10 at 12 57 59 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/8739ef2d-3c52-49b4-9e0b-ac39b414e0c8)

I also wanted to know: 
What are the top 10 neighborhoods with the most count of homes with an above average Overallqual score

![Screen Shot 2024-01-10 at 1 05 27 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/91c41619-a7ab-426f-a4d3-1b7303e4493d)

## Analysis
In my analysis, I conducted multiple t-tests to determine whether there existed a significant difference in price per sq ft between homes categorized as above average and those categorized as below average. These tests were performed across multiple categories.

### Hypothesis 1 - There is a significant difference in price per sq ft between above average GrLivArea homes and below average GrLivArea homes. Ha: μ1 - μ2 ≠ 0
```python
# splitting the data for a t-test into 2 samples on GrLivArea
above_avg_GrLivArea_df = hp_df[hp_df['GrLivArea'] > hp_df['GrLivArea'].mean()]  # sample of above average GrLivArea
below_avg_GrLivArea_df = hp_df[hp_df['GrLivArea'] <= hp_df['GrLivArea'].mean()]  # sample of below average GrLivArea

# running t-test
t_statistic, p_value = stats.ttest_ind(above_avg_GrLivArea_df["Price per sq ft"], below_avg_GrLivArea_df["Price per sq ft"])

# Calculate the confidence interval
ci = stats.t.interval(0.95, len(above_avg_GrLivArea_df["Price per sq ft"]) + len(below_avg_GrLivArea_df["Price per sq ft"]) - 2,
                      loc=above_avg_GrLivArea_df["Price per sq ft"].mean() - below_avg_GrLivArea_df["Price per sq ft"].mean(),
                      scale=stats.sem(above_avg_GrLivArea_df["Price per sq ft"]) + stats.sem(below_avg_GrLivArea_df["Price per sq ft"]))

x = ['Below Avg GrLivArea', 'Above Avg GrLivArea']
y = [mean1, mean2]
error = [margin_error, margin_error]

colors = ['blue', 'orange'] 

plt.bar(x, y, yerr=error, color=colors)  
plt.xlabel('GrLivArea')
plt.ylabel('Price per sq ft')
plt.title('Mean Price per sq ft of Below and Above Average GrLivArea Homes', fontsize=30)

for i, v in enumerate(y):
    plt.text(i, v + 1.5, str(round(v, 2)), ha='center', va='bottom', fontsize=24)
plt.show()
print("T-Statistic:", t_statistic)
print("P-Value:", p_value)
print("Confidence Interval (95%):", ci)
  ```
![Screen Shot 2024-01-10 at 3 44 07 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/3fc7b969-1c18-4b24-85c9-32c33686e606)

The results of the t-test indicate a statistically significant difference in the price per sq ft between homes with above-average and below-average GrLivArea. The confidence interval (0.17, 3.70) suggests that, on average, homes with above-average GrLivArea have a price per sq ft that is between approximately 0.17 and 3.70 higher than homes with below-average GrLivArea. The low p-value (0.0028) provides strong evidence against the null hypothesis, supporting the conclusion of a significant difference in prices.

### Hypothesis 2 - There is a significant difference in price per sq ft between above average GarageCars homes and below average GarageCars homes. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-10 at 3 52 53 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/b6387bee-381d-4bb8-9d19-40ba342d2c5c)

The results of the analysis indicate a statistically significant difference in the average price per square foot between houses with below average GarageCars and houses with above average GarageCars. The confidence interval of (3.3 - 6.9) suggests that, with 95% confidence, the average price per square foot for houses with above average GarageCars is estimated to be higher by approximately 3.34 to 6.90 compared to houses with below average GarageCars. Additionally, the very small p-value of 1.72e-13 provides strong evidence against the null hypothesis, indicating that the observed difference is unlikely to have occurred by chance alone.

### Hypothesis 3 - There is a significant difference in price per sq ft between above average GarageArea homes and below average GarageArea homes. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-10 at 3 56 25 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/a9a1adeb-529b-43d1-927b-fd6c783b5a7a)

The results of the analysis indicate a statistically significant difference in the average price per square foot between houses with below average GarageArea and houses with above average GarageArea. The confidence interval of (0.35-2.91) suggests that, with 95% confidence, the average price per square foot for houses with above average GarageArea is estimated to be higher by approximately 0.36 to 3 dollars compared to houses with below average GarageArea. Additionally, the p-value of 0.0117 provides strong evidence against the null hypothesis, indicating that the observed difference is unlikely to have occurred by chance alone.

### Hypothesis 4 - There is a significant difference in price per sq ft between above average TotalBsmtSF homes and below average TotalBsmtSF homes. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-10 at 3 58 31 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/a9d2f057-912e-4c89-ba62-7d65ec67a4c4)

The results indicate a statistically significant difference in the average price per square foot between houses with below average TotalBsmtSF and houses with above average TotalBsmtSF. The confidence interval of (1.32-3.8) suggests that, with 95% confidence, the average price per square foot for houses with above average TotalBsmtSF is estimated to be higher by approximately 1 to 4 dollars compared to houses with below average TotalBsmtSF. Additionally, the small p-value of 7.89e-05 provides strong evidence against the null hypothesis, indicating that the observed difference is unlikely to have occurred by chance alone.

### Hypothesis 5 - There is a significant difference in price per sq ft between above average 1stFlrSF homes and below average 1stFlrSF homes. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-10 at 4 01 45 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/744fdb54-20b0-43e2-82e3-564f4aed53a3)

The results indicate a statistically significant difference in the average price per square foot between houses with below average 1stFlrSF and houses with above average 1stFlrSF. The confidence interval of (0.52-3.04) suggests that, with 95% confidence, the average price per square foot for houses with above average 1stFlrSF is estimated to be higher by approximately 0.50-3 dollars compared to houses with below average 1stFlrSF. Additionally, the small p-value of 0.00658 provides strong evidence against the null hypothesis, indicating that the observed difference is unlikely to have occurred by chance alone.

### Hypothesis 6 - There is a significant difference in price per sq ft between above average condition homes and below average condition homes. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-10 at 4 35 17 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/010e2a55-db29-42b2-809e-6c383f62535f)

With 95% confidence, there is a statistically significant difference in the average price per square foot between above average condition homes and below condition homes. The confidence interval (CI) of 3.16 to 5.5 suggests that, on average, the price per square foot for below average condition homes is higher by that amount compared to above average condition homes

### Hypothesis 7 - There is a significant difference in price per sq ft between above average quality homes and below average quality homes. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-10 at 4 49 32 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/bbc101d6-0130-4739-962b-2acc82c22cee)

The results indicate a statistically significant difference in the average price per square foot between houses with above average overall quality score and houses with below average overall quality score. The confidence interval of (7.32-9.86) suggests that, with 95% confidence, the average price per square foot for houses with above average overall quality score is estimated to be higher by approximately 7-10 dollars compared to houses with below average overall quality score. Additionally, the small p-value of 6.447839019158312e-40 provides strong evidence against the null hypothesis, indicating that the observed difference is unlikely to have occurred by chance alone.

### Hypothesis 8 - There is a significant difference in the mean price per square foot between above-average quality homes in the top 10 neighborhoods and other homes. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-10 at 4 55 52 PM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/30d2099c-aec6-47df-854a-29c7c7047720)

The results indicate a statistically significant difference in the average price per square foot between above average quality homes in the top neighbordhoods and the other homes. The confidence interval of (5.9-8.4) suggests that, with 95% confidence, the average price per square foot for houses with above average overall quality homes in the top neighborhoods is estimated to be higher by approximately 6-8 dollars compared to other homes. Additionally, the small p-value of 1.7714406722025997e-27 provides strong evidence against the null hypothesis, indicating that the observed difference is unlikely to have occurred by chance alone.

### Hypothesis 9 - There is a signigicant difference in the mean price per sq ft between homes built after the year 2000 and homes built before the year 2000. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-11 at 10 51 30 AM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/631b1c8f-3dba-48d5-b56d-73ae1d1e95af)

The results indicate a statistically significant difference in the average price per square foot between homes built after the year 2000 and homes built before the year 2000. The confidence interval of (8.15, 11.04) suggests that, with 95% confidence, the average price per square foot for homes built after the year 2000 is estimated to be higher by approximately 8-11 dollars compared to homes built before the year 2000. Additionally, the small p-value of 1.166027845190789e-39 provides strong evidence against the null hypothesis, indicating that the observed difference is unlikely to have occurred by chance alone.

### Hypothesis 10 - There is a significant difference in the mean price per square foot between above-average quality homes in the top 10 neighborhoods bulit after 2000 and above-average quality homes in the top 10 neighborhoods bulit before 2000. Ha: μ1 - μ2 ≠ 0

![Screen Shot 2024-01-11 at 10 53 31 AM](https://github.com/Melmissymelissa/Project-2-Statistical-Significance---Pricing-Factors---House-Prices/assets/142250108/00feb80e-4f61-4840-88d7-3e3811ac8c30)

The t-test results show a significant difference in the mean price per square foot between above-average quality homes in the top 10 neighborhoods built after 2000 and above-average quality homes in the top 10 neighborhoods built before 2000. The t-statistic of 3.705 indicates a positive difference, suggesting that homes built after 2000 have a higher mean price per square foot. The p-value of 0.000242 suggests strong evidence against the null hypothesis. The 95% confidence interval of (1.865, 6.771) indicates that the mean price per square foot for homes built after 2000 is likely to be between 2-7 dollars higher than for homes built before 2000.

## Results
After conducting statistical analysis (t-tests) on 10 hypotheses, it can be concluded that the following factors contribute to a higher price per square foot:

- Above-average overall quality score - This suggests that buyers are willing to pay more for homes that are built with better materials and have superior craftsmanship.
- Larger-than-average living areas - This could be because larger living spaces provide more comfort and functionality, which is valued by buyers.
- A garage that can accommodate more cars than the average - This suggests that having ample garage space is considered desirable by buyers, potentially for storing multiple vehicles or providing additional storage space.
- A basement area larger than the average - Basements can provide additional living space or storage, and buyers may be willing to pay more for homes with larger basement areas.
- A first floor larger than the average - This could be because larger first floors offer more living space and flexibility in terms of room layout and functionality
- Below-average overall condition score - This could be due to factors such as historical significance, unique architectural features, or potential for renovation and improvement.
- Homes built after the year 2000 - This suggests that newer homes, with modern amenities and construction standards, are valued more by buyers.

Furthermore, by focusing on above-average quality homes built after the year 2000 in the top neighborhoods, the analysis found that this combination can further contribute to a higher price per square foot. This indicates that buyers are willing to pay a premium for newer, high-quality homes located in desirable neighborhoods.

## Recommendations
I can conclude that homes buyers in Ames, Iowa prioritize well-built homes with superior craftsmanship and materials. They also value space and are willing to pay more for homes that offer ample living and storage space, and may be willing to overlook some condition issues if the home possesses other desirable features or characteristics. However, these buyers tend to have a preference for newer construction, which may offer modern amenities, updated designs, and adherence to current building standards. Lastly, these buyers are willing to pay a premium for homes located in desirable neighborhoods with a reputation for quality and desirability. Overall, these conclusions suggest that home buyers prioritize quality, space, newer construction, and desirable locations when making purchasing decisions. With that being said, I recommend we:

   1. Focus on high quality properties - conducting thorough due diligence on the underlying properties and ensuring they meet certain quality criteria.
   2. Consider property size
   3. Access proerty condition
   4. Consider market trends - Monitor market trends and consider the demand for newer homes
   5. Diversify the portfolio - Investing in a mix of mortgage-backed securities backed by properties with different characteristics, such as varying quality scores, sizes, conditions, and construction years. Diversification can help mitigate the impact of any specific property or market-related risks

