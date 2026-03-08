## Can Matrix Be Rotated to Match Target: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Given two square matrices `mat` and `target`, determine if `mat` can be rotated by 0, 90, 180, or 270 degrees clockwise to become exactly equal to `target`. The solution checks all four possible rotations.

**Approach:**  
- Rotate the matrix step by step (90° clockwise each time) and compare with target after each rotation.  
- Rotation is done in-place using transpose and row reversal (or any equivalent method).  
- If a match is found before completing four rotations, return `True`; otherwise return `False`.

**Algorithm:**  
1. Let `n = len(mat)`.  
2. Repeat 4 times:  
   - If `mat == target`, return `True`.  
   - Rotate `mat` by 90° clockwise (transpose then reverse each row).  
3. Return `False`.

**Pseudocode:**  
```
function canBeRotated(mat, target):
    for _ in range(4):
        if mat == target:
            return True
        // rotate mat 90° clockwise
        mat = transpose(mat)
        mat = reverseRows(mat)
    return False
```

---

## Original In-Place Rotation Check

```python
def can_be_rotated_original(mat, target):
    n = len(mat)
    # Make a copy to avoid modifying original for comparison (though original code modifies)
    # We'll follow the original logic that modifies mat in-place
    for _ in range(4):
        if mat == target:                # compare current orientation with target
            return True
        # Rotate mat 90° clockwise in-place
        for i in range(n):
            for j in range(i, n):
                mat[i][j], mat[j][i] = mat[j][i], mat[i][j]   # transpose
        for row in mat:
            row.reverse()                  # reverse each row
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_original([row[:] for row in mat], target)  # pass a copy to preserve original
print("Can be rotated:", result)   # Output: Can be rotated: True
```

---

## Using `zip` and `reversed` for Rotation (Non-Modifying)

```python
def rotate_90(mat):
    # Return a new matrix rotated 90° clockwise
    return [list(reversed(row)) for row in zip(*mat)]

def can_be_rotated_zip(mat, target):
    # Work on copies to avoid modifying input
    current = [row[:] for row in mat]
    for _ in range(4):
        if current == target:
            return True
        current = rotate_90(current)          # create new rotated matrix
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_zip(mat, target)
print("Using zip:", result)   # Output: Using zip: True
```

---

## Using `numpy.rot90`

```python
import numpy as np

def can_be_rotated_numpy(mat, target):
    mat_np = np.array(mat)
    target_np = np.array(target)
    for k in range(4):
        if np.array_equal(mat_np, target_np):
            return True
        mat_np = np.rot90(mat_np, k=-1)      # rotate clockwise (k=-1 for 90° clockwise)
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_numpy(mat, target)
print("Using numpy:", result)   # Output: Using numpy: True
```

---

## Precompute All Rotations

```python
def rotate_90_ccw(mat):
    # Rotate 90° counter-clockwise (helper for building all rotations)
    return [list(row) for row in zip(*mat)][::-1]

def all_rotations(mat):
    # Return list of all 4 rotations (0°, 90°, 180°, 270° clockwise)
    rot0 = [row[:] for row in mat]
    rot1 = [list(reversed(row)) for row in zip(*mat)]          # 90° clockwise
    rot2 = [list(reversed(row)) for row in zip(*rot1)]         # 180°
    rot3 = [list(reversed(row)) for row in zip(*rot2)]         # 270°
    return [rot0, rot1, rot2, rot3]

def can_be_rotated_precompute(mat, target):
    for rotation in all_rotations(mat):
        if rotation == target:
            return True
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_precompute(mat, target)
print("Precomputed rotations:", result)   # Output: Precomputed rotations: True
```

---

## Recursive Rotation Check

```python
def can_be_rotated_recursive(mat, target, k=0):
    if k == 4:
        return False
    if mat == target:
        return True
    # Rotate mat 90° clockwise (new matrix)
    rotated = [list(reversed(row)) for row in zip(*mat)]
    return can_be_rotated_recursive(rotated, target, k + 1)

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_recursive(mat, target)
print("Recursive:", result)   # Output: Recursive: True
```

---

## Using `deque` and Rotating Rows/Columns

This variant uses `collections.deque` to rotate rows and columns conceptually.

```python
from collections import deque

def can_be_rotated_deque(mat, target):
    # Convert matrix to deque of deques for efficient rotation simulation
    n = len(mat)
    # Convert to deque of deques
    dq_mat = deque(deque(row) for row in mat)
    dq_target = deque(deque(row) for row in target)
    
    for _ in range(4):
        if dq_mat == dq_target:
            return True
        # Simulate 90° clockwise rotation using deques
        # Build new rotated matrix by taking elements from columns in reverse
        new_dq = deque()
        for col in range(n):
            new_row = deque()
            for row in range(n-1, -1, -1):
                new_row.append(dq_mat[row][col])
            new_dq.append(new_row)
        dq_mat = new_dq
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_deque(mat, target)
print("Using deque:", result)   # Output: Using deque: True
```

---

## Using String Representation and Rotation

Convert matrix to string and check rotated versions by string manipulation (for small matrices).

```python
def matrix_to_string(mat):
    return ''.join(str(num) for row in mat for num in row)

def rotate_string(s, n):
    # Rotate a string representation of an n x n matrix by 90° clockwise
    # This is more conceptual; for simplicity we'll rotate the matrix instead
    pass

def can_be_rotated_string(mat, target):
    # Convert both to tuples of tuples for hashability
    mat_tup = tuple(tuple(row) for row in mat)
    target_tup = tuple(tuple(row) for row in target)
    
    # Generate all rotations using tuple operations
    def rotate(t):
        return tuple(tuple(reversed(col)) for col in zip(*t))
    
    current = mat_tup
    for _ in range(4):
        if current == target_tup:
            return True
        current = rotate(current)
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_string(mat, target)
print("String representation (using tuples):", result)   # Output: True
```

---

## Using List Comprehension to Generate Rotated Versions

```python
def rotate_90_listcomp(mat):
    n = len(mat)
    return [[mat[n - 1 - j][i] for j in range(n)] for i in range(n)]

def can_be_rotated_listcomp(mat, target):
    current = [row[:] for row in mat]
    for _ in range(4):
        if current == target:
            return True
        current = rotate_90_listcomp(current)
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_listcomp(mat, target)
print("List comprehension rotation:", result)   # Output: True
```

---

## Mathematical Approach: Check via Transpose and Reverse without Explicit Rotation

```python
def can_be_rotated_math(mat, target):
    # Check if target is equal to mat rotated by any multiple of 90°
    # Without actually rotating, we can compare indices
    n = len(mat)
    # Define transformations for 0°, 90°, 180°, 270° clockwise
    for k in range(4):
        match = True
        for i in range(n):
            for j in range(n):
                # Get expected value from mat based on rotation
                if k == 0:
                    val = mat[i][j]
                elif k == 1:
                    val = mat[n - 1 - j][i]
                elif k == 2:
                    val = mat[n - 1 - i][n - 1 - j]
                else:  # k == 3
                    val = mat[j][n - 1 - i]
                if val != target[i][j]:
                    match = False
                    break
            if not match:
                break
        if match:
            return True
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_math(mat, target)
print("Mathematical index comparison:", result)   # Output: True
```

---

## Using `itertools.cycle` to Generate Rotations

```python
import itertools

def rotate_gen(mat):
    # Generator yielding successive 90° rotations
    current = [row[:] for row in mat]
    yield current
    for _ in range(3):
        current = [list(reversed(row)) for row in zip(*current)]
        yield current

def can_be_rotated_cycle(mat, target):
    for rotation in rotate_gen(mat):
        if rotation == target:
            return True
    return False

# Example call
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
target = [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
result = can_be_rotated_cycle(mat, target)
print("Using cycle generator:", result)   # Output: True
```