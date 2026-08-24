**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


**Program**
~~~

missing_values = df.isnull().sum()
display(missing_values[missing_values > 0])
~~~
~~~

numeric_cols = df.select_dtypes(include=['number'])
print("--- Mean ---")
display(numeric_cols.mean())
print("\n--- Median ---")
display(numeric_cols.median())
print("\n--- Mode ---")
display(numeric_cols.mode().iloc[0])
~~~
~~~
print("--- Standard Deviation ---")
display(numeric_cols.std())
print("\n--- Variance ---")
display(numeric_cols.var())
print("\n--- Interquartile Range (IQR) ---")
iqr = numeric_cols.quantile(0.75) - numeric_cols.quantile(0.25)
display(iqr)
~~~
~~~
from sklearn.preprocessing import StandardScaler
import pandas as pd
numeric_cols_for_scaling = df.select_dtypes(include=['number'])
scaler = StandardScaler()
scaled_numeric_data = scaler.fit_transform(numeric_cols_for_scaling)
df_scaled = pd.DataFrame(scaled_numeric_data, columns=numeric_cols_for_scaling.columns, index=df.index)
print("Scaled Numeric Data (first 5 rows):")
display(df_scaled.head())
~~~
~~~
median_pm25 = df['PM2.5 (µg/m³)'].median()
df['PM2.5 (µg/m³)'].fillna(median_pm25, inplace=True)
print(f"Missing values in 'PM2.5 (µg/m³)' after imputation: {df['PM2.5 (µg/m³)'].isnull().sum()}")
display(df[['PM2.5 (µg/m³)']].head())
~~~
~~~
numeric_cols_for_outliers = df.select_dtypes(include=['number'])
print("\n--- Outlier Detection Summary (IQR Method) ---")
outlier_counts = {}
for column in numeric_cols_for_outliers.columns:
   Q1 = numeric_cols_for_outliers[column].quantile(0.25)
   Q3 = numeric_cols_for_outliers[column].quantile(0.75)
   IQR = Q3 - Q1
   lower_bound = Q1 - 1.5 * IQR
   upper_bound = Q3 + 1.5 * IQR
   outliers = numeric_cols_for_outliers[(numeric_cols_for_outliers[column] < lower_bound) | (numeric_cols_for_outliers[column] > upper_bound)][column]
   outlier_counts[column] = len(outliers)
   if len(outliers) > 0:
        print(f"Column '{column}': {len(outliers)} outliers detected (min: {outliers.min():.2f}, max: {outliers.max():.2f})")
    else:
        print(f"Column '{column}': No outliers detected.")
print("\nSummary of Outlier Counts:")
display(pd.Series(outlier_counts).sort_values(ascending=False))

~~~
~~~
print("Descriptive statistics for PM2.5 (µg/m³):")
display(df['PM2.5 (µg/m³)'].describe())
print("\nQuantiles for PM2.5 (µg/m³):")
display(df['PM2.5 (µg/m³)'].quantile([0.1, 0.25, 0.5, 0.75, 0.9, 0.95, 0.99]))
~~~

~~~
import matplotlib.pyplot as plt
import seaborn as sns
plt.figure(figsize=(10, 6))
sns.kdeplot(df['PM10 (µg/m³)'], fill=True)
plt.title('Density Plot of PM10 (µg/m³) Distribution')
plt.xlabel('PM10 (µg/m³)')
plt.ylabel('Density')
plt.grid(True, linestyle='--', alpha=0.7)
plt.show()

~~~
~~~
import matplotlib.pyplot as plt
import seaborn as sns
plt.figure(figsize=(10, 6))
sns.kdeplot(df['PM10 (µg/m³)'], fill=True)
plt.title('Density Plot of PM10 (µg/m³) Distribution')
plt.xlabel('PM10 (µg/m³)')
plt.ylabel('Density')
plt.grid(True, linestyle='--', alpha=0.7)
plt.show()

~~~
~~~
import matplotlib.pyplot as plt
import seaborn as sns
plt.figure(figsize=(10, 8))
sns.scatterplot(x='PM2.5 (µg/m³)', y='Ozone (µg/m³)', data=df, alpha=0.6)
plt.title('Scatter Plot of PM2.5 vs. Ozone')
plt.xlabel('PM2.5 (µg/m³)')
plt.ylabel('Ozone (µg/m³)')
plt.grid(True, linestyle='--', alpha=0.7)
plt.show()
~~~
**Output**


<img width="233" height="752" alt="image" src="https://github.com/user-attachments/assets/dfafab1a-3944-4980-a648-b7ec9aeb11f6" />
<br>
<img width="248" height="691" alt="image" src="https://github.com/user-attachments/assets/d17b03ca-40e4-42c8-9d43-90f40bf8902a" />
<br>
<img width="299" height="702" alt="image" src="https://github.com/user-attachments/assets/c6257a5f-1812-423b-ae67-45e52eaddb9a" />
<br>
<img width="261" height="712" alt="image" src="https://github.com/user-attachments/assets/ce6d3d98-19c3-4f09-abea-228f4cfa60cb" />
<br>
<img width="275" height="658" alt="image" src="https://github.com/user-attachments/assets/793865a8-ba18-45c8-b576-a0dadc88086c" />
<br>
<img width="312" height="658" alt="image" src="https://github.com/user-attachments/assets/1afe5ff4-f7ae-45bc-9e4a-2aecd8068bb8" />
<br>
<img width="256" height="671" alt="image" src="https://github.com/user-attachments/assets/e92e7eec-4dd4-480a-97a0-ff6e2e7777d5" />
<br>
<img width="1691" height="254" alt="image" src="https://github.com/user-attachments/assets/1d198ee6-0e0d-4a97-bdb8-7bf90b0ffdbb" />
<br>
<img width="198" height="226" alt="image" src="https://github.com/user-attachments/assets/95042d8e-f20e-48e4-ba24-430a32d7193c" />
<br>
<img width="198" height="682" alt="image" src="https://github.com/user-attachments/assets/85282bb6-87ee-44bb-964a-b62c0ab8a095" />
<br>
<img width="759" height="589" alt="image" src="https://github.com/user-attachments/assets/055e7314-7abc-4273-92a3-222a6469d197" />
<br>
<img width="791" height="468" alt="image" src="https://github.com/user-attachments/assets/3bf7d30d-7807-4993-846b-b9946d793365" />
<br>




**Interpretation**

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.



**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


