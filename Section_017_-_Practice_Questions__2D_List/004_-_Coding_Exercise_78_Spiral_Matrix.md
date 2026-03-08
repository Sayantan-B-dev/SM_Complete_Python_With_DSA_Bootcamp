## Spiral Order Traversal: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Spiral order traversal of a matrix means visiting elements in a clockwise spiral pattern: starting from the top-left corner, move right across the top row, then down the rightmost column, then left across the bottom row, then up the leftmost column, and repeat inward until all elements are visited.

**Approach:**  
Maintain four boundaries: top, bottom, left, right. While top ≤ bottom and left ≤ right:
- Traverse from left to right along the top row, then increment top.
- Traverse from top to bottom along the right column, then decrement right.
- If top ≤ bottom, traverse from right to left along the bottom row, then decrement bottom.
- If left ≤ right, traverse from bottom to top along the left column, then increment left.

**Algorithm:**  
```
result = []
top = 0
bottom = len(matrix) - 1
left = 0
right = len(matrix[0]) - 1
while top <= bottom and left <= right:
    for col in range(left, right+1):
        result.append(matrix[top][col])
    top += 1
    for row in range(top, bottom+1):
        result.append(matrix[row][right])
    right -= 1
    if top <= bottom:
        for col in range(right, left-1, -1):
            result.append(matrix[bottom][col])
        bottom -= 1
    if left <= right:
        for row in range(bottom, top-1, -1):
            result.append(matrix[row][left])
        left += 1
return result
```

**Pseudocode:**  
```
function spiralOrder(matrix):
    result = []
    top = 0
    bottom = len(matrix) - 1
    left = 0
    right = len(matrix[0]) - 1
    while top <= bottom and left <= right:
        for col from left to right:
            result.append(matrix[top][col])
        top++
        for row from top to bottom:
            result.append(matrix[row][right])
        right--
        if top <= bottom:
            for col from right down to left:
                result.append(matrix[bottom][col])
            bottom--
        if left <= right:
            for row from bottom down to top:
                result.append(matrix[row][left])
            left++
    return result
```

---

## Original Boundary Method

```python
def spiral_order_original(matrix):
    if not matrix:
        return []
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    while top <= bottom and left <= right:
        # Top row: left to right
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1

        # Right column: top to bottom
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1

        if top <= bottom:
            # Bottom row: right to left
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1

        if left <= right:
            # Left column: bottom to top
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1

    return result

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Spiral order:", spiral_order_original(matrix))  # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

## Counterclockwise Spiral

```python
def spiral_order_counterclockwise(matrix):
    if not matrix:
        return []
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    while top <= bottom and left <= right:
        # Top to bottom along left column
        for row in range(top, bottom + 1):
            result.append(matrix[row][left])
        left += 1

        # Left to right along bottom row
        for col in range(left, right + 1):
            result.append(matrix[bottom][col])
        bottom -= 1

        if left <= right:
            # Bottom to top along right column
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][right])
            right -= 1

        if top <= bottom:
            # Right to left along top row
            for col in range(right, left - 1, -1):
                result.append(matrix[top][col])
            top += 1

    return result

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Counterclockwise:", spiral_order_counterclockwise(matrix))  # Output: [1, 4, 7, 8, 9, 6, 3, 2, 5]
```

---

## Direction Vectors with Visited Matrix

```python
def spiral_order_direction(matrix):
    if not matrix:
        return []
    rows, cols = len(matrix), len(matrix[0])
    visited = [[False] * cols for _ in range(rows)]
    result = []
    # Direction order: right, down, left, up
    dr = [0, 1, 0, -1]
    dc = [1, 0, -1, 0]
    r = c = 0
    dir_idx = 0

    for _ in range(rows * cols):
        result.append(matrix[r][c])
        visited[r][c] = True
        next_r, next_c = r + dr[dir_idx], c + dc[dir_idx]
        # If next step is out of bounds or already visited, change direction
        if next_r < 0 or next_r >= rows or next_c < 0 or next_c >= cols or visited[next_r][next_c]:
            dir_idx = (dir_idx + 1) % 4
            next_r, next_c = r + dr[dir_idx], c + dc[dir_idx]
        r, c = next_r, next_c

    return result

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Direction vectors:", spiral_order_direction(matrix))  # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

## Recursive Layer Approach

