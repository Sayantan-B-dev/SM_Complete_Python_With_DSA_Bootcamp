# Counting Negative Numbers in a Sorted Matrix

**Explanation:**  
The function counts the number of negative integers in a matrix where each row and each column is sorted in non‑increasing order. It leverages this property to achieve an O(m + n) time complexity by traversing the matrix from the top‑right corner.

**Approach:**  
Start at the top‑right corner. If the current element is negative, all elements below it in the same column are also negative (because columns are non‑increasing). Therefore, add the number of rows from the current row to the bottom to the count and move left to the next column. If the current element is non‑negative, move down to the next row (since elements to the left are larger or equal and cannot be negative). Continue until all rows and columns are processed.

**Algorithm:**  
1. Let `rows` = number of rows in the matrix, `cols` = number of columns.  
2. Initialize `row = 0`, `col = cols - 1` (top‑right corner), `count = 0`.  
3. While `row < rows` and `col >= 0`:  
   - If `grid[row][col] < 0`:  
        - Add `(rows - row)` to `count` (all elements from current row downward in this column are negative).  
        - Move left: `col -= 1`.  
   - Else:  
        - Move down: `row += 1`.  
4. Return `count`.

**Pseudocode:**  
```
function countNegatives(grid):
    rows = length(grid)
    cols = length(grid[0])
    row = 0
    col = cols - 1
    count = 0

    while row < rows and col >= 0:
        if grid[row][col] < 0:
            count = count + (rows - row)
            col = col - 1
        else:
            row = row + 1

    return count
```

## Brute Force Nested Loop
```python
def countNegatives(grid):
    # Main logic: iterate through each row and each cell
    count = 0
    for row in grid:
        for val in row:
            if val < 0:
                count += 1
    return count  # Return total negative count

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Flatten with List Comprehension
```python
def countNegatives(grid):
    # Main logic: flatten the matrix and count negatives in one comprehension
    return len([val for row in grid for val in row if val < 0])  # Return length of filtered list

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Using itertools.chain
```python
import itertools

def countNegatives(grid):
    # Main logic: chain all rows into a single iterator and sum boolean conditions
    return sum(1 for val in itertools.chain.from_iterable(grid) if val < 0)

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Sum with Generator Expression
```python
def countNegatives(grid):
    # Main logic: generator yields 1 for each negative, sum adds them
    return sum(val < 0 for row in grid for val in row)

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Binary Search Per Row (Rows Sorted Descending)
```python
import bisect

def countNegatives(grid):
    # Main logic: for each row, use binary search to find first negative index
    # Since rows are non-increasing, we can search for the insertion point of 0
    count = 0
    for row in grid:
        # bisect_left returns index where 0 would be inserted to maintain order
        # In a descending list, we need to reverse or use custom key; simpler: use negative of values
        # Alternative: use bisect on reversed row (ascending) of negatives
        # Here we leverage that row is sorted descending, so negative region is contiguous at the end.
        # We can find the first element < 0 using binary search manually:
        lo, hi = 0, len(row)
        while lo < hi:
            mid = (lo + hi) // 2
            if row[mid] < 0:
                hi = mid
            else:
                lo = mid + 1
        # lo is index of first negative; negatives count = len(row) - lo
        count += len(row) - lo
    return count

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Two-Pointer from Bottom-Left (Original Variant)
```python
def countNegatives(grid):
    rows, cols = len(grid), len(grid[0])
    row, col = rows - 1, 0  # Start at bottom-left
    count = 0
    # Main logic: move upward when negative, right when non-negative
    while row >= 0 and col < cols:
        if grid[row][col] < 0:
            count += cols - col  # All to the right in this row are negative
            row -= 1
        else:
            col += 1
    return count

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Using NumPy (Professional/External Library)
```python
import numpy as np

def countNegatives(grid):
    # Main logic: convert to numpy array and use vectorized comparison
    arr = np.array(grid)
    return np.sum(arr < 0)  # Return count of elements where condition is True

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Using functools.reduce on Rows
```python
from functools import reduce

def countNegatives(grid):
    # Main logic: reduce rows by adding counts, starting with 0
    def row_count(acc, row):
        return acc + sum(1 for x in row if x < 0)
    return reduce(row_count, grid, 0)

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Recursive Divide and Conquer
```python
def countNegatives(grid):
    # Helper recursive function on submatrix defined by rows and columns range
    def helper(r_start, r_end, c_start, c_end):
        if r_start > r_end or c_start > c_end:
            return 0
        # If top-left of submatrix is negative, all cells in submatrix are negative (since sorted)
        if grid[r_start][c_start] < 0:
            return (r_end - r_start + 1) * (c_end - c_start + 1)
        # If bottom-right is non-negative, no negatives in submatrix
        if grid[r_end][c_end] >= 0:
            return 0
        # Split into four quadrants and recurse
        r_mid = (r_start + r_end) // 2
        c_mid = (c_start + c_end) // 2
        return (helper(r_start, r_mid, c_start, c_mid) +
                helper(r_start, r_mid, c_mid + 1, c_end) +
                helper(r_mid + 1, r_end, c_start, c_mid) +
                helper(r_mid + 1, r_end, c_mid + 1, c_end))
    
    if not grid or not grid[0]:
        return 0
    return helper(0, len(grid)-1, 0, len(grid[0])-1)

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```

## Using map with Binary Search (Functional Style)
```python
import bisect

def countNegatives(grid):
    # Main logic: map each row to count of negatives using binary search
    # Since rows are descending, we can reverse row to make ascending and use bisect_left
    def count_in_row(row):
        # Reverse row to ascending order, then find first >=0 (i.e., insertion point of 0)
        # But reversing creates copy; we can use bisect on negative values as before
        lo, hi = 0, len(row)
        while lo < hi:
            mid = (lo + hi) // 2
            if row[mid] < 0:
                hi = mid
            else:
                lo = mid + 1
        return len(row) - lo
    return sum(map(count_in_row, grid))

# Call the function and print output
result = countNegatives([[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]])
print(result)  # Output: 8
```