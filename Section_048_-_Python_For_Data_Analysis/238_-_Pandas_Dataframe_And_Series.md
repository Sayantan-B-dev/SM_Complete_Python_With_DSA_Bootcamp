# Pandas – A Complete Guide to DataFrame and Series

Pandas is the go‑to library for data manipulation and analysis in Python. It provides two core data structures:

- **Series**: a one‑dimensional labeled array (like a column in a table)
- **DataFrame**: a two‑dimensional labeled data structure (like a spreadsheet or SQL table)

This guide will take you from the basics to more advanced operations, with plenty of examples and explanations. We'll cover installation, creation, indexing, manipulation, and key methods.

---

## 1. Installing Pandas and NumPy

You can install Pandas along with NumPy using `pip` and a `requirements.txt` file.

**requirements.txt**:
```
pandas
numpy
```

Then run:
```bash
pip install -r requirements.txt
```

Alternatively, install them directly:
```bash
pip install pandas numpy
```

If you use Anaconda, they are often pre‑installed; otherwise:
```bash
conda install pandas numpy
```

---

## 2. Importing Pandas

By convention, Pandas is imported as `pd`:

```python
import pandas as pd
import numpy as np   # often used together
```

---

## 3. Series – The One‑Dimensional Array

A **Series** is a one‑dimensional labeled array capable of holding any data type (integers, strings, floats, Python objects, etc.). The labels (indices) can be any hashable type.

### 3.1 Creating a Series

**From a list** (default index = 0, 1, 2, ...):
```python
data = [1, 2, 3, 4, 5]
series = pd.Series(data)
print(series)
```
Output:
```
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

**From a dictionary** (keys become the index):
```python
data = {'a': 1, 'b': 2, 'c': 3}
series_dict = pd.Series(data)
print(series_dict)
```
Output:
```
a    1
b    2
c    3
dtype: int64
```

**With custom index** using `index` parameter:
```python
data = [10, 20, 30]
index = ['a', 'b', 'c']
series_custom = pd.Series(data, index=index)
print(series_custom)
```
Output:
```
a    10
b    20
c    30
dtype: int64
```

### 3.2 Series Attributes and Methods

- `series.values` – NumPy array of the data
- `series.index` – the index object
- `series.dtype` – data type
- `series.shape` – tuple (size,)
- `series.size` – number of elements
- `series.head(n)` – first n rows
- `series.tail(n)` – last n rows

Example:
```python
print(series.values)      # [1 2 3 4 5]
print(series.index)       # RangeIndex(start=0, stop=5, step=1)
print(series.dtype)       # int64
print(series.shape)       # (5,)
print(series.size)        # 5
```

### 3.3 Vectorized Operations on Series

Like NumPy, Series support element‑wise operations:
```python
s = pd.Series([1, 2, 3])
print(s + 10)      # 11, 12, 13
print(s * 2)       # 2, 4, 6
print(np.sqrt(s))  # 1.0, 1.414, 1.732
```

---

## 4. DataFrame – The Two‑Dimensional Table

A **DataFrame** is a two‑dimensional, size‑mutable, potentially heterogeneous tabular data structure with labeled axes (rows and columns). It can be thought of as a collection of Series sharing the same index.

### 4.1 Creating a DataFrame

**From a dictionary of lists** (each list becomes a column):
```python
data = {
    'Name': ['Krish', 'John', 'Jack'],
    'Age': [25, 30, 45],
    'City': ['Bangalore', 'New York', 'Florida']
}
df = pd.DataFrame(data)
print(df)
```
Output:
```
    Name  Age       City
0  Krish   25  Bangalore
1   John   30   New York
2   Jack   45    Florida
```

**From a list of dictionaries** (each dict becomes a row):
```python
data = [
    {'Name': 'Krish', 'Age': 32, 'City': 'Bangalore'},
    {'Name': 'John', 'Age': 34, 'City': 'Bangalore'},
    {'Name': 'Bappy', 'Age': 32, 'City': 'Bangalore'},
    {'Name': 'Jack', 'Age': 32, 'City': 'Bangalore'}
]
df = pd.DataFrame(data)
print(df)
```
Output:
```
    Name  Age       City
