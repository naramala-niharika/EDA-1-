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

```
# 2. Generate statistical summary :
```
summary = df.describe().T

# Calculate interquartile range (IQR)
summary['IQR'] = summary['75%'] - summary['25%']

# Select relevant statistics
summary = summary[['mean', '50%', 'std', 'IQR']]
summary.columns = ['Mean', 'Median', 'Standard Deviation', 'Interquartile Range']

summary

```
# 3. Data Visualization:


# Create histograms or boxplots to visualize the distributions of various numerical variables.

Histogram : 
```
df.hist(bins=30, figsize=(18, 12), color='pink', edgecolor='black')
plt.suptitle("Histograms of Numerical Features", fontsize=18, y=1.02)
plt.tight_layout() 
plt.show()

```
Box plot : 
```

selected_columns = ['LB', 'ASTV', 'MSTV', 'ALTV', 'MLTV', 'Width']
plt.figure(figsize=(14, 8))
df[selected_columns].boxplot()
plt.title("Boxplots of Selected Features")
plt.xticks(rotation=0)
plt.show()

```
# Use bar charts or pie charts to display the frequency of categories for categorical variables.

Bar chart

df['NSP_Label'] = df['NSP'].map({1: 'Normal', 2: 'Suspect', 3: 'Pathologic'})


```
plt.figure(figsize=(8, 4))
sns.countplot(x='NSP_Label', data=df, palette='Set2')
plt.title('Frequency of Fetal State Classes (NSP)')
plt.xlabel('Fetal State Class')
plt.ylabel('Count')
plt.show()

```
Pie Chart 
```
label_map = {1: 'Normal', 2: 'Suspect', 3: 'Pathologic'}
df['NSP_Label'] = df['NSP'].map(label_map)
class_counts = df['NSP_Label'].value_counts()
colors = ['#595a56', '#41ee22', '#ddff33']

plt.figure(figsize=(5, 5))
patches, texts, autotexts = plt.pie(
    class_counts,
    autopct='%1.1f%%',
    colors=colors,
    startangle=140,
    textprops={'fontsize': 12}
)

plt.legend(patches, class_counts.index, title="Fetal State", loc='best')
plt.title('NSP Class Distribution')
plt.axis('equal')  
plt.tight_layout()
plt.show()




```
### Correlation Heatmap:
```
numeric_df = df.select_dtypes(include=['number'])

plt.figure(figsize=(12, 8))
correlation_matrix = numeric_df.corr()
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', fmt='.2f')
plt.title("Correlation Heatmap of Numeric Features")
plt.show()

```








