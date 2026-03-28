# Data Visualization with Seaborn: A Complete Reference

Seaborn is a Python visualization library built on top of Matplotlib. It provides a high‑level interface for creating statistically informed, aesthetically pleasing graphics with minimal code. This document covers Seaborn’s key features, its advantages over Matplotlib, installation, and a detailed exploration of its plotting functions, including parameters, usage examples, and customization techniques.

---

## 1. Why Seaborn?

Seaborn extends Matplotlib by offering:

- **Simplified syntax**: Complex plots like boxplots, violin plots, and pairplots can be created in a single line.
- **Statistical integration**: Built‑in support for aggregating data, estimating regression lines, and visualizing distributions.
- **Beautiful defaults**: Attractive color palettes and themes that are easy to customize.
- **Automatic handling of DataFrames**: Directly works with pandas DataFrames, using column names as input.
- **Multi‑plot grids**: `FacetGrid`, `pairplot`, and `jointplot` facilitate exploring multi‑dimensional data.

While Matplotlib gives full control over every plot element, Seaborn abstracts many details, making it faster to produce publication‑quality figures.

---

## 2. Installation

Seaborn can be installed via pip:

```bash
pip install seaborn
```

If using Anaconda:

```bash
conda install seaborn
```

Seaborn automatically installs its dependencies: matplotlib, numpy, pandas, and scipy.

---

## 3. Importing and Setting Up

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# For inline display in Jupyter
%matplotlib inline
```

Seaborn’s default theme and color palette are applied automatically. To reset to Matplotlib defaults: `sns.reset_defaults()`.

---

## 4. Loading Sample Data

Seaborn includes several built‑in datasets. One of the most used is the `tips` dataset, which contains restaurant tipping information.

```python
tips = sns.load_dataset('tips')
print(tips.head())
```

Columns: `total_bill`, `tip`, `sex`, `smoker`, `day`, `time`, `size`.

---

## 5. Basic Plotting Functions

### 5.1 Scatter Plot

A scatter plot visualizes the relationship between two numeric variables.

```python
sns.scatterplot(x='total_bill', y='tip', data=tips)
plt.title('Scatter Plot of Total Bill vs Tip')
plt.show()
```

**Key parameters for `sns.scatterplot()`**  
- `x`, `y` : column names or arrays  
- `data` : DataFrame  
- `hue` : column name; points are colored by this categorical variable  
- `style` : column name; marker style varies by this variable  
- `size` : column name; marker size varies by this variable  
- `palette` : color palette for different `hue` categories  
- `alpha` : transparency (0 to 1)

### 5.2 Line Plot

Line plots connect points in order of the x‑axis, useful for trends.

```python
sns.lineplot(x='size', y='total_bill', data=tips)
plt.title('Line Plot of Total Bill by Party Size')
plt.show()
```

**Parameters**  
- `x`, `y`, `data` as above  
- `hue`, `style`, `size` : as in scatterplot  
- `marker` : marker style for points  
- `err_style` : error bar style ('band' or 'bars')  
- `ci` : confidence interval size (default 95)

### 5.3 Categorical Plots

#### Bar Plot

`barplot` aggregates data and shows the mean (or other estimator) with error bars.

```python
sns.barplot(x='day', y='total_bill', data=tips)
plt.title('Bar Plot of Total Bill by Day')
plt.show()
```

**Parameters**  
- `estimator` : function to aggregate (default `np.mean`)  
- `ci` : confidence interval (default 95); set to `None` to suppress error bars  
- `capsize` : width of the error bar caps  
- `order` : order of the categories  
- `palette` : color palette  
- `hue` : group by a second categorical variable

#### Box Plot

Box plots show quartiles, median, and outliers.

```python
sns.boxplot(x='day', y='total_bill', data=tips)
```

**Parameters**  
- `orient` : 'v' (vertical) or 'h' (horizontal)  
- `width` : width of each box  
- `showfliers` : whether to show outlier points  
- `whis` : multiplier for the IQR to define outliers (default 1.5)

#### Violin Plot

Violin plots combine a box plot with a kernel density estimate.

```python
sns.violinplot(x='day', y='total_bill', data=tips)
```

**Parameters**  
- `inner` : representation of the data inside the violin ('box', 'quartile', 'point', 'stick')  
- `split` : if True, half‑violins are drawn for two hue levels  
- `scale` : scaling of the width ('area', 'count', 'width')

### 5.4 Distribution Plots

#### Histogram

Histograms show the distribution of a continuous variable.

```python
sns.histplot(tips['total_bill'], bins=10, kde=True)
```

**Parameters**  
- `bins` : number of bins or bin edges  
- `kde` : if True, overlay a kernel density estimate  
- `stat` : statistic to compute ('count', 'frequency', 'density', 'probability')  
- `cumulative` : if True, plot cumulative distribution  
- `color`, `edgecolor`, `alpha`

#### Kernel Density Estimate (KDE) Plot

KDE plots estimate the probability density function.

```python
sns.kdeplot(tips['total_bill'], fill=True)
```

**Parameters**  
- `fill` : if True, fill under the curve  
- `bw_method` : bandwidth estimation method  
- `cut` : how far beyond the data the curve is drawn  
- `clip` : limits for the data

### 5.5 Pair Plot

`pairplot` creates a matrix of scatter plots for all numeric columns, with histograms on the diagonal.

```python
sns.pairplot(tips)
```

**Parameters**  
- `hue` : column name for coloring points  
- `diag_kind` : type of plot on diagonal ('auto', 'hist', 'kde')  
- `corner` : if True, only show lower triangle  
- `plot_kws`, `diag_kws` : dictionaries of keyword arguments for scatter and diagonal plots

### 5.6 Heatmap

Heatmaps visualize matrix data, often correlation matrices.

```python
# Select numeric columns and compute correlation
corr = tips[['total_bill', 'tip', 'size']].corr()