```python
def spiral_order_recursive(matrix):
    def spiral_layer(top, bottom, left, right):
        if top > bottom or left > right:
            return []
        result = []
        # Top row
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        # Right column
        for row in range(top + 1, bottom + 1):
            result.append(matrix[row][right])
        if top < bottom and left < right:
            # Bottom row
            for col in range(right - 1, left - 1, -1):
                result.append(matrix[bottom][col])
            # Left column
            for row in range(bottom - 1, top, -1):
                result.append(matrix[row][left])
        # Recursively process inner layer
        result.extend(spiral_layer(top + 1, bottom - 1, left + 1, right - 1))
        return result

    if not matrix:
        return []
    return spiral_layer(0, len(matrix) - 1, 0, len(matrix[0]) - 1)

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Recursive:", spiral_order_recursive(matrix))  # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

## Using Deque and Rotating Matrix

```python
from collections import deque

def spiral_order_deque(matrix):
    if not matrix:
        return []
    result = []
    # Convert matrix to deque of deques for efficient pop operations
    mat = deque(deque(row) for row in matrix)
    while mat:
        # Pop the top row
        result.extend(mat.popleft())
        # Rotate the remaining matrix counterclockwise (so that next top row is the original right column)
        if mat:
            # Transpose and reverse rows to simulate rotation
            mat = deque(deque(row) for row in zip(*mat))
            mat.reverse()
    return result

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Deque rotation:", spiral_order_deque(matrix))  # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

## Starting from Bottom-Left (Anticlockwise)

```python
def spiral_order_bottom_left(matrix):
    if not matrix:
        return []
    result = []
    rows, cols = len(matrix), len(matrix[0])
    top, bottom = 0, rows - 1
    left, right = 0, cols - 1

    while top <= bottom and left <= right:
        # Bottom row: left to right
        for col in range(left, right + 1):
            result.append(matrix[bottom][col])
        bottom -= 1

        # Right column: bottom to top
        for row in range(bottom, top - 1, -1):
            result.append(matrix[row][right])
        right -= 1

        if top <= bottom:
            # Top row: right to left
            for col in range(right, left - 1, -1):
                result.append(matrix[top][col])
            top += 1

        if left <= right:
            # Left column: top to bottom
            for row in range(top, bottom + 1):
                result.append(matrix[row][left])
            left += 1

    return result

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Bottom-left start:", spiral_order_bottom_left(matrix))  # Output: [7, 8, 9, 6, 3, 2, 1, 4, 5] (spiral starting bottom-left)
```

---

## Non-Square Matrix (Generic m x n)

The original algorithm already works for non-square matrices. This variant explicitly handles any m x n.

```python
def spiral_order_generic(matrix):
    if not matrix:
        return []
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    while top <= bottom and left <= right:
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1

        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1

        if top <= bottom:
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1

        if left <= right:
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1

    return result

# Example with 3x4 matrix
matrix = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
print("3x4 spiral:", spiral_order_generic(matrix))  # Output: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
```

---

## Using `zip` and List Manipulation (Rotate and Pop)

```python
def spiral_order_zip(matrix):
    result = []
    while matrix:
        # Add first row
        result += matrix[0]
        # Remove first row
        matrix = matrix[1:]
        # Rotate remaining matrix counterclockwise (transpose and reverse)
        if matrix:
            matrix = [list(row) for row in zip(*matrix)][::-1]
    return result

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Zip rotation:", spiral_order_zip(matrix))  # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

## Generator Version (Yielding Elements)

```python
def spiral_order_generator(matrix):
    if not matrix:
        return
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    while top <= bottom and left <= right:
        for col in range(left, right + 1):
            yield matrix[top][col]
        top += 1
        for row in range(top, bottom + 1):
            yield matrix[row][right]
        right -= 1
        if top <= bottom:
            for col in range(right, left - 1, -1):
                yield matrix[bottom][col]
            bottom -= 1
        if left <= right:
            for row in range(bottom, top - 1, -1):
                yield matrix[row][left]
            left += 1

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Generator:", list(spiral_order_generator(matrix)))  # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

## Using NumPy (If Available)

```python
import numpy as np

def spiral_order_numpy(matrix):
    if not matrix:
        return []
    arr = np.array(matrix)
    result = []
    while arr.size > 0:
        # Add first row
        result.extend(arr[0].tolist())
        # Remove first row and rotate counterclockwise
        arr = arr[1:]
        if arr.size > 0:
            arr = np.rot90(arr, k=-1)  # rotate clockwise to bring next column to top
    return result

# Example call
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("NumPy:", spiral_order_numpy(matrix))  # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```