0  Krish   32  Bangalore
1   John   34  Bangalore
2  Bappy   32  Bangalore
3   Jack   32  Bangalore
```

**From a NumPy array**:
```python
arr = np.array([[1, 2, 3], [4, 5, 6]])
df_np = pd.DataFrame(arr, columns=['A', 'B', 'C'])
print(df_np)
```
Output:
```
   A  B  C
0  1  2  3
1  4  5  6
```

### 4.2 Reading Data from CSV (Real‑World Scenario)

In practice, you'll often load data from a file:
```python
df = pd.read_csv('sales_data.csv')
df.head(5)   # first 5 rows
df.tail(5)   # last 5 rows
```

### 4.3 DataFrame Attributes and Methods

- `df.shape` – (rows, columns)
- `df.columns` – column names
- `df.index` – row labels
- `df.dtypes` – data types of each column
- `df.info()` – concise summary
- `df.describe()` – statistical summary for numeric columns
- `df.head(n)`, `df.tail(n)`
- `df.values` – NumPy array representation

Example:
```python
print("Shape:", df.shape)
print("Columns:", df.columns)
print("Data types:\n", df.dtypes)
print("Statistical summary:\n", df.describe())
```

---

## 5. Accessing Data in a DataFrame

Pandas provides several ways to access data: by column, by row, by label, or by position.

### 5.1 Selecting Columns

A single column returns a Series:
```python
df['Name']          # Series
type(df['Name'])    # pandas.core.series.Series
```

Multiple columns return a DataFrame:
```python
df[['Name', 'Age']]   # DataFrame
```

### 5.2 Selecting Rows with `loc` (by label) and `iloc` (by position)

- `df.loc[row_label]` – returns a Series (row) by index label.
- `df.iloc[row_position]` – returns a Series by integer position.

```python
df.loc[0]       # first row (by label)
df.iloc[0]      # first row (by integer index)
```

You can also slice:
```python
df.loc[0:2]     # rows 0,1,2 (inclusive of end)
df.iloc[0:2]    # rows 0 and 1 (exclusive of end)
```

### 5.3 Accessing a Specific Element

- `df.at[row_label, col_label]` – fast label‑based scalar access.
- `df.iat[row_position, col_position]` – fast integer‑based scalar access.

```python
df.at[2, 'Age']        # value at row index 2, column 'Age'
df.iat[2, 2]           # value at row 2, column 2 (if column index 2 is third column)
```

### 5.4 Boolean Indexing (Filtering)

Select rows based on a condition:
```python
df[df['Age'] > 30]                     # rows where Age > 30
df[(df['Age'] > 30) & (df['City'] == 'Bangalore')]   # multiple conditions
```

---

## 6. Data Manipulation

### 6.1 Adding a Column

Simply assign a new column name to a list, Series, or array:
```python
df['Salary'] = [50000, 60000, 70000]   # must match number of rows
```

You can also create a column based on existing ones:
```python
df['Bonus'] = df['Salary'] * 0.1
```

### 6.2 Removing a Column

Use `df.drop()` with `axis=1` (axis 0 = rows, axis 1 = columns). By default, `drop` returns a new DataFrame without modifying the original. To modify in‑place, set `inplace=True`.

```python
df.drop('Salary', axis=1, inplace=True)   # removes 'Salary' column
```

**Why `axis=1`?**  
- `axis=0` refers to rows (index axis)
- `axis=1` refers to columns  
If you omit `axis`, it defaults to `axis=0`, so `df.drop('Salary')` would try to drop a row labeled 'Salary', which may not exist.

### 6.3 Removing a Row

```python
df.drop(0, inplace=True)   # drops row with index label 0
```

### 6.4 Modifying Values in a Column

```python
df['Age'] = df['Age'] + 1   # add 1 to every Age
```

### 6.5 Renaming Columns

```python
df.rename(columns={'Name': 'Full Name'}, inplace=True)
```

---

## 7. Handling Missing Data

Pandas uses `NaN` (Not a Number) for missing data. Common methods:

- `df.isna()` – boolean mask of missing values
- `df.dropna()` – drop rows with any missing values
- `df.fillna(value)` – fill missing values
- `df.interpolate()` – interpolate missing values

Example:
```python
df = pd.DataFrame({'A': [1, np.nan, 3], 'B': [4, 5, np.nan]})
df.fillna(0, inplace=True)
```

---

## 8. Important Methods for Data Analysis

### 8.1 Descriptive Statistics
```python
df.describe()                # summary statistics for numeric columns
df['Age'].mean()             # mean of a column
df['Age'].median()           # median
df['Age'].std()              # standard deviation
df['Age'].value_counts()     # frequency of unique values
```

### 8.2 Grouping and Aggregation
```python
df.groupby('City')['Age'].mean()     # average age per city
df.groupby('City').agg({'Age': 'mean', 'Salary': 'sum'})
```

### 8.3 Sorting
```python
df.sort_values('Age', ascending=False)   # sort by Age descending
df.sort_index()                          # sort by row index
```

### 8.4 Merging and Joining
```python
# Merge two DataFrames on a common column
df1 = pd.DataFrame({'key': ['A', 'B'], 'value': [1, 2]})
df2 = pd.DataFrame({'key': ['A', 'B'], 'value': [3, 4]})
merged = pd.merge(df1, df2, on='key')
```

### 8.5 Applying Functions
```python
df['Age'].apply(lambda x: x * 2)      # apply a function to a column
df.apply(np.sqrt, axis=1)             # apply function row‑wise (axis=1)
```

### 8.6 Pivot Tables
```python
pd.pivot_table(df, values='Sales', index='Region', columns='Year', aggfunc='sum')
```

---

## 9. Working with NumPy Together

Pandas is built on NumPy, so you can easily convert between them:

```python
# DataFrame to NumPy array
arr = df.values         # returns a NumPy array (2D)

