# Unemployment in India Analysis

## Project Overview

This project presents an exploratory data analysis (EDA) of unemployment trends in India using monthly labour market data. The analysis focuses on:

* Unemployment rate trends over time
* Regional unemployment variations
* Rural and urban employment differences
* Seasonal unemployment patterns
* The impact of the COVID-19 pandemic on employment

The dataset includes:

* Region
* Date
* Frequency
* Estimated Unemployment Rate (%)
* Estimated Employed Population
* Labour Participation Rate (%)
* Area Type (Rural/Urban)

---

# Dataset Source

Dataset obtained from Kaggle:

[Kaggle – Unemployment in India Dataset](https://www.kaggle.com/datasets/gokulrajkmv/unemployment-in-india?utm_source=chatgpt.com)

---

# Technologies and Libraries Used

The project was implemented using Python and the following libraries:

* `pandas` — data manipulation and analysis
* `numpy` — numerical operations
* `matplotlib` — data visualization
* `seaborn` — advanced statistical visualization
* `kagglehub` — dataset download and access
* `os` — file path management

---

# Environment Setup

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import kagglehub
import os
```

---

# Dataset Download and Loading

```python
# Download dataset from Kaggle
path = kagglehub.dataset_download("gokulrajkmv/unemployment-in-india")

# Define dataset path
file_path = os.path.join(path, 'Unemployment in India.csv')

# Load dataset
data = pd.read_csv(file_path)
```

---

# Initial Data Inspection

Initial exploration of the dataset included:

```python
data.head()
data.tail()
data.info()
data.describe()
```

The inspection process helped identify:

* Dataset structure
* Column names
* Data types
* Statistical summaries
* Potential missing values

---

# Data Cleaning

Several preprocessing steps were performed to prepare the dataset for analysis.

## 1. Remove Extra Spaces from Column Names

```python
data.columns = data.columns.str.strip()
```

## 2. Rename Columns for Readability

```python
data.rename(columns={
    'Estimated Unemployment Rate (%)': 'Unemployment_Rate',
    'Estimated Employed': 'Employed',
    'Estimated Labour Participation Rate (%)': 'Labour_Participation_Rate'
}, inplace=True)
```

## 3. Convert Date Column to Datetime Format

```python
data['Date'] = pd.to_datetime(data['Date'])
```

## 4. Check Missing Values

```python
data.isnull().sum()
```

The dataset contained minimal missing values, which were removed during preprocessing.

```python
data.dropna(inplace=True)
```

---

# Exploratory Data Analysis (EDA)

## Overall Unemployment Trends

The average unemployment rate across all observations was calculated to understand the general employment condition in India.

```python
average_rate = data['Unemployment_Rate'].mean()
print(average_rate)
```

A time-series visualization was created to observe unemployment fluctuations over time.

```python
plt.figure(figsize=(12,6))
plt.plot(data['Date'], data['Unemployment_Rate'])

plt.title('Unemployment Rate Over Time')
plt.xlabel('Date')
plt.ylabel('Unemployment Rate (%)')

plt.xticks(rotation=45)
plt.show()
```

### Observation

The unemployment rate showed noticeable fluctuations across different periods, with a significant spike during the COVID-19 pandemic.

---

# COVID-19 Impact Analysis

To analyze the effect of COVID-19:

* Pre-COVID period: Before March 2020
* COVID period: March 2020 onwards

## Calculate Average Rates

```python
before_covid = data[data['Date'] < '2020-03-01']['Unemployment_Rate'].mean()

during_covid = data[data['Date'] >= '2020-03-01']['Unemployment_Rate'].mean()
```

## Visualization

```python
plt.figure(figsize=(12,6))

plt.plot(data['Date'], data['Unemployment_Rate'])

plt.axvline(pd.to_datetime('2020-03-01'),
            color='red',
            linestyle='--',
            label='COVID-19 Start')

plt.legend()

plt.title('COVID-19 Impact on Unemployment')
plt.xlabel('Date')
plt.ylabel('Unemployment Rate (%)')

plt.show()
```

### Observation

A sharp increase in unemployment was observed during the COVID-19 lockdown period due to:

* Business shutdowns
* Reduced economic activity
* Workforce displacement

---

# Regional Analysis

The analysis identified regions with the highest unemployment rates.

## Top 10 Regions

```python
top_regions = data.groupby('Region')['Unemployment_Rate'] \
                  .mean() \
                  .sort_values(ascending=False) \
                  .head(10)
```

## Visualization

```python
top_regions.plot(kind='bar', figsize=(10,6))

plt.title('Top 10 Regions by Unemployment Rate')
plt.ylabel('Average Unemployment Rate (%)')

plt.show()
```

### Observation

Some regions consistently experienced higher unemployment levels due to:

* Economic inequality
* Industrial concentration
* Population density
* Labour market instability

---

# Rural vs Urban Analysis

The unemployment differences between rural and urban areas were examined.

```python
area_analysis = data.groupby('Area')['Unemployment_Rate'].mean()

print(area_analysis)
```

## Visualization

```python
area_analysis.plot(kind='bar')

plt.title('Rural vs Urban Unemployment')
plt.ylabel('Average Unemployment Rate (%)')

plt.show()
```

### Observation

Urban unemployment rates were generally higher during the pandemic because urban economies depend heavily on:

* Service industries
* Retail businesses
* Corporate employment

Rural areas showed relatively stable employment due to agricultural activities.

---

# Seasonal Trend Analysis

Monthly unemployment trends were analyzed to identify seasonal patterns.

## Extract Month

```python
data['Month'] = data['Date'].dt.month
```

## Monthly Average Analysis

```python
monthly_trend = data.groupby('Month')['Unemployment_Rate'].mean()
```

## Visualization

```python
monthly_trend.plot(marker='o', figsize=(10,5))

plt.title('Seasonal Unemployment Trend')
plt.xlabel('Month')
plt.ylabel('Average Unemployment Rate (%)')

plt.grid()

plt.show()
```

### Observation

Seasonal patterns suggest:

* Temporary employment fluctuations
* Agricultural employment cycles
* Economic seasonality across industries

---

# Correlation Analysis

Relationships between unemployment, employment, and labour participation were examined.

## Correlation Matrix

```python
correlation = data[[
    'Unemployment_Rate',
    'Employed',
    'Labour_Participation_Rate'
]].corr()

print(correlation)
```

## Heatmap Visualization

```python
sns.heatmap(correlation,
            annot=True,
            cmap='coolwarm')

plt.title('Correlation Matrix')

plt.show()
```

### Observation

Key relationships identified:

* Employment and unemployment are negatively correlated
* Labour participation influences unemployment trends
* Higher employment levels correspond to lower unemployment rates

---

# Key Findings

## 1. COVID-19 Severely Affected Employment

The pandemic caused a major increase in unemployment due to lockdown restrictions and economic disruption.

## 2. Urban Areas Were More Vulnerable

Urban regions experienced higher unemployment because of dependency on service-sector jobs.

## 3. Seasonal Employment Patterns Exist

Certain months recorded predictable changes in unemployment linked to agriculture and temporary work cycles.

## 4. Regional Differences Are Significant

Some states consistently showed higher unemployment levels, indicating uneven economic development.

---

# Policy Recommendations

Based on the analysis, the following recommendations are suggested:

## Employment Support Programs

Governments can introduce:

* Public employment schemes
* Youth job initiatives
* SME financial support

## Skill Development Initiatives

Investment in:

* Technical education
* Digital literacy
* Vocational training

## Rural Economic Development

Support for:

* Agricultural modernization
* Rural entrepreneurship
* Small-scale industries

## Crisis Preparedness

Develop:

* Social protection systems
* Emergency wage support
* Workforce recovery programs

---

# Conclusion

This project successfully analyzed unemployment trends in India using Python-based data analytics techniques. The findings revealed substantial regional, seasonal, and pandemic-related variations in unemployment patterns.

The COVID-19 pandemic had a major impact on the labour market, particularly in urban regions. Seasonal employment cycles and regional economic differences also played important roles in unemployment fluctuations.

The analysis demonstrates how data science and visualization techniques can support economic planning and policy decision-making.
