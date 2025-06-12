# EDA-1-
## Aim : EXPLORATORY DATA ANALYSIS ON A DATASET
## Program : 
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
# Function to remove outliers using IQR
def remove_outliers_iqr(data):
    numeric_cols = data.select_dtypes(include='number').columns
    for col in numeric_cols:
        Q1 = data[col].quantile(0.25)
        Q3 = data[col].quantile(0.75) 
        IQR = Q3 - Q1
        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR
        # Keep only rows within the IQR range
        data = data[(data[col] >= lower) & (data[col] <= upper)]
    return data

# Apply outlier removal
df_cleaned = remove_outliers_iqr(df)
print("Original shape:", df.shape)
# Generate statistical summary
summary = df.describe().T

# Calculate interquartile range (IQR)
summary['IQR'] = summary['75%'] - summary['25%']

# Select relevant statistics
summary = summary[['mean', '50%', 'std', 'IQR']]
summary.columns = ['Mean', 'Median', 'Standard Deviation', 'Interquartile Range']

summary

```
# Create histograms or boxplots to visualize the distributions of various numerical variables.

 ##Histogram 

df.hist(bins=30, figsize=(18, 12), color='pink', edgecolor='black')
plt.suptitle("Histograms of Numerical Features", fontsize=18, y=1.02)
plt.tight_layout()
plt.show()

```