# NumPy array to DataFrame
df_from_np = pd.DataFrame(arr, columns=['A', 'B', 'C'])
```

You can also use NumPy functions on Pandas objects (they work element‑wise):
```python
df['LogAge'] = np.log(df['Age'])
```

---

## 10. Comprehensive Examples to Clear All Confusion

Let's build a few practical examples that illustrate all the concepts.

### Example 1: Creating and Exploring a DataFrame

```python
import pandas as pd
import numpy as np

# Create a DataFrame from a dictionary
data = {
    'Product': ['A', 'B', 'C', 'D', 'E'],
    'Sales': [100, 200, 150, 300, 250],
    'Price': [10, 20, 15, 30, 25],
    'Region': ['North', 'South', 'East', 'West', 'North']
}
df = pd.DataFrame(data)

print("DataFrame:")
print(df)
print("\nShape:", df.shape)
print("\nColumns:", df.columns.tolist())
print("\nData types:\n", df.dtypes)
print("\nStatistical summary:\n", df.describe())
```

### Example 2: Accessing and Filtering

```python
# Select column 'Sales'
sales_series = df['Sales']
print("Sales series:\n", sales_series)

# Select rows where Sales > 200
high_sales = df[df['Sales'] > 200]
print("\nHigh sales:\n", high_sales)

# Select rows where Region is North and Sales > 100
north_high = df[(df['Region'] == 'North') & (df['Sales'] > 100)]
print("\nNorth high sales:\n", north_high)

