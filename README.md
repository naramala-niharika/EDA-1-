# EDA-1-
## Aim : EXPLORATORY DATA ANALYSIS ON A DATASET
## Steps : 
 1.	Data Cleaning and Preparation :
```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
df=pd.read_csv("Cardiotocographic.csv")
df.head()
df.info()
df.isnull().sum()
df.duplicated().sum()
df.drop_duplicates(inplace=True)
print(df.columns)
df.fillna(df.mean(numeric_only=True), inplace=True)
df.isnull().sum()
def detect_outliers_iqr(data):
    outlier_indices = {}

    for col in data.select_dtypes(include='number').columns:
        Q1 = data[col].quantile(0.25)
        Q3 = data[col].quantile(0.75)
        IQR = Q3 - Q1
        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR
        outliers = data[(data[col] < lower) | (data[col] > upper)]
        outlier_indices[col] = len(outliers)

    return outlier_indices

outlier_counts = detect_outliers_iqr(df)
print("Outliers per column:")
for col, count in outlier_counts.items():
    print(f"{col}: {count}")


```





