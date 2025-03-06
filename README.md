# Overview

Welcome to my analysis of the data layoffs, focusing on the impact of layoffs accross industries. This project was created out of a desire to navigate and understand the job market more effectively. 

In light of recent layoffs across various industries, this analysis incorporates data on job market trends and the impact of these layoffs . By examining the dataset, I aim to provide a comprehensive understanding of the current job market landscape, highlighting which industry have the highest and more.

# The Questions

Below are the questions I want to answer in my project:

1. How has the percentage of layoffs varied across different industries?
2. How have layoffs varied throughout the year?
3. How have companies raised funds from different sources?

# Tools I Used

For my deep dive into the layoffs dataset, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```
import pandas as pd
import ast
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
df=pd.read_csv(r"C:\Users\aumee\Downloads\layoffs.csv")
```
For cleanup:

```
def change(x):
    if isinstance(x, str) and 'Crypto' in x:
        return 'Crypto'
    return x
df['industry'] = df['industry'].apply(change)

def change2(x):
    if 'Unknown' in x:
        return np.nan
    else :
        return x
df['stage']=df['stage'].astype(str).apply(change2)

def change3(x):
    if 'United States' in x:
        return 'United States'
    return x
df['country'] = df['country'].apply(change3)

df['date']=pd.to_datetime(df['date'])

```

# The Analysis

Each cell aimed at investigating specific aspects of the data. Here’s how I approached each question:

## 1. How has the percentage of layoffs varied across different industries?

To find the percentage of layoffs across different industries.I identified the top six industries with the highest number of layoffs and filtered the dataset accordingly. Using a boxplot, I visualized the percentage of layoffs in each of these top industries. This helps compare layoff percentages across the top industries at a glance.

### Visualise Data

```
plot3= df['industry'].value_counts().head(6)
top_industries = plot3.index.tolist()
df_filtered = df[df['industry'].isin(top_industries)].copy()
plt.figure(figsize=(10, 4))
sns.boxplot(data=df_filtered, x='percentage_laid_off',y='industry',hue='industry',legend=False)
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'{x*100:.0f}%'))
plt.ylabel('Industries',size=15)
plt.xlabel('Percentage',size=15)
plt.title('Percentage Layoff In Each Industries',size=20)
plt.show()
```

### Results:

![Screenshot 2025-02-15 175751](https://github.com/user-attachments/assets/eccfec7c-3966-48f1-b41c-e4731657e7ce)

### Insights:

* Varied Impact: Layoff percentages differ significantly across industries.
* Food Sector High: Food industry shows the highest median layoff percentage.
* Healthcare Lowest: Healthcare industry has the lowest and most consistent layoff percentages.
* Mid-Range Variation: Other industries (Retail, Transportation, Marketing, Finance) fall in between with varying degrees of layoff impact.

## 2. How have layoffs varied throughout the year?

I created a plot to visualize the median total layoffs throughout the year by month. I grouped the data by month, calculated the median number of layoffs, and converted the month numbers to their corresponding names. Using a line plot, I displayed the total layoffs per month, with green lines, red star markers, and no legend. This visualization highlights the monthly trend in layoffs, helping identify patterns across the year.

### Visualize Data

```
plot4=df.groupby('month')['total_laid_off'].median().reset_index()
plot['month_name'] = plot['month'].apply(lambda x: pd.to_datetime(x,format='%m').strftime('%b'))
plot.plot(kind='line',x='month_name',y='total_laid_off',color='green',marker='*',markerfacecolor='red',markersize=10,legend=False)
plt.title('Total Layoff Throught The Year ',size=20)
plt.ylabel('No. Of Layoff Per Month',size=15)
plt.xlabel('Months',size=15)
```
### Results:

![Screenshot 2025-02-15 181212](https://github.com/user-attachments/assets/33f90366-ead8-4a47-b393-6fef5b43809f)

### Insights:

* Fluctuating Layoffs: Layoffs varied significantly throughout the year.
* Early Peak: Layoffs started high, peaking in January.
* Mid-Year Dip:  A significant drop occurred mid-year, reaching a low in July.
* Late Rise: Layoffs rose again towards the end of the year, peaking in November.
* Overall Trend:  A U-shaped pattern is observed in the layoff trend.

## 2. How have companies raised funds from different sources? 

I created a plot to visualize the median funds raised by top companies at different stages. By grouping the data by company and stage, and merging it with industry information. This plot shows the funds raised (in USD) by each company, with different colors representing various industries.This visualization highlights the distribution of funds raised by companies from different sources.

### Visualise Data

```
plot5 = df.groupby(['company', 'stage'])['funds_raised_millions'].median().sort_values(ascending=False).head(10)[1:].reset_index(name='Funds')
temp_df = df[['industry', 'company']].drop_duplicates()
final_plot = plot5.merge(temp_df, how='inner')

plt.figure(figsize=(10,5))
sns.scatterplot(data=final_plot, x='Funds', y='company', hue='industry', legend=True)
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x / 100)}M'))

from adjustText import adjust_text
texts = []

for i, row in final_plot.iterrows():
    company = row['company']
    stage = row['stage']
    funds = row['Funds']
    texts.append(plt.text(funds, company, stage))

adjust_text(texts)
plt.legend(loc='upper left')
plt.ylabel('Companies' ,size=15)
plt.xlabel('Funds (USD)' ,size=15)
plt.title('Funds Raised By Companies From Differenet Sources' ,size=20)
plt.show()

```

### Results:

![Screenshot 2025-02-15 181826](https://github.com/user-attachments/assets/8e1d91ac-f95d-4854-9121-b3b10b568c93)

### Insights:

* Funding Sources Varied: Companies raised funds from diverse sources.
* IPO Common:  Post-IPO funding is prevalent among many companies.
* Series Funding: WeWork utilized Series H funding rounds.
* Acquisition: Flipkart was acquired, representing a different funding path.
* Funding Amounts Differ: Total funds raised vary significantly across companies.

## Conclusion 
In summary, the analysis of layoffs reveals varying impacts across industries, with the food sector facing the highest layoffs and healthcare showing resilience. Layoffs fluctuated throughout the year, peaking in January and November while dipping mid-year. Companies employed diverse fundraising strategies, reflecting varied approaches to financial stability. These trends emphasize the need for adaptability in navigating economic challenges.






 