# Access specific element
print("\nSales of product C:", df.at[2, 'Sales'])
print("Sales of product C (by position):", df.iat[2, 1])
```

### Example 3: Adding, Removing, Modifying Columns

```python
# Add a new column 'Revenue' = Sales * Price
df['Revenue'] = df['Sales'] * df['Price']
print("\nAfter adding Revenue:\n", df)

# Add a column with a constant
df['Tax'] = 0.05
print("\nAfter adding Tax:\n", df)

# Remove 'Tax' column
df.drop('Tax', axis=1, inplace=True)
print("\nAfter dropping Tax:\n", df)

# Modify 'Price' by adding 5
df['Price'] = df['Price'] + 5
print("\nAfter increasing Price by 5:\n", df)
```

### Example 4: Sorting and Grouping

```python
# Sort by Revenue descending
sorted_df = df.sort_values('Revenue', ascending=False)
print("\nSorted by Revenue:\n", sorted_df)

# Group by Region and calculate total sales and average price
grouped = df.groupby('Region').agg({
    'Sales': 'sum',
    'Price': 'mean',
    'Revenue': 'sum'
})
print("\nGrouped by Region:\n", grouped)
```

### Example 5: Handling Missing Data

```python
# Introduce some missing values
df.loc[2, 'Sales'] = np.nan   # product C sales missing
df.loc[4, 'Price'] = np.nan   # product E price missing
print("\nDataFrame with missing values:\n", df)

# Check missing values
print("\nMissing values per column:\n", df.isna().sum())

# Fill missing Sales with median
df['Sales'].fillna(df['Sales'].median(), inplace=True)

# Fill missing Price with mean
df['Price'].fillna(df['Price'].mean(), inplace=True)

# Recalculate Revenue after filling
df['Revenue'] = df['Sales'] * df['Price']
print("\nAfter filling missing values:\n", df)
```

### Example 6: Reading and Writing CSV

```python
# Write the DataFrame to a CSV file
df.to_csv('products.csv', index=False)

# Read it back
df_read = pd.read_csv('products.csv')
print("\nRead from CSV:\n", df_read.head())
```

### Example 7: Combining with NumPy

```python
# Create a NumPy array from the DataFrame (excluding the index)
np_arr = df[['Sales', 'Price', 'Revenue']].values
print("\nNumPy array:\n", np_arr)
print("Shape of NumPy array:", np_arr.shape)

# Apply a NumPy function to a column
df['LogSales'] = np.log(df['Sales'])
print("\nAfter log transform:\n", df)
```

---

## 11. Summary of Key Pandas Concepts

| Concept | Description |
|---------|-------------|
| **Series** | 1D labeled array; can be created from list, dict, or scalar. |
| **DataFrame** | 2D labeled table; columns are Series sharing an index. |
| **Indexing** | `[]` for columns, `loc` for label‑based, `iloc` for position‑based. |
| **Boolean indexing** | `df[condition]` filters rows. |
| **Adding columns** | `df['new'] = ...` |
| **Removing columns** | `df.drop('col', axis=1, inplace=True)` |
| **Handling missing values** | `isna()`, `dropna()`, `fillna()`. |
| **Grouping** | `groupby()` and aggregation functions. |
| **Merging** | `pd.merge()` to combine DataFrames. |
| **Pivot tables** | `pd.pivot_table()` for summary tables. |
| **Apply functions** | `df.apply()` for row/column‑wise operations. |
| **NumPy integration** | `df.values` gives a NumPy array; NumPy functions work element‑wise. |

---

## 12. Final Thoughts

Pandas is an indispensable tool for data analysis. Mastering Series and DataFrame will allow you to clean, transform, and analyze data efficiently. The examples above cover the most common operations, but Pandas offers many more features (time series, multi‑indexing, advanced merging, etc.). As you practice, you'll discover its full power.

**Further resources:**
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [10 Minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html)
- [Pandas Cookbook](https://pandas.pydata.org/docs/user_guide/cookbook.html)