sns.heatmap(corr, annot=True, cmap='coolwarm', center=0)
```

**Parameters**  
- `annot` : if True, write the data values in each cell  
- `fmt` : string format for annotations (e.g., '.2f')  
- `cmap` : colormap (e.g., 'coolwarm', 'viridis', 'RdBu')  
- `center` : value at which to center the colormap  
- `square` : if True, make cells square  
- `linewidths` : width of lines separating cells  
- `cbar` : if True, draw a colorbar  
- `cbar_kws` : keyword arguments for colorbar

---

## 6. Customization and Theming

Seaborn provides several functions to control the look of plots.

### 6.1 Figure Size

Use `plt.figure(figsize=(width, height))` before plotting:

```python
plt.figure(figsize=(10, 6))
sns.barplot(x='day', y='total_bill', data=tips)
```

### 6.2 Theme and Style

Seaborn has built‑in themes: `darkgrid`, `whitegrid`, `dark`, `white`, `ticks`.

```python
sns.set_style('whitegrid')
```

To set the context for different output sizes (paper, talk, poster, notebook):

```python
sns.set_context('talk')
```

### 6.3 Color Palettes

Seaborn offers many color palettes: `deep`, `muted`, `bright`, `pastel`, `dark`, `colorblind`.

```python
sns.set_palette('pastel')
```

Custom palettes can be created with `sns.color_palette()`.

### 6.4 Removing Spines

For a minimalistic look, remove top and right spines:

```python
sns.despine()
```

---

## 7. Advanced: Aggregating with `estimator`

In categorical plots, the `estimator` parameter controls how data is aggregated. For example, to show total sales (sum) instead of mean:

```python
sns.barplot(x='day', y='total_bill', data=tips, estimator=sum)
```

This is particularly useful when plotting aggregated data from a DataFrame.

---

## 8. Working with Real Data: Sales Dataset Example

Assume a CSV file `sales_data.csv` with columns: `Date`, `Category`, `Value`, `Product`, `Sales`, `Region`. Below are examples of using Seaborn to visualize this data.

### 8.1 Load and Inspect Data

```python
sales_df = pd.read_csv('sales_data.csv')
print(sales_df.head())
print(sales_df.info())
```

### 8.2 Total Sales by Product (Bar Plot with Estimator)

To show total sales per product, use `estimator=sum`:

```python
plt.figure(figsize=(10, 6))
sns.barplot(x='Product', y='Sales', data=sales_df, estimator=sum, ci=None, palette='viridis')
plt.title('Total Sales by Product')
plt.xlabel('Product')
plt.ylabel('Total Sales')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

- `ci=None` removes error bars (since each product appears multiple times, but we want the sum).
- `palette='viridis'` applies a color map.

### 8.3 Total Sales by Region

Similarly, group by region:

```python
plt.figure(figsize=(8, 6))
sns.barplot(x='Region', y='Sales', data=sales_df, estimator=sum, ci=None, palette='coolwarm')
plt.title('Total Sales by Region')
plt.xlabel('Region')
plt.ylabel('Total Sales')
plt.show()
```

### 8.4 Sales Distribution by Region (Box Plot)

```python
sns.boxplot(x='Region', y='Sales', data=sales_df, palette='Set2')
plt.title('Sales Distribution by Region')
plt.show()
```

### 8.5 Time Series Trend (Line Plot with Aggregation)

First, convert `Date` to datetime and aggregate daily sales:

```python
sales_df['Date'] = pd.to_datetime(sales_df['Date'])
daily_sales = sales_df.groupby('Date')['Sales'].sum().reset_index()

plt.figure(figsize=(12, 5))
sns.lineplot(x='Date', y='Sales', data=daily_sales, marker='o')
plt.title('Daily Sales Trend')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

### 8.6 Correlation Heatmap

Select numeric columns and compute correlation:

```python
numeric_cols = sales_df.select_dtypes(include=[np.number]).columns
corr = sales_df[numeric_cols].corr()

plt.figure(figsize=(8, 6))
sns.heatmap(corr, annot=True, cmap='coolwarm', center=0, fmt='.2f')
plt.title('Correlation Matrix')
plt.show()
```

---

## 9. Summary of Key Seaborn Functions

| Function          | Purpose                                 | Key Parameters                                                                 |
|-------------------|-----------------------------------------|--------------------------------------------------------------------------------|
| `scatterplot`     | Scatter plot for two numeric variables  | `x, y, hue, style, size, palette, alpha`                                      |
| `lineplot`        | Line plot for trends                    | `x, y, hue, style, marker, ci, err_style`                                      |
| `barplot`         | Aggregated bar plot with error bars     | `x, y, hue, estimator, ci, capsize, palette`                                   |
| `boxplot`         | Box and whisker plot                    | `x, y, hue, orient, width, showfliers, whis`                                   |
| `violinplot`      | Violin plot combining box and KDE       | `x, y, hue, inner, split, scale`                                               |
| `histplot`        | Histogram                               | `x, bins, kde, stat, cumulative, color`                                        |
| `kdeplot`         | Kernel density estimate                 | `x, fill, bw_method, cut, clip`                                                |
| `pairplot`        | Matrix of scatter plots                 | `data, hue, diag_kind, corner, plot_kws, diag_kws`                             |
| `heatmap`         | Heatmap of matrix data                  | `data, annot, fmt, cmap, center, square, linewidths`                           |
| `set_style`       | Set plot style                          | `'darkgrid', 'whitegrid', 'dark', 'white', 'ticks'`                            |
| `set_context`     | Scale plot elements                     | `'paper', 'notebook', 'talk', 'poster'`                                        |
| `set_palette`     | Set color palette                       | `'deep', 'muted', 'pastel', 'bright', 'dark', 'colorblind'` or custom list     |

---

## 10. Conclusion

Seaborn simplifies the process of creating complex, aesthetically pleasing visualizations. Its tight integration with pandas, statistical aggregation, and automatic handling of data structures allows for rapid exploration and communication of data insights. By mastering the functions and parameters outlined in this guide, one can produce a wide variety of plots suitable for both exploratory analysis and publication.