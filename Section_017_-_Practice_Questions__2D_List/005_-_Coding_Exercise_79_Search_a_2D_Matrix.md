## Binary Search on Flattened Matrix

**Explanation:**  
The matrix is sorted row-wise and the first element of each row is greater than the last element of the previous row. This means the matrix can be treated as a single sorted array by flattening it row by row. Binary search can be performed on this virtual 1D array using index mapping.

**Approach:**  
- Treat the matrix as a 1D array of size `rows * cols`.  
- Use binary search on indices from 0 to `rows*cols - 1`.  
- Map the mid index back to 2D coordinates: `row = mid // cols`, `col = mid % cols`.  
- Compare the element with target and adjust bounds accordingly.

**Algorithm:**  
1. Set `rows = len(matrix)`, `cols = len(matrix[0])`.  
2. Set `left = 0`, `right = rows * cols - 1`.  
3. While `left <= right`:  
   - Compute `mid = (left + right) // 2`.  
   - Get `value = matrix[mid // cols][mid % cols]`.  
   - If `value == target`, return `True`.  
   - If `value < target`, set `left = mid + 1`.  
   - Else set `right = mid - 1`.  
4. Return `False`.

**Pseudocode:**  
```
function searchMatrix(matrix, target):
    rows = length(matrix)
    cols = length(matrix[0])
    left = 0
    right = rows * cols - 1
    while left <= right:
        mid = floor((left + right) / 2)
        value = matrix[mid // cols][mid % cols]
        if value == target:
            return True
        elif value < target:
            left = mid + 1
        else:
            right = mid - 1
    return False
```

```python
def search_matrix_original(matrix, target):
    rows = len(matrix)
    cols = len(matrix[0])
    left, right = 0, rows * cols - 1

    while left <= right:
        mid = (left + right) // 2                     # middle index in flattened array
        # Map to 2D coordinates
        value = matrix[mid // cols][mid % cols]        # row = mid // cols, col = mid % cols
        if value == target:
            return True                                 # found
        elif value < target:
            left = mid + 1                               # search right half
        else:
            right = mid - 1                              # search left half
    return False                                         # not found

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_original(matrix, target))   # Output: True
```

---

## Row-wise Binary Search with `bisect`

**Explanation:**  
Since each row is sorted, we can first locate the row where the target might exist by comparing with the last elements of rows. Then use Python's `bisect` module to perform binary search on that row.

**Approach:**  
- Iterate over rows (or use binary search on row boundaries) to find the candidate row.  
- In this variant, we simply loop through rows and use `bisect_left` on each row to check if the target is present. This is efficient for small matrices.

**Algorithm:**  
1. For each row in matrix:  
   - Use `bisect_left(row, target)` to get insertion point.  
   - If insertion point < len(row) and `row[insertion_point] == target`, return `True`.  
2. Return `False`.

**Pseudocode:**  
```
import bisect
function searchMatrix(matrix, target):
    for row in matrix:
        idx = bisect_left(row, target)
        if idx < len(row) and row[idx] == target:
            return True
    return False
```

```python
import bisect

def search_matrix_bisect(matrix, target):
    for row in matrix:
        # bisect_left returns the leftmost index where target could be inserted
        idx = bisect.bisect_left(row, target)
        # Check if target is actually present at that index
        if idx < len(row) and row[idx] == target:
            return True                         # found
    return False                                 # not found

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_bisect(matrix, target))   # Output: True
```

---

## Two-Step Binary Search (Row then Column)

**Explanation:**  
Because the matrix has the property that the first element of each row is greater than the last of the previous row, we can perform binary search on the first column to find the candidate row, then binary search within that row.

**Approach:**  
- Binary search on the first column to find the largest row where `matrix[row][0] <= target`.  
- If no such row exists, target is smaller than all, return `False`.  
- Then binary search within that row for the target.

**Algorithm:**  
1. Set `top = 0`, `bottom = rows - 1`.  
2. While `top <= bottom`:  
   - `mid_row = (top + bottom) // 2`.  
   - If `matrix[mid_row][0] == target`, return `True`.  
   - If `matrix[mid_row][0] < target`, set `top = mid_row + 1`.  
   - Else set `bottom = mid_row - 1`.  
3. After loop, `bottom` is the last row where first element <= target. If `bottom < 0`, return `False`.  
4. Perform binary search on row `bottom` using `bisect` or manual.

**Pseudocode:**  
```
function searchMatrix(matrix, target):
    rows = len(matrix)
    cols = len(matrix[0])
    top, bottom = 0, rows - 1
    while top <= bottom:
        mid = (top + bottom) // 2
        if matrix[mid][0] == target:
            return True
        elif matrix[mid][0] < target:
            top = mid + 1
        else:
            bottom = mid - 1
    if bottom < 0:
        return False
    # binary search on row bottom
    left, right = 0, cols - 1
    while left <= right:
        mid = (left + right) // 2
        if matrix[bottom][mid] == target:
            return True
        elif matrix[bottom][mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return False
```

```python
def search_matrix_two_step(matrix, target):
    rows = len(matrix)
    cols = len(matrix[0])
    # Step 1: find candidate row
    top, bottom = 0, rows - 1
    while top <= bottom:
        mid_row = (top + bottom) // 2
        if matrix[mid_row][0] == target:
            return True                             # found
        elif matrix[mid_row][0] < target:
            top = mid_row + 1                         # search in lower rows
        else:
            bottom = mid_row - 1                      # search in upper rows
    # After loop, bottom is the last row with first element <= target
    if bottom < 0:
        return False                                 # target smaller than all
    # Step 2: binary search in that row
    left, right = 0, cols - 1
    while left <= right:
        mid_col = (left + right) // 2
        val = matrix[bottom][mid_col]
        if val == target:
            return True
        elif val < target:
            left = mid_col + 1
        else:
            right = mid_col - 1
    return False

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_two_step(matrix, target))   # Output: True
```

---

## Linear Scan (Simple but Inefficient)

**Explanation:**  
For very small matrices or when simplicity is key, a linear scan through all elements can be used. It is O(n*m) but straightforward.

**Approach:**  
- Iterate through each row and each element.  
- If element equals target, return `True`.  
- If element exceeds target and rows are sorted, we could break early, but here we just scan all.

**Algorithm:**  
1. For each row in matrix:  
   For each element in row:  
     If element == target, return `True`.  
2. Return `False`.

**Pseudocode:**  
```
function searchMatrix(matrix, target):
    for row in matrix:
        for val in row:
            if val == target:
                return True
    return False
```

```python
def search_matrix_linear(matrix, target):
    for row in matrix:
        for val in row:
            if val == target:
                return True                     # found
    return False                                 # not found

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_linear(matrix, target))   # Output: True
```

---

## Flatten with `itertools.chain` and Binary Search

**Explanation:**  
Use `itertools.chain` to flatten the matrix into a single iterable, then convert to a list and perform binary search using `bisect`. This is similar to the first method but uses built-in flattening.

**Approach:**  
- Flatten the matrix with `chain.from_iterable`.  
- Convert to a list (or keep as list if already).  
- Use `bisect_left` to find insertion point and check if the element exists.

**Algorithm:**  
1. `flat = list(chain.from_iterable(matrix))`.  
2. `idx = bisect_left(flat, target)`.  
3. Return `idx < len(flat) and flat[idx] == target`.

**Pseudocode:**  
```
from itertools import chain
import bisect
function searchMatrix(matrix, target):
    flat = list(chain.from_iterable(matrix))
    idx = bisect_left(flat, target)
    return idx < len(flat) and flat[idx] == target
```

```python
from itertools import chain
import bisect

def search_matrix_chain(matrix, target):
    # Flatten the matrix into a single list
    flat = list(chain.from_iterable(matrix))
    # Use bisect to find insertion point
    idx = bisect.bisect_left(flat, target)
    # Check if target is present at that index
    return idx < len(flat) and flat[idx] == target

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_chain(matrix, target))   # Output: True
```

---

## Recursive Binary Search on Flattened Matrix

**Explanation:**  
A recursive version of the binary search on the virtual flattened array. This demonstrates recursion and is elegant.

**Approach:**  
- Define a recursive function that takes left and right indices.  
- Base case: if left > right, return `False`.  
- Compute mid, map to value, and compare.  
- Recurse on appropriate half.

**Algorithm:**  
1. `rows = len(matrix)`, `cols = len(matrix[0])`.  
2. Define `helper(left, right)`.  
   - If `left > right`, return `False`.  
   - `mid = (left+right)//2`, `value = matrix[mid//cols][mid%cols]`.  
   - If `value == target`, return `True`.  
   - If `value < target`, recurse with `(mid+1, right)`.  
   - Else recurse with `(left, mid-1)`.  
3. Call `helper(0, rows*cols-1)`.

