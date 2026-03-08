## Matrix Reshape: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Reshaping a matrix means changing its dimensions while preserving the total number of elements. The elements are taken row-wise from the original matrix and placed row-wise into the new shape. If the total elements do not match, return the original matrix unchanged.

**Approach:**  
- Check if `rows * cols == r * c`. If not, return original.  
- Flatten the original matrix (conceptually) and fill the new matrix row by row.  
- Use index mapping: for each element at position `k` (0-indexed) in flattened order, its new position is `new_row = k // c`, `new_col = k % c`.

**Algorithm:**  
1. `rows = len(mat)`, `cols = len(mat[0])`.  
2. If `rows * cols != r * c`, return `mat`.  
3. Create a 2D list `result` of size `r x c` filled with zeros.  
4. Initialize `index = 0`.  
5. For `i` from 0 to `rows-1`:  
   For `j` from 0 to `cols-1`:  
     Set `result[index // c][index % c] = mat[i][j]`.  
     Increment `index`.  
6. Return `result`.

**Pseudocode:**  
```
function matrixReshape(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    result = [[0]*c for _ in range(r)]
    index = 0
    for i in range(rows):
        for j in range(cols):
            result[index // c][index % c] = mat[i][j]
            index++
    return result
```

---

## Original Iterative with Index

```python
def matrix_reshape_original(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    # Check dimension compatibility
    if rows * cols != r * c:
        return mat
    # Create new matrix with zeros
    result = [[0] * c for _ in range(r)]
    index = 0
    # Main logic: flatten and fill row-wise
    for i in range(rows):
        for j in range(cols):
            # Compute new row and column using index
            result[index // c][index % c] = mat[i][j]
            index += 1
    return result

# Example call
mat = [[1, 2], [3, 4]]
r, c = 1, 4
reshaped = matrix_reshape_original(mat, r, c)
print("Reshaped:", reshaped)   # Output: Reshaped: [[1, 2, 3, 4]]
```

---

## Flatten and Re-chunk with List Comprehension

```python
def matrix_reshape_flat(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Flatten the matrix using nested list comprehension
    flat = [mat[i][j] for i in range(rows) for j in range(cols)]
    # Build new matrix by slicing the flat list
    return [flat[i * c:(i + 1) * c] for i in range(r)]

# Example call
mat = [[1, 2], [3, 4]]
r, c = 1, 4
reshaped = matrix_reshape_flat(mat, r, c)
print("Flatten then chunk:", reshaped)   # Output: [[1, 2, 3, 4]]
```

---

## Using `itertools.chain` to Flatten

```python
from itertools import chain

def matrix_reshape_chain(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Flatten using chain.from_iterable
    flat = list(chain.from_iterable(mat))
    # Build rows by slicing
    return [flat[i * c:(i + 1) * c] for i in range(r)]

# Example call
mat = [[1, 2], [3, 4]]
r, c = 4, 1
reshaped = matrix_reshape_chain(mat, r, c)
print("Using itertools.chain:", reshaped)   # Output: [[1], [2], [3], [4]]
```

---

## Using NumPy's `reshape` (if available)

```python
import numpy as np

def matrix_reshape_numpy(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Convert to numpy array and reshape
    arr = np.array(mat)
    reshaped = arr.reshape(r, c)
    return reshaped.tolist()

# Example call
mat = [[1, 2], [3, 4]]
r, c = 1, 4
reshaped = matrix_reshape_numpy(mat, r, c)
print("NumPy reshape:", reshaped)   # Output: [[1, 2, 3, 4]]
```

---

## Recursive Build with Flatten and Pop

```python
def matrix_reshape_recursive(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Flatten helper
    def flatten(matrix):
        if not matrix:
            return []
        return matrix[0] + flatten(matrix[1:])   # recursive flatten
    flat = flatten(mat)
    # Build rows recursively
    def build(flat_list, r, c):
        if r == 0:
            return []
        return [flat_list[:c]] + build(flat_list[c:], r - 1, c)
    return build(flat, r, c)

# Example call
mat = [[1, 2], [3, 4]]
r, c = 1, 4
reshaped = matrix_reshape_recursive(mat, r, c)
print("Recursive:", reshaped)   # Output: [[1, 2, 3, 4]]
```

---

## Using `zip` and Slicing (Flatten with `sum`)

```python
def matrix_reshape_sum(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Flatten using sum (concatenates lists)
    flat = sum(mat, [])
    # Build rows by slicing
    return [flat[i * c:(i + 1) * c] for i in range(r)]

# Example call
mat = [[1, 2], [3, 4]]
r, c = 4, 1
reshaped = matrix_reshape_sum(mat, r, c)
print("Using sum flatten:", reshaped)   # Output: [[1], [2], [3], [4]]
```

---

## Using `collections.deque` and Loops

```python
from collections import deque

def matrix_reshape_deque(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Push all elements into a deque
    dq = deque()
    for row in mat:
        dq.extend(row)
    # Pop elements to fill new matrix
    result = []
    for i in range(r):
        new_row = []
        for j in range(c):
            new_row.append(dq.popleft())
        result.append(new_row)
    return result

# Example call
mat = [[1, 2], [3, 4]]
r, c = 1, 4
reshaped = matrix_reshape_deque(mat, r, c)
print("Deque:", reshaped)   # Output: [[1, 2, 3, 4]]
```

---

## One-Liner with List Comprehension and Index Calculation

```python
def matrix_reshape_oneliner(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Single list comprehension: generate new matrix directly
    return [[mat[(i * c + j) // cols][(i * c + j) % cols] for j in range(c)] for i in range(r)]

# Example call
mat = [[1, 2], [3, 4]]
r, c = 1, 4
reshaped = matrix_reshape_oneliner(mat, r, c)
print("One-liner index mapping:", reshaped)   # Output: [[1, 2, 3, 4]]
```

---

## Using `map` and `zip` to Chunk Flattened List

```python
def matrix_reshape_map(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Flatten using list comprehension
    flat = [x for row in mat for x in row]
    # Chunk using map and zip
    chunks = [flat[i::c] for i in range(c)]  # not exactly; better to use:
    # Actually, a common chunking method: zip(*[iter(flat)]*c)
    iterator = iter(flat)
    result = list(map(list, zip(*[iterator] * c)))
    return result

# Example call
mat = [[1, 2], [3, 4]]
r, c = 1, 4
reshaped = matrix_reshape_map(mat, r, c)
print("Using zip(iter) chunking:", reshaped)   # Output: [[1, 2, 3, 4]]
```

---

## Using `numpy` and `flatten` (Alternative)

```python
import numpy as np

def matrix_reshape_numpy_alt(mat, r, c):
    rows = len(mat)
    cols = len(mat[0])
    if rows * cols != r * c:
        return mat
    # Using numpy's ravel (flatten) and reshape
    arr = np.array(mat)
    return arr.ravel().reshape(r, c).tolist()

# Example call
mat = [[1, 2], [3, 4]]
r, c = 1, 4
reshaped = matrix_reshape_numpy_alt(mat, r, c)
print("NumPy ravel+reshape:", reshaped)   # Output: [[1, 2, 3, 4]]
```