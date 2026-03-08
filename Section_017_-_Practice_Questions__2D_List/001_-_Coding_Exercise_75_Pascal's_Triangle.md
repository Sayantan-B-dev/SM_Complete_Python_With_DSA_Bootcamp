## Pascal's Triangle: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Pascal's Triangle is a triangular array where each number is the sum of the two numbers directly above it. The first and last element of each row is 1. Given `numRows`, generate the first `numRows` of Pascal's Triangle.

**Approach:**  
- Initialize an empty list `triangle`.  
- For each row from 0 to `numRows-1`:  
  - Create a row list of length `row+1` filled with zeros.  
  - Set first and last element to 1.  
  - For columns 1 to `row-1`, compute value as `triangle[row-1][col-1] + triangle[row-1][col]`.  
  - Append the row to `triangle`.  
- Return `triangle`.

**Algorithm:**  
```
triangle = []
for row in range(numRows):
    current = [0] * (row + 1)
    current[0] = 1
    current[row] = 1
    for col in range(1, row):
        current[col] = triangle[row-1][col-1] + triangle[row-1][col]
    triangle.append(current)
return triangle
```

**Pseudocode:**  
```
function generate(numRows):
    triangle = []
    for row = 0 to numRows-1:
        current = array of size row+1 filled with 0
        current[0] = 1
        current[row] = 1
        for col = 1 to row-1:
            current[col] = triangle[row-1][col-1] + triangle[row-1][col]
        triangle.append(current)
    return triangle
```

---

## Basic Iterative Construction

```python
def generate_basic(numRows):
    triangle = []
    for row in range(numRows):
        # Create a row with row+1 elements initialized to 0
        current_row = [0] * (row + 1)
        # First and last elements are always 1
        current_row[0] = 1
        current_row[row] = 1
        # Fill middle elements using previous row
        for col in range(1, row):
            current_row[col] = triangle[row-1][col-1] + triangle[row-1][col]
        triangle.append(current_row)
    return triangle

# Example call
result = generate_basic(5)
print("Pascal's Triangle (5 rows):")
for row in result:
    print(row)
# Output:
# [1]
# [1, 1]
# [1, 2, 1]
# [1, 3, 3, 1]
# [1, 4, 6, 4, 1]
```

---

## Using math.comb (Binomial Coefficients)

```python
import math

def generate_comb(numRows):
    triangle = []
    for row in range(numRows):
        # Each element at position col is C(row, col)
        current_row = [math.comb(row, col) for col in range(row + 1)]
        triangle.append(current_row)
    return triangle

# Example call
result = generate_comb(5)
print("Using math.comb:")
for row in result:
    print(row)
# Output same as above
```

---

## Recursive Row Generation

```python
def generate_recursive(numRows):
    if numRows == 0:
        return []
    if numRows == 1:
        return [[1]]
    
    # Get previous rows recursively
    prev_triangle = generate_recursive(numRows - 1)
    prev_row = prev_triangle[-1]
    
    # Build new row from previous
    new_row = [1]
    for i in range(len(prev_row) - 1):
        new_row.append(prev_row[i] + prev_row[i + 1])
    new_row.append(1)
    
    prev_triangle.append(new_row)
    return prev_triangle

# Example call
result = generate_recursive(5)
print("Recursive generation:")
for row in result:
    print(row)
```

---

## Using zip to Build Next Row

```python
def generate_zip(numRows):
    triangle = []
    if numRows >= 1:
        triangle.append([1])
    for _ in range(1, numRows):
        # Previous row
        prev = triangle[-1]
        # Create new row by zipping shifted copies
        # [0] + prev and prev + [0] give pairs to sum
        new_row = [a + b for a, b in zip([0] + prev, prev + [0])]
        triangle.append(new_row)
    return triangle

# Example call
result = generate_zip(5)
print("Using zip:")
for row in result:
    print(row)
```

---

## List Comprehension with Previous Row

```python
def generate_listcomp(numRows):
    triangle = []
    for row in range(numRows):
        if row == 0:
            triangle.append([1])
        else:
            prev = triangle[-1]
            # List comprehension: first 1, then sums, then last 1
            new_row = [1] + [prev[i] + prev[i+1] for i in range(row-1)] + [1]
            triangle.append(new_row)
    return triangle

# Example call
result = generate_listcomp(5)
print("List comprehension:")
for row in result:
    print(row)
```

---

## Using functools.reduce

```python
from functools import reduce

def generate_reduce(numRows):
    # reduce accumulates rows
    def build_rows(rows, _):
        if not rows:
            return [[1]]
        last = rows[-1]
        # Compute next row
        next_row = [a + b for a, b in zip([0] + last, last + [0])]
        return rows + [next_row]
    
    # Use reduce on range to build rows sequentially
    triangle = reduce(build_rows, range(numRows - 1), [])
    return triangle if triangle else []

# Example call
result = generate_reduce(5)
print("Using reduce:")
for row in result:
    print(row)
```

---

## Generator Function (Lazy Evaluation)

```python
def pascal_generator():
    """Generator that yields rows of Pascal's triangle indefinitely."""
    row = [1]
    while True:
        yield row
        # Compute next row
        row = [a + b for a, b in zip([0] + row, row + [0])]

def generate_generator(numRows):
    gen = pascal_generator()
    triangle = [next(gen) for _ in range(numRows)]
    return triangle

# Example call
result = generate_generator(5)
print("Generator:")
for row in result:
    print(row)
```

---

## Memoized Recursive with Cache

```python
from functools import lru_cache

def generate_memoized(numRows):
    @lru_cache(maxsize=None)
    def get_row(r):
        if r == 0:
            return (1,)
        prev = get_row(r - 1)
        # Build tuple row using sums
        row = [1] + [prev[i] + prev[i+1] for i in range(r-1)] + [1]
        return tuple(row)
    
    triangle = [list(get_row(i)) for i in range(numRows)]
    return triangle

# Example call
result = generate_memoized(5)
print("Memoized recursive:")
for row in result:
    print(row)
```

---

## Using NumPy for Vectorized Computation

```python
import numpy as np

def generate_numpy(numRows):
    triangle = []
    for r in range(numRows):
        if r == 0:
            triangle.append([1])
        else:
            # Create arrays for shifted copies
            prev = np.array(triangle[-1])
            left = np.concatenate(([0], prev))
            right = np.concatenate((prev, [0]))
            new_row = (left + right).tolist()
            triangle.append(new_row)
    return triangle

# Example call
result = generate_numpy(5)
print("NumPy vectorized:")
for row in result:
    print(row)
```

---

## One-Liner with itertools.accumulate

```python
from itertools import accumulate

def generate_oneliner(numRows):
    # Using accumulate to generate successive rows
    # Each step: row = [1] + [x+y for x,y in zip(row, row[1:])] + [1]
    rows = [[1]]
    for _ in range(1, numRows):
        rows.append([1] + [x + y for x, y in zip(rows[-1], rows[-1][1:])] + [1])
    return rows

# Example call (same as basic, just compact)
result = generate_oneliner(5)
print("One-liner style:")
for row in result:
    print(row)
```