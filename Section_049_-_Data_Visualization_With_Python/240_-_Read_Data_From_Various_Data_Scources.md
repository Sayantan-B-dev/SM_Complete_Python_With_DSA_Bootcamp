# Reading Data from Different Sources in Pandas

Pandas provides flexible functions to read data from various file formats and sources. This document covers the most common ones: JSON, CSV from URLs, HTML tables, Excel files, and Pickle files. For each, the required libraries, reading methods, and saving options are described.

---

## 1. JSON Data

JSON (JavaScript Object Notation) is a lightweight data interchange format. Pandas can read JSON strings or files and convert them into DataFrames.

### 1.1 Reading JSON from a String

When a JSON string is available (e.g., from an API response), it can be wrapped in a `StringIO` object to mimic a file.

```python
import pandas as pd
from io import StringIO

data = '{"employee_name": "James", "email": "james@gmail.com", "job_profile": [{"title1":"Team Lead", "title2":"Sr. Developer"}]}'
df = pd.read_json(StringIO(data))
```

The `read_json()` function parses the JSON and creates a DataFrame. For nested structures like the `job_profile` list, the result will contain a column with lists.

### 1.2 Writing DataFrame to JSON

A DataFrame can be saved back to JSON using `df.to_json()`. The `orient` parameter controls the output structure.

```python
# Default orientation (records)
df.to_json()

# Orientation by index
df.to_json(orient='index')

# Orientation as list of records
df.to_json(orient='records')
```

Common `orient` values:
- `'records'`: list of objects (each row as a dict)
- `'index'`: dict with index keys, values are row dicts
- `'columns'`: dict with column keys, values are column arrays
- `'table'`: JSON Table Schema format

---

## 2. CSV from a URL

Pandas can read CSV files directly from a URL using `pd.read_csv()`. This is useful for accessing publicly available datasets.

```python
# Example: Wine dataset from UCI Machine Learning Repository
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/wine/wine.data"
df = pd.read_csv(url, header=None)
df.head()
```

The `header=None` argument indicates that the CSV file has no header row.

### 2.1 Saving DataFrame to CSV

A DataFrame can be written to a CSV file using `df.to_csv()`.

```python
df.to_csv("wine.csv", index=False)
```

The `index=False` parameter prevents writing row indices.

---

## 3. HTML Tables from Web Pages

Pandas can extract tables from HTML pages using `pd.read_html()`. This function returns a list of DataFrames, one for each table found on the page. To use it, additional libraries must be installed: `lxml`, `html5lib`, and `beautifulsoup4`. These libraries parse HTML and extract table structures.

### 3.1 Installing Required Libraries

```bash
pip install lxml html5lib beautifulsoup4
```

### 3.2 Reading All Tables from a URL

```python
url = "https://www.fdic.gov/resources/resolutions/bank-failures/failed-bank-list/"
tables = pd.read_html(url)
# The first table (index 0) is typically the one of interest
df_failed_banks = tables[0]
```

### 3.3 Filtering Tables with `match`

The `match` parameter can be used to select tables that contain a specific string. The `header` parameter specifies which row to use as column headers.

```python
url = "https://en.wikipedia.org/wiki/Mobile_country_code"
df_mcc = pd.read_html(url, match="Country", header=0)[0]
```

This returns the first table that contains the word "Country" and uses the first row as column headers.

---

## 4. Excel Files (XLSX)

Pandas can read Excel files using `pd.read_excel()`. The recommended engine for modern Excel files (`.xlsx`) is `openpyxl`.

### 4.1 Installing the Required Engine

```bash
pip install openpyxl
```

### 4.2 Reading an Excel File

```python
df_excel = pd.read_excel('data.xlsx')
```

Additional parameters:
- `sheet_name`: specify sheet name or index (default 0)
- `header`: row to use as column names
- `usecols`: columns to read

### 4.3 Writing to Excel

A DataFrame can be saved to Excel using `df.to_excel()`, which also requires `openpyxl`.

```python
df_excel.to_excel('output.xlsx', index=False)
```

---

## 5. Pickle Files

Pickle is a Python‑specific binary serialization format. It preserves the data structure exactly, including data types and indexes, making it very efficient for saving and loading DataFrames.

### 5.1 Saving a DataFrame to Pickle

```python
df_excel.to_pickle('df_excel.pkl')
```

### 5.2 Reading a Pickle File

```python
df_loaded = pd.read_pickle('df_excel.pkl')
```

**Advantages of Pickle:**
- Very fast I/O
- Preserves all metadata (index, column names, data types)
- Useful for intermediate storage during analysis

**Caveats:**
- Not human‑readable
- Pickle files may not be compatible across different Python versions
- Security: only unpickle trusted files

---

## 6. Summary Table of Reading Functions

| Source          | Function                 | Required Libraries                  |
|-----------------|--------------------------|-------------------------------------|
| JSON (string)   | `pd.read_json()`         | None (built‑in)                     |
| JSON (file)     | `pd.read_json()`         | None                                |
| CSV (URL)       | `pd.read_csv()`          | None                                |
| HTML tables     | `pd.read_html()`         | `lxml`, `html5lib`, `beautifulsoup4`|
| Excel (XLSX)    | `pd.read_excel()`        | `openpyxl` (or `xlrd` for old XLS)  |
| Pickle          | `pd.read_pickle()`       | None                                |

---

## 7. Complete Example

The following code demonstrates reading data from multiple sources and saving them in various formats.

```python
import pandas as pd
from io import StringIO

# 1. JSON from string
json_data = '{"employee_name": "James", "email": "james@gmail.com", "job_profile": [{"title1":"Team Lead", "title2":"Sr. Developer"}]}'
df_json = pd.read_json(StringIO(json_data))
print("JSON DataFrame:")
print(df_json)

# Save as JSON with different orientations
print("\nJSON orient='records':")
print(df_json.to_json(orient='records'))

# 2. CSV from URL (wine dataset)
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/wine/wine.data"
df_wine = pd.read_csv(url, header=None)
print("\nWine dataset shape:", df_wine.shape)
df_wine.to_csv("wine.csv", index=False)

# 3. HTML tables (requires lxml, html5lib, beautifulsoup4)
# Uncomment after installing packages
# url = "https://en.wikipedia.org/wiki/Mobile_country_code"
# df_mcc = pd.read_html(url, match="Country", header=0)[0]
# print("\nMobile Country Code table shape:", df_mcc.shape)

# 4. Excel file (requires openpyxl)
# df_excel = pd.read_excel('data.xlsx')
# df_excel.to_pickle('df_excel.pkl')
# df_loaded = pd.read_pickle('df_excel.pkl')
```

---

## 8. Notes on Libraries

- **lxml**: A fast XML/HTML parser. It is the recommended backend for `pd.read_html()`.
- **html5lib**: A pure‑Python HTML parser that handles malformed HTML. It can be used as an alternative to lxml.
- **beautifulsoup4**: A library for parsing HTML and XML; it works in conjunction with the other two.
- **openpyxl**: The standard engine for reading/writing Excel `.xlsx` files in pandas.
- **xlrd**: An older library for reading old `.xls` Excel files. It is not required for `.xlsx`.

---

This guide provides a foundation for importing data from diverse sources into pandas, enabling seamless integration of data from files, APIs, and web pages into analysis workflows.