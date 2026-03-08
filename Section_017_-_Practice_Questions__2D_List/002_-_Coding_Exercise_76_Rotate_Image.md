## Rotate Matrix (Image) 90 Degrees Clockwise: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Rotating a square matrix by 90 degrees clockwise means that the first row becomes the last column, the second row becomes the second-last column, etc. The given function first transposes the matrix (swap rows and columns) and then reverses each row to achieve the clockwise rotation.

**Approach:**  
- Transpose the matrix: for all i < j, swap matrix[i][j] with matrix[j][i].  
- Reverse each row. This yields the 90-degree clockwise rotated matrix.

**Algorithm:**  
1. Let `n = len(matrix)`.  
2. For `i = 0` to `n-1`:  
   For `j = i` to `n-1`:  
     Swap `matrix[i][j]` and `matrix[j][i]`.  
3. For each `row` in `matrix`:  
   Reverse `row`.  
4. (Optional) Return the matrix.

**Pseudocode:**  
```
procedure rotate(matrix):
    n = length(matrix)
    for i = 0 to n-1:
        for j = i to n-1:
            swap(matrix[i][j], matrix[j][i])
    for each row in matrix:
        reverse(row)
```

---

## Original In-Place Transpose + Reverse

```python
def rotate_original(matrix):
    n = len(matrix)
    # Transpose: swap elements across the diagonal
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # Reverse each row to complete clockwise rotation
    for row in matrix:
        row.reverse()
    return matrix   # return for convenience

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_original(mat)
print("Rotated clockwise:")
for row in rotated:
    print(row)
# Output:
# [7, 4, 1]
# [8, 5, 2]
# [9, 6, 3]
```

---

## Rotate Anticlockwise (Transpose + Reverse Columns)

```python
def rotate_anticlockwise(matrix):
    n = len(matrix)
    # Transpose
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # Reverse each column to get anticlockwise rotation
    for i in range(n):
        for j in range(n // 2):
            matrix[j][i], matrix[n - 1 - j][i] = matrix[n - 1 - j][i], matrix[j][i]
    return matrix

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_anticlockwise(mat)
print("Rotated anticlockwise:")
for row in rotated:
    print(row)
# Output:
# [3, 6, 9]
# [2, 5, 8]
# [1, 4, 7]
```

---

## Using `zip` and Reversed (Create New Matrix)

```python
def rotate_zip(matrix):
    # zip(*matrix) gives tuples of columns; reversed gives clockwise rotation
    return [list(reversed(col)) for col in zip(*matrix)]

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_zip(mat)
print("Using zip and reversed:")
for row in rotated:
    print(row)
# Output same as clockwise
```

---

## Using List Comprehension with `zip` and `reversed` (Another Way)

```python
def rotate_listcomp(matrix):
    # Equivalent: take last column first, etc.
    n = len(matrix)
    return [[matrix[n - 1 - j][i] for j in range(n)] for i in range(n)]

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_listcomp(mat)
print("List comprehension mapping:")
for row in rotated:
    print(row)
```

---

## Using `numpy.rot90`

```python
import numpy as np

def rotate_numpy(matrix):
    # Convert to numpy array, rotate clockwise (k=1 means 90° clockwise)
    arr = np.array(matrix)
    rotated = np.rot90(arr, k=-1)  # or k=3 for clockwise? Actually rot90 rotates anticlockwise by default, so k=-1 for clockwise
    return rotated.tolist()

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_numpy(mat)
print("Using numpy (clockwise):")
for row in rotated:
    print(row)
```

---

## In-Place Rotation by Layers (Ring by Ring)

```python
def rotate_layers(matrix):
    n = len(matrix)
    for layer in range(n // 2):
        first = layer
        last = n - 1 - layer
        for i in range(first, last):
            offset = i - first
            # Save top
            top = matrix[first][i]
            # left -> top
            matrix[first][i] = matrix[last - offset][first]
            # bottom -> left
            matrix[last - offset][first] = matrix[last][last - offset]
            # right -> bottom
            matrix[last][last - offset] = matrix[i][last]
            # top -> right
            matrix[i][last] = top
    return matrix

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_layers(mat)
print("Layer-by-layer rotation:")
for row in rotated:
    print(row)
```

---

## Reverse Rows Then Transpose (Clockwise Alternative)

```python
def rotate_reverse_then_transpose(matrix):
    # First reverse rows, then transpose
    n = len(matrix)
    # Reverse rows
    for row in matrix:
        row.reverse()
    # Transpose
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    return matrix

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_reverse_then_transpose(mat)
print("Reverse then transpose (also clockwise):")
for row in rotated:
    print(row)
```

---

## Using Recursion (Divide and Conquer)

```python
def rotate_recursive(matrix):
    def helper(submat, offset, size):
        if size <= 1:
            return
        # Rotate the outer ring of the current submatrix
        for i in range(size - 1):
            temp = submat[0][offset + i]
            submat[0][offset + i] = submat[size - 1 - i][offset]
            submat[size - 1 - i][offset] = submat[size - 1][offset + size - 1 - i]
            submat[size - 1][offset + size - 1 - i] = submat[i][offset + size - 1]
            submat[i][offset + size - 1] = temp
        # Recursively rotate inner matrix
        helper(submat, offset + 1, size - 2)
    
    n = len(matrix)
    helper(matrix, 0, n)
    return matrix

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_recursive(mat)
print("Recursive rotation:")
for row in rotated:
    print(row)
```

---

## Using `map` and `reversed`

```python
def rotate_map(matrix):
    # map(list, zip(*matrix)) gives columns, then reversed for clockwise
    return list(map(list, reversed(list(zip(*matrix)))))

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_map(mat)
print("Using map and reversed:")
for row in rotated:
    print(row)
```

---

## Using `deque` and Rotate (Not for 2D, but can be adapted)

This variant treats matrix as list of rows and rotates using deque on columns. It's a bit contrived but shows another way.

```python
from collections import deque

def rotate_deque(matrix):
    # Convert rows to deque for efficient pop
    n = len(matrix)
    # Build new matrix by popping from each column
    rotated = []
    for i in range(n):
        new_row = deque()
        for j in range(n - 1, -1, -1):
            new_row.append(matrix[j][i])
        rotated.append(list(new_row))
    return rotated

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
rotated = rotate_deque(mat)
print("Using deque for column extraction:")
for row in rotated:
    print(row)
```