# NumPy – A Comprehensive Guide for Beginners and Advanced Users

NumPy is the cornerstone of scientific computing in Python. It provides efficient multi-dimensional arrays and a vast collection of mathematical functions. Whether you're just starting or looking to deepen your understanding, this guide covers everything from the basics to advanced concepts like memory layout, broadcasting, and vectorization.

---

## 1. What is NumPy and Why Use It?

**NumPy** (Numerical Python) is an open‑source library that adds support for large, multi‑dimensional arrays and matrices, along with a large set of high‑level mathematical functions to operate on these arrays. It is the foundation upon which many other scientific libraries (SciPy, Pandas, scikit‑learn) are built.

**Why NumPy over Python lists?**
- **Performance:** NumPy arrays are stored in contiguous blocks of memory (like C arrays), allowing for fast access and vectorized operations. Python lists store references to objects, leading to overhead.
- **Vectorization:** Operations are applied element‑wise without explicit Python loops, which are slow.
- **Convenience:** A rich set of functions for linear algebra, statistics, random number generation, and more.
- **Memory efficiency:** NumPy arrays use less memory because they store homogeneous data types.

---

## 2. Installation

Install NumPy using `pip`:

```bash
pip install numpy
```

If you use Anaconda, it comes pre‑installed. Otherwise, you can install via conda:

```bash
conda install numpy
```

---

## 3. Importing NumPy

By convention, NumPy is imported as `np`:

```python
import numpy as np
```

This gives you access to all NumPy functions through the `np` prefix.

---

## 4. Creating 1‑D Arrays

The basic way to create a NumPy array is from a Python list using `np.array()`:

```python
arr1 = np.array([1, 2, 3, 4, 5])
print(arr1)          # [1 2 3 4 5]
print(type(arr1))    # <class 'numpy.ndarray'>
print(arr1.shape)    # (5,)   — a tuple showing dimensions (5 elements, 1 dimension)
```

**Explanation:**  
- `np.array()` converts the list into a homogeneous array. The elements are stored in contiguous memory.
- The `shape` attribute returns a tuple indicating the size of each dimension. For a 1‑D array, it's `(n,)` where `n` is the length.

You can also specify the data type with `dtype`:

```python
arr_float = np.array([1, 2, 3], dtype=float)   # [1. 2. 3.]
```

---

## 5. Array Attributes

Understanding attributes helps you inspect and work with arrays effectively.

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])
print("Array:\n", arr)
print("Shape:", arr.shape)          # (2, 3)   → 2 rows, 3 columns
print("Number of dimensions:", arr.ndim)   # 2
print("Total number of elements:", arr.size)  # 6
print("Data type:", arr.dtype)      # int64 (may vary)
print("Item size (bytes):", arr.itemsize)   # 8 (for int64)
```

- **`shape`:** A tuple describing the size of each dimension. Useful for checking dimensions before operations.
- **`ndim`:** The number of axes (dimensions). A scalar is 0‑dimensional, a vector is 1‑D, a matrix is 2‑D, etc.
- **`size`:** Total number of elements, equal to the product of the shape.
- **`dtype`:** The data type of the elements. NumPy supports many types (int32, float64, complex128, etc.).
- **`itemsize`:** Size in bytes of each element. For int64 it's 8 bytes.

**Advanced note:** NumPy arrays are stored in **row‑major (C‑style) order** by default. That means the last dimension changes fastest. You can also create arrays in **column‑major (Fortran‑style) order** using `order='F'`. This matters for performance when iterating over large arrays.

---

## 6. Reshaping Arrays

`reshape()` changes the shape of an array without altering its data. It returns a **view** whenever possible (no copy) – a powerful feature that saves memory.

```python
arr1 = np.array([1, 2, 3, 4, 5])
arr2d = arr1.reshape(1, 5)   # 1 row, 5 columns
print(arr2d.shape)            # (1, 5)
print(arr2d)
# [[1 2 3 4 5]]
```

**Important:** The total number of elements must stay the same. Using `-1` in one dimension lets NumPy infer the size:

```python
arr1.reshape(5, -1)   # 5 rows, 1 column → shape (5,1)
```

The `reshape` method returns a new array object, but if possible it shares the same data buffer (a view). You can check with `arr1.base` or `arr2d.base`. Modifying the view modifies the original.

**Why reshape?** Often needed to feed data into machine learning models (e.g., a batch of images) or to match the required input shape of functions.

---

## 7. Creating 2‑D Arrays Directly

You can create a 2‑D array by passing a list of lists:

```python
arr2d = np.array([[1, 2, 3], [4, 5, 6]])
print(arr2d)
# [[1 2 3]
#  [4 5 6]]
```

Here each inner list becomes a row. The shape is `(2, 3)`.

---

## 8. `np.arange()` – In‑Depth

`np.arange()` generates evenly spaced values within a given interval. It is similar to Python’s `range()` but returns a NumPy array.

```python
np.arange(5)        # [0 1 2 3 4]
np.arange(2, 10, 2) # [2 4 6 8]
np.arange(0, 10, 2) # [0 2 4 6 8]
```

**Parameters:** `start` (default 0), `stop` (exclusive), `step` (default 1). All can be integers or floats.

**Reshaping `arange` to `(5,1)`:**

```python
arr = np.arange(0, 10, 2).reshape(5, 1)
print(arr)
# [[0]
#  [2]
#  [4]
#  [6]
#  [8]]
```

**Why useful?** `arange` is perfect for creating sequences for indexing, simulation, or generating test data.

**Note:** For large arrays, consider `np.linspace()` when you need a specific number of points between two endpoints.

---

## 9. `np.ones()` and `np.eye()`

- `np.ones(shape)` creates an array filled with 1s.
- `np.eye(N)` creates an N×N identity matrix (1s on diagonal, 0s elsewhere).

```python
print(np.ones((3, 4)))   # 3 rows, 4 columns of 1.0
# [[1. 1. 1. 1.]
#  [1. 1. 1. 1.]
#  [1. 1. 1. 1.]]

print(np.eye(3))
# [[1. 0. 0.]
#  [0. 1. 0.]
#  [0. 0. 1.]]
```

Both functions also accept a `dtype` argument to specify the element type (e.g., `int`, `float`). By default, they create float arrays.

**Similar functions:** `np.zeros()`, `np.full()`, `np.empty()` (for uninitialized arrays, faster but with arbitrary values).

---

## 10. Vectorized Operations

Vectorization means applying an operation to every element of an array without writing an explicit loop. It leverages highly optimized C code under the hood.

```python
arr1 = np.array([1, 2, 3, 4, 5])
arr2 = np.array([10, 20, 30, 40, 50])

print("Addition:", arr1 + arr2)          # [11 22 33 44 55]
print("Subtraction:", arr1 - arr2)       # [-9 -18 -27 -36 -45]
print("Multiplication:", arr1 * arr2)    # [10 40 90 160 250]
print("Division:", arr1 / arr2)          # [0.1 0.1 0.1 0.1 0.1]
```

**How does it work?** The `+` operator is overloaded for NumPy arrays to perform element‑wise addition. This is called **broadcasting** when arrays have compatible shapes. Here both arrays have the same shape, so it's straightforward.

**Performance comparison:** A Python loop over a million elements might take tens of milliseconds; vectorized operations take microseconds. This is crucial for large datasets.

---

## 11. Universal Functions (ufuncs)

Universal functions are vectorized functions that operate element‑wise on arrays. They are implemented in C for speed and can be used with any array shape.

```python
arr = np.array([2, 3, 4, 5, 6])

print("Square root:", np.sqrt(arr))      # [1.414 1.732 2. 2.236 2.449]
print("Exponential:", np.exp(arr))       # [7.389 20.085 54.598 148.413 403.429]
print("Sine:", np.sin(arr))              # [0.909 0.141 -0.756 -0.958 -0.279]
print("Natural log:", np.log(arr))       # [0.693 1.099 1.386 1.609 1.792]
```

**List of common ufuncs:**
- Trigonometric: `sin`, `cos`, `tan`, `arcsin`, ...
- Logarithmic: `log`, `log10`, `log2`
- Exponential: `exp`, `expm1`
- Arithmetic: `add`, `subtract`, `multiply`, `divide`, `power`
- Miscellaneous: `abs`, `sqrt`, `square`, `sign`, `ceil`, `floor`

Ufuncs can also accept an `out` parameter to store results in an existing array, saving memory.

---

## 12. Array Slicing and Indexing

### Basic Indexing
Access elements using indices. For 2‑D arrays, use `arr[row, col]` (preferred) or `arr[row][col]`.

```python
arr = np.array([[1, 2, 3, 4],
                [5, 6, 7, 8],
                [9, 10, 11, 12]])

print(arr[0, 0])    # 1
print(arr[0][0])    # 1 (less efficient because it creates a temporary view)
```

### Slicing
Slicing uses the syntax `start:stop:step` for each dimension.

```python
# Rows from 1 onward, columns 1 to 3 (exclusive of 3)
print(arr[1:, 1:3])
# [[ 6  7]
#  [10 11]]

# Rows 0 and 1, columns 2 onward
print(arr[0:2, 2:])
# [[3 4]
#  [7 8]]

# All rows, columns 2 onward
print(arr[:, 2:])
# [[ 3  4]
#  [ 7  8]
#  [11 12]]
```

**Modifying slices** changes the original array because slices are views (no copy).

```python
arr[0, 0] = 100
print(arr)
# [[100   2   3   4]
#  [  5   6   7   8]
#  [  9  10  11  12]]

arr[1:] = 100        # assign 100 to all elements in rows 1 onward
print(arr)
# [[100   2   3   4]
#  [100 100 100 100]
#  [100 100 100 100]]
```

**Advanced indexing:**
- **Boolean indexing:** Use a boolean mask to select elements.
- **Fancy indexing:** Use arrays of indices.

Example of boolean indexing:

```python
data = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
mask = (data >= 5) & (data <= 8)
print(data[mask])   # [5 6 7 8]
```

---

## 13. Statistical Functions

NumPy provides fast statistical functions that can operate along specific axes.

```python
data = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

print("Mean:", np.mean(data))          # 5.5
print("Median:", np.median(data))      # 5.5
print("Standard deviation:", np.std(data))   # 2.872
print("Variance:", np.var(data))       # 8.25
```

**Axis parameter:** For multi‑dimensional arrays, you can compute statistics along rows or columns.

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])
print(np.mean(arr, axis=0))   # mean of columns → [2.5 3.5 4.5]
print(np.mean(arr, axis=1))   # mean of rows    → [2. 5.]
```

**Normalization** (z‑score) example:

```python
normalized = (data - np.mean(data)) / np.std(data)
print("Normalized data:", normalized)
```

---

## 14. Logical Operations on Arrays

NumPy arrays support element‑wise comparison operators, returning boolean arrays.

```python
data = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

print(data > 5)          # [False False False False False  True  True  True  True  True]
```

Combine conditions with `&` (and) and `|` (or). Note that parentheses are necessary because `&` has higher precedence than comparison operators.

```python
mask = (data >= 5) & (data <= 8)
print(data[mask])        # [5 6 7 8]
```

You can also assign new values based on conditions:

```python
data[data > 5] = 0
print(data)              # [1 2 3 4 5 0 0 0 0 0]
```

**Why useful?** Filtering, data cleaning, and masking are common tasks in data analysis.

---

## 15. Advanced Concepts (for Deeper Understanding)

### Broadcasting
Broadcasting allows NumPy to work with arrays of different shapes during arithmetic operations. The smaller array is "broadcast" across the larger one without copying data unnecessarily.

**Rule:** Two arrays are compatible for broadcasting if, for each dimension, either the dimensions are equal or one of them is 1.

Example:

```python
a = np.array([1, 2, 3])      # shape (3,)
b = np.array([[10], [20]])   # shape (2,1)
result = a + b                # shape (2,3) by broadcasting
print(result)
# [[11 12 13]
#  [21 22 23]]
```

### View vs Copy
- **View:** A new array object that shares the same data buffer as the original. Changing the view changes the original. Examples: slicing, `reshape` (if possible).
- **Copy:** A new array with its own copy of data. Created by `.copy()` or when reshaping would break contiguity.

Use `.base` to check if an array is a view. Memory efficiency is crucial for large datasets.

### Memory Layout
NumPy arrays are stored in contiguous memory blocks. The default order is **C‑order** (row‑major). You can create **F‑order** (column‑major) arrays with `order='F'`. The order affects iteration speed and memory access patterns. For large multi‑dimensional arrays, choosing the right order can improve performance.

### Adding Dimensions with `np.newaxis`
You can increase the dimension of an array using `np.newaxis` (or `None`):

```python
a = np.array([1, 2, 3])
print(a[:, np.newaxis])   # shape (3,1)
print(a[np.newaxis, :])   # shape (1,3)
```

This is often used to prepare arrays for broadcasting.

---

## 16. Complete Example (Including the Provided Jupyter Snippets)

Below is an annotated version of the notebook code you shared, with comments explaining each step.

```python
import numpy as np

# ====================
# 1D array creation
# ====================
arr1 = np.array([1, 2, 3, 4, 5])
print(arr1)                     # [1 2 3 4 5]
print(type(arr1))               # <class 'numpy.ndarray'>
print(arr1.shape)               # (5,)

# Reshape to 1x5
arr2 = np.array([1, 2, 3, 4, 5])
arr2.reshape(1, 5)              # Returns a 2D array with 1 row, 5 columns

# 2D array directly
arr2 = np.array([[1, 2, 3, 4, 5], [2, 3, 4, 5, 6]])
print(arr2)                     # 2x5 array
print(arr2.shape)               # (2, 5)

# arange and reshape to (5,1)
np.arange(0, 10, 2).reshape(5, 1)

# ones and identity
np.ones((3, 4))
np.eye(3)

# ====================
# Array attributes
# ====================
arr = np.array([[1, 2, 3], [4, 5, 6]])
print("Shape:", arr.shape)              # (2, 3)
print("Number of dimensions:", arr.ndim) # 2
print("Size (number of elements):", arr.size) # 6
print("Data type:", arr.dtype)          # int64
print("Item size (in bytes):", arr.itemsize) # 8

# ====================
# Vectorized operations
# ====================
arr1 = np.array([1, 2, 3, 4, 5])
arr2 = np.array([10, 20, 30, 40, 50])

print("Addition:", arr1 + arr2)
print("Subtraction:", arr1 - arr2)
print("Multiplication:", arr1 * arr2)
print("Division:", arr1 / arr2)

# ====================
# Universal functions
# ====================
arr = np.array([2, 3, 4, 5, 6])
print(np.sqrt(arr))
print(np.exp(arr))
print(np.sin(arr))
print(np.log(arr))

# ====================
# Slicing and indexing
# ====================
arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
print("Array:\n", arr)
print(arr[1:, 1:3])      # rows 1 onward, cols 1 and 2
print(arr[0][0])         # first row, first column (less efficient)
print(arr[0:2, 2:])      # rows 0-1, cols 2 onward
print(arr[1:, 2:])       # rows 1 onward, cols 2 onward

# Modify elements
arr[0, 0] = 100
print(arr)
arr[1:] = 100
print(arr)

# ====================
# Statistical concepts
# ====================
data = np.array([1, 2, 3, 4, 5])
mean = np.mean(data)
std_dev = np.std(data)
normalized_data = (data - mean) / std_dev
print("Normalized data:", normalized_data)

data = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
print("Mean:", np.mean(data))
print("Median:", np.median(data))
print("Standard Deviation:", np.std(data))
print("Variance:", np.var(data))

# ====================
# Logical operations
# ====================
data = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
print(data[(data >= 5) & (data <= 8)])   # [5 6 7 8]
```

---

## Conclusion

NumPy is a powerful tool that forms the backbone of scientific computing in Python. Mastering its array operations, broadcasting, and universal functions will enable you to write efficient, readable code for data analysis, machine learning, and numerical simulations. The concepts covered here—from basic array creation to advanced memory management—provide a solid foundation for both beginners and those looking to deepen their expertise.

**Further resources:**
- [NumPy Documentation](https://numpy.org/doc/stable/)
- [NumPy User Guide](https://numpy.org/doc/stable/user/index.html)
- [SciPy Lecture Notes](https://scipy-lectures.org/)