**Pseudocode:**  
```
function searchMatrix(matrix, target):
    rows = len(matrix)
    cols = len(matrix[0])
    function binary(l, r):
        if l > r: return False
        mid = floor((l+r)/2)
        val = matrix[mid//cols][mid%cols]
        if val == target: return True
        if val < target: return binary(mid+1, r)
        else: return binary(l, mid-1)
    return binary(0, rows*cols-1)
```

```python
def search_matrix_recursive(matrix, target):
    rows = len(matrix)
    cols = len(matrix[0])
    total = rows * cols

    def binary_search(left, right):
        if left > right:
            return False                               # base case: not found
        mid = (left + right) // 2
        val = matrix[mid // cols][mid % cols]          # map to 2D
        if val == target:
            return True                                 # found
        elif val < target:
            return binary_search(mid + 1, right)        # search right half
        else:
            return binary_search(left, mid - 1)         # search left half

    return binary_search(0, total - 1)

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_recursive(matrix, target))   # Output: True
```

---

## Using NumPy's `searchsorted`

**Explanation:**  
If NumPy is available, we can flatten the matrix into a NumPy array and use `np.searchsorted` to perform binary search efficiently.

**Approach:**  
- Convert matrix to a NumPy array and flatten.  
- Use `np.searchsorted` to find insertion point.  
- Check if the index is within bounds and the element matches.

**Algorithm:**  
1. `arr = np.array(matrix).flatten()`.  
2. `idx = np.searchsorted(arr, target)`.  
3. Return `idx < len(arr) and arr[idx] == target`.

**Pseudocode:**  
```
import numpy as np
function searchMatrix(matrix, target):
    arr = np.array(matrix).flatten()
    idx = np.searchsorted(arr, target)
    return idx < len(arr) and arr[idx] == target
```

```python
import numpy as np

def search_matrix_numpy(matrix, target):
    # Convert to numpy array and flatten
    arr = np.array(matrix).flatten()
    # Perform binary search to find insertion index
    idx = np.searchsorted(arr, target)
    # Check if target exists at that index
    return idx < len(arr) and arr[idx] == target

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_numpy(matrix, target))   # Output: True
```

---

## Using `functools.reduce` and `operator.contains`

**Explanation:**  
This is a more functional programming approach, though not the most efficient. It uses `reduce` to combine rows and then checks membership.

**Approach:**  
- Use `reduce` to concatenate all rows into a single list.  
- Then use the `in` operator to check for target.

**Algorithm:**  
1. `flat = reduce(list.__add__, matrix, [])`.  
2. Return `target in flat`.

**Pseudocode:**  
```
from functools import reduce
function searchMatrix(matrix, target):
    flat = reduce(lambda acc, row: acc + row, matrix, [])
    return target in flat
```

```python
from functools import reduce

def search_matrix_reduce(matrix, target):
    # Flatten matrix using reduce (list concatenation)
    flat = reduce(lambda acc, row: acc + row, matrix, [])
    # Check membership
    return target in flat                              # returns True if found

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_reduce(matrix, target))   # Output: True
```

---

## Using `any` and Generator Expression

**Explanation:**  
This variant uses a generator expression with `any` to check each element lazily. It short-circuits on the first match.

**Approach:**  
- Generate each element from the matrix using nested loops in a generator.  
- Use `any` to see if any element equals target.

**Algorithm:**  
1. Return `any(val == target for row in matrix for val in row)`.

**Pseudocode:**  
```
function searchMatrix(matrix, target):
    return any(val == target for row in matrix for val in row)
```

```python
def search_matrix_any(matrix, target):
    # Generator expression yields True as soon as target is found
    return any(val == target for row in matrix for val in row)

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_any(matrix, target))   # Output: True
```

---

## Using `map` and `list` to Flatten and `in`

**Explanation:**  
Another simple flattening approach using `map` and list comprehension to create a flat list, then check membership.

**Approach:**  
- Use a list comprehension that iterates over rows and elements to flatten.  
- Use `in` to check for target.

**Algorithm:**  
1. `flat = [val for row in matrix for val in row]`.  
2. Return `target in flat`.

**Pseudocode:**  
```
function searchMatrix(matrix, target):
    flat = [val for row in matrix for val in row]
    return target in flat
```

```python
def search_matrix_flatten(matrix, target):
    # Flatten using list comprehension
    flat = [val for row in matrix for val in row]
    # Check membership
    return target in flat

# Example call
matrix = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
target = 3
print(search_matrix_flatten(matrix, target))   # Output: True
```