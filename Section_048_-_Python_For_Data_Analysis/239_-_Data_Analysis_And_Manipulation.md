# Pandas Data Manipulation – A Complete Practical Guide

This guide walks you through essential data manipulation tasks in Pandas, using a realistic dataset. We'll cover inspecting data, handling missing values, transforming columns, grouping, aggregating, and merging DataFrames. Every concept is illustrated with code and explanations, ensuring both beginners and advanced users gain clarity.

---

## 1. Loading and Inspecting Data

First, let's load the sample data. The dataset contains sales records with columns: `Date`, `Category`, `Value`, `Product`, `Sales`, `Region`. Some entries have missing values (empty cells).

```python
import pandas as pd
import numpy as np

# For demonstration, we'll read from a string (but you would use pd.read_csv('file.csv'))
data = """Date,Category,Value,Product,Sales,Region
2023-01-01,A,28.0,Product1,754.0,East
2023-01-02,B,39.0,Product3,110.0,North
2023-01-03,C,32.0,Product2,398.0,East
2023-01-04,B,8.0,Product1,522.0,East
2023-01-05,B,26.0,Product3,869.0,North
2023-01-06,B,54.0,Product3,192.0,West
2023-01-07,A,16.0,Product1,936.0,East
2023-01-08,C,89.0,Product1,488.0,West
2023-01-09,C,37.0,Product3,772.0,West
2023-01-10,A,22.0,Product2,834.0,West
2023-01-11,B,7.0,Product1,842.0,North
2023-01-12,B,60.0,Product2,,West
2023-01-13,A,70.0,Product3,628.0,South
2023-01-14,A,69.0,Product1,423.0,East
2023-01-15,A,47.0,Product2,893.0,West
2023-01-16,C,,Product1,895.0,North
2023-01-17,C,93.0,Product2,511.0,South
2023-01-18,C,,Product1,108.0,West
2023-01-19,A,31.0,Product2,578.0,West
2023-01-20,A,59.0,Product1,736.0,East
2023-01-21,C,82.0,Product3,606.0,South
2023-01-22,C,37.0,Product2,992.0,South
2023-01-23,B,62.0,Product3,942.0,North
2023-01-24,C,92.0,Product2,342.0,West
2023-01-25,A,24.0,Product2,458.0,East
2023-01-26,C,95.0,Product1,584.0,West
2023-01-27,C,71.0,Product2,619.0,North
2023-01-28,C,56.0,Product2,224.0,North
2023-01-29,B,,Product3,617.0,North
2023-01-30,C,51.0,Product2,737.0,South
2023-01-31,B,50.0,Product3,735.0,West
2023-02-01,A,17.0,Product2,189.0,West
2023-02-02,B,63.0,Product3,338.0,South
2023-02-03,C,27.0,Product3,,East
2023-02-04,C,70.0,Product3,669.0,West
2023-02-05,B,60.0,Product2,,West
2023-02-06,C,36.0,Product3,177.0,East
2023-02-07,C,2.0,Product1,,North
2023-02-08,C,94.0,Product1,408.0,South
2023-02-09,A,62.0,Product1,155.0,West
2023-02-10,B,15.0,Product1,578.0,East
2023-02-11,C,97.0,Product1,256.0,East
2023-02-12,A,93.0,Product3,164.0,West
2023-02-13,A,43.0,Product3,949.0,East
2023-02-14,A,96.0,Product3,830.0,East
2023-02-15,B,99.0,Product2,599.0,West
2023-02-16,B,6.0,Product1,938.0,South
2023-02-17,B,69.0,Product3,143.0,West
2023-02-18,C,65.0,Product3,182.0,North
2023-02-19,C,11.0,Product3,708.0,North"""

# Load into DataFrame
df = pd.read_csv(pd.compat.StringIO(data))  # in practice: df = pd.read_csv('your_file.csv')
```

Now inspect the data:

```python
# First 5 rows
print(df.head(5))
```

Output (truncated for brevity):
```
         Date Category  Value  Product  Sales Region
0  2023-01-01        A   28.0  Product1  754.0   East
1  2023-01-02        B   39.0  Product3  110.0  North
2  2023-01-03        C   32.0  Product2  398.0   East
3  2023-01-04        B    8.0  Product1  522.0   East
4  2023-01-05        B   26.0  Product3  869.0  North
```

```python
# Last 5 rows
print(df.tail(5))
```

```python
# Summary info (data types, non-null counts)
print(df.info())
```

`info()` shows column names, non‑null counts, and data types. It's essential to spot missing values early.

```python
# Statistical summary for numeric columns
print(df.describe())
```

`describe()` gives count, mean, std, min, quartiles, max for numeric columns. By default, it excludes NaN values, so the count shows the number of non‑null entries.

---

## 2. Handling Missing Values

Missing values are represented as `NaN` (Not a Number). Detecting and handling them is crucial.

### 2.1 Detecting Missing Values

- `df.isnull()` returns a boolean DataFrame where `True` indicates missing.
- `df.isnull().any()` returns a Series: `True` for columns that contain at least one missing.
- `df.isnull().any(axis=1)` returns a Series: `True` for rows that contain at least one missing.
- `df.isnull().sum()` returns the count of missing values per column.

```python
# Check if any column has missing values
print(df.isnull().any())
```

Output:
```
Date        False
Category    False
Value        True
Product     False
Sales        True
Region      False
dtype: bool
```

```python
# Count missing per column
print(df.isnull().sum())
```

Output:
```
Date        0
Category    0
Value       4
Product     0
Sales       5
Region      0
dtype: int64
```

So `Value` has 4 missing, `Sales` has 5 missing.

### 2.2 Filling Missing Values

**Fill all with a constant (e.g., 0):**

```python
df_filled = df.fillna(0)
print(df_filled.head())
```

This creates a new DataFrame; to modify in‑place, use `inplace=True`.

**Fill with the column mean (for numeric columns):**

```python
# Fill missing 'Value' with mean of 'Value'
mean_value = df['Value'].mean()
df['Value_filled'] = df['Value'].fillna(mean_value)
```

Or directly without storing the mean:

```python
df['Value_filled'] = df['Value'].fillna(df['Value'].mean())
```

**Fill 'Sales' with median** (more robust if outliers exist):

```python
df['Sales_filled'] = df['Sales'].fillna(df['Sales'].median())
```

**Forward fill** (carry last observation forward):

```python
df['Sales_ffill'] = df['Sales'].fillna(method='ffill')
```

**Interpolate** (linear interpolation for time series):

```python
df['Value_interpolated'] = df['Value'].interpolate()
```

### 2.3 Creating a New Column After Filling

It's often wise to keep original and filled columns for comparison.

```python
df['Sales_fillNA'] = df['Sales'].fillna(df['Sales'].mean())
print(df[['Sales', 'Sales_fillNA']].head(10))
```

---

## 3. Data Types and Conversion

Check column data types:

```python
print(df.dtypes)
```

Output:
```
Date          object
Category      object
Value        float64
Product       object
Sales        float64
Region        object
dtype: object
```

`Date` is `object` (string). To convert to datetime:

```python
df['Date'] = pd.to_datetime(df['Date'])
```

Now `df['Date'].dtype` will be `datetime64[ns]`.

**Convert numeric columns to integer (after filling missing):**

```python
# First fill missing, then convert to int
df['Value_int'] = df['Value'].fillna(df['Value'].mean()).astype(int)
print(df[['Value', 'Value_int']].head())
```

Note: `astype(int)` works only if no NaN remains. That's why we fill first.

---

## 4. Renaming Columns

Rename columns using a dictionary:

```python
# Rename 'Date' to 'Sale Date'
df = df.rename(columns={'Date': 'Sale Date'})
```

If you want to rename multiple:

```python
df = df.rename(columns={'Value': 'Amount', 'Product': 'Item'})
```

The `rename` method returns a new DataFrame by default; use `inplace=True` to modify the original.

---

## 5. Applying Functions with `apply` and Lambda

`apply` allows you to run a function on each element of a Series or each row/column of a DataFrame.

**Multiply a column by 2 using lambda:**

```python
df['Value_doubled'] = df['Value'].apply(lambda x: x * 2 if pd.notnull(x) else x)
```

**More complex: create a new column based on condition:**

```python
df['Value_category'] = df['Value'].apply(lambda x: 'High' if x > 50 else 'Low')
```

**Apply a custom function:**

```python
def categorize(value):
    if pd.isnull(value):
        return 'Missing'
    elif value > 50:
        return 'High'
    else:
        return 'Low'

df['Value_category2'] = df['Value'].apply(categorize)
```

---

## 6. Data Aggregation and Grouping

Grouping splits the data into groups based on some criteria, then you apply an aggregation function.

**Group by a single column and get mean of another:**

```python
# Mean 'Value' per 'Product'
grouped_mean = df.groupby('Product')['Value'].mean()
print(grouped_mean)
```

**Group by multiple columns and get sum:**

```python
# Total 'Value' per Product and Region
grouped_sum = df.groupby(['Product', 'Region'])['Value'].sum()
print(grouped_sum)
```

**Multiple aggregation functions on a group:**

```python
# For each Region, compute mean, sum, and count of 'Value'
grouped_agg = df.groupby('Region')['Value'].agg(['mean', 'sum', 'count'])
print(grouped_agg)
```

You can also use `agg` with a dictionary to apply different functions to different columns:

```python
df.groupby('Region').agg({
    'Value': ['mean', 'std'],
    'Sales': 'sum'
})
```

---

## 7. Merging and Joining DataFrames

Often you need to combine data from multiple sources.

Let's create two sample DataFrames:

```python
df1 = pd.DataFrame({'Key': ['A', 'B', 'C'], 'Value1': [1, 2, 3]})
df2 = pd.DataFrame({'Key': ['A', 'B', 'D'], 'Value2': [4, 5, 6]})
```

**Inner join** (only keys that appear in both):

```python
merged_inner = pd.merge(df1, df2, on='Key', how='inner')
print(merged_inner)
```
Output:
```
  Key  Value1  Value2
0   A       1       4
1   B       2       5
```

**Outer join** (all keys from both, missing filled with NaN):

```python
merged_outer = pd.merge(df1, df2, on='Key', how='outer')
print(merged_outer)
```
Output:
```
  Key  Value1  Value2
0   A     1.0     4.0
1   B     2.0     5.0
2   C     3.0     NaN
3   D     NaN     6.0
```

**Left join** (keep all keys from left, add matching from right):

```python
merged_left = pd.merge(df1, df2, on='Key', how='left')
print(merged_left)
```
Output:
```
  Key  Value1  Value2
0   A       1     4.0
1   B       2     5.0
2   C       3     NaN
```

**Right join** (keep all keys from right, add matching from left):

```python
merged_right = pd.merge(df1, df2, on='Key', how='right')
print(merged_right)
```
Output:
```
  Key  Value1  Value2
0   A     1.0       4
1   B     2.0       5
2   D     NaN       6
```

---

## 8. Additional Useful Operations

### 8.1 Sorting

```python
# Sort by Sales descending
df_sorted = df.sort_values('Sales', ascending=False)
print(df_sorted[['Product', 'Sales']].head())
```

### 8.2 Filtering Rows

```python
# Rows where Sales > 800 and Region is East
high_sales_east = df[(df['Sales'] > 800) & (df['Region'] == 'East')]
print(high_sales_east)
```

### 8.3 Dropping Duplicates

```python
# Drop duplicate rows based on all columns
df_no_dup = df.drop_duplicates()
```

### 8.4 Pivot Tables

```python
# Average Value per Product per Region
pivot = pd.pivot_table(df, values='Value', index='Product', columns='Region', aggfunc='mean')
print(pivot)
```

---

## 9. Complete Example with the Provided Dataset

Let's apply all the techniques to the original dataset (including missing values) in one flow.

```python
import pandas as pd
import numpy as np

# Load data (already in df)
# 1. Inspect
print("=== Basic Info ===")
print(df.head(2))
print(df.info())
print(df.describe())

# 2. Handle missing values
print("\n=== Missing Values ===")
print(df.isnull().sum())

# Fill Value with mean, Sales with median
df['Value_filled'] = df['Value'].fillna(df['Value'].mean())
df['Sales_filled'] = df['Sales'].fillna(df['Sales'].median())

# 3. Change data type (Date to datetime)
df['Date'] = pd.to_datetime(df['Date'])

# 4. Rename columns
df = df.rename(columns={'Date': 'Transaction Date', 'Value': 'Original Value'})

# 5. Create new column with apply
df['Value_doubled'] = df['Original Value'].apply(lambda x: x * 2 if pd.notnull(x) else x)

# 6. Grouping and aggregation
print("\n=== Grouped Analysis ===")
print(df.groupby('Product')['Original Value'].mean())
print(df.groupby(['Product', 'Region'])['Sales'].sum())

# 7. Merge with a hypothetical discount DataFrame
discount_df = pd.DataFrame({
    'Region': ['East', 'West', 'North', 'South'],
    'Discount': [0.05, 0.10, 0.07, 0.08]
})
merged = pd.merge(df, discount_df, on='Region', how='left')
print("\n=== Merged with discounts ===")
print(merged[['Region', 'Sales', 'Discount']].head())
```

---

## 10. Common Pitfalls and Tips

- **`inplace=True` vs return:** Many Pandas operations return a new DataFrame by default. If you want to modify the original, use `inplace=True` (e.g., `df.dropna(inplace=True)`). However, method chaining is often cleaner without `inplace`.
- **Axis confusion:** `axis=0` (default) means rows, `axis=1` means columns. For `drop`, you must use `axis=1` to drop columns.
- **Missing values in grouping:** By default, `groupby` excludes rows where the grouping key is NaN. To include them, use `dropna=False`.
- **Performance:** Vectorized operations (like `df['col'] * 2`) are much faster than `apply` with lambda. Use `apply` only when necessary.

---

## 11. Summary

Things covered:

- Loading data and inspecting with `head`, `tail`, `info`, `describe`.
- Detecting missing values with `isnull().any()` and `isnull().sum()`.
- Filling missing values using `fillna` with constant, mean, median, forward fill.
- Creating new columns after filling.
- Renaming columns with `rename`.
- Changing data types with `astype`.
- Applying functions using `apply` and lambda.
- Grouping and aggregating with `groupby` and `agg`.
- Merging DataFrames with `merge` (inner, outer, left, right).
- Additional operations like sorting, filtering, pivot tables.
