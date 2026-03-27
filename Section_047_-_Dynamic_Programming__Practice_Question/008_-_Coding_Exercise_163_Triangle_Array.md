# Minimum Path Sum in a Triangle: Bottom‑Up Dynamic Programming

## Table of Contents
1. [Problem Definition / Purpose](#problem-definition--purpose)
2. [Full Implementation](#full-implementation)
3. [Code Breakdown](#code-breakdown)
4. [Core Conceptual Model](#core-conceptual-model)
5. [Algorithm Design](#algorithm-design)
6. [Execution Flow](#execution-flow)
7. [Recursion Tree](#recursion-tree)
8. [Complexity Analysis](#complexity-analysis)
9. [Example Usage](#example-usage)
10. [Expected Output](#expected-output)
11. [Edge Cases and Constraints](#edge-cases-and-constraints)
12. [Key Observations](#key-observations)

---

## Problem Definition / Purpose
The function `minimumTotal(triangle)` computes the minimum path sum from the top to the bottom of a triangular array. At each step, you can move from an element to the element directly below (same index) or to the element diagonally below (index + 1). The goal is to find a path that minimizes the total sum of the elements visited.

---

## Full Implementation

```python
def minimumTotal(triangle):
    """
    Find the minimum path sum from top to bottom of a triangle.

    Parameters:
    triangle (List[List[int]]): Triangular array where triangle[i] has length i+1.

    Returns:
    int: Minimum sum of numbers along a path from top to bottom.
    """
    # Edge case: empty triangle -> no path, sum = 0.
    if not triangle:
        return 0

    # Initialize DP array with the last row of the triangle.
    # This array will store the minimum path sum from the current row to the bottom.
    dp = triangle[-1][:]   # copy to avoid modifying original input

    # Process rows from the second last up to the top.
    for row_index in range(len(triangle) - 2, -1, -1):
        # For each element in the current row, compute the minimum path sum
        # from that element to the bottom.
        for col_index in range(len(triangle[row_index])):
            # The element can go down (dp[col_index]) or down‑right (dp[col_index+1]).
            # Add the smaller of those two to the current element's value.
            dp[col_index] = triangle[row_index][col_index] + min(
                dp[col_index],       # directly below
                dp[col_index + 1]    # diagonal below
            )

    # After processing the top row, dp[0] holds the minimum total path sum.
    return dp[0]
```

---

## Code Breakdown

### Edge Case Handling
```python
if not triangle:
    return 0
```
- **What**: If the input triangle is empty (e.g., `[]`), return 0.
- **Why**: There is no path, so the sum is 0.

### DP Initialization
```python
dp = triangle[-1][:]   # copy to avoid modifying original input
```
- **What**: Creates a copy of the last row of the triangle.
- **Why**: In the bottom‑up approach, the minimum path sum from any element in the last row to the bottom is simply the element itself (since no further moves). This row serves as the base for the DP.

### Iteration Over Rows
```python
for row_index in range(len(triangle) - 2, -1, -1):
    for col_index in range(len(triangle[row_index])):
        dp[col_index] = triangle[row_index][col_index] + min(
            dp[col_index],       # path going down
            dp[col_index + 1]    # path going diagonally
        )
```
- **What**: The outer loop processes rows from the second‑last row up to the top (index decreasing). For each element in the current row, we update `dp[col_index]` to store the minimum sum from that element to the bottom.
- **Why**: The recurrence: `minPath(row, col) = triangle[row][col] + min(minPath(row+1, col), minPath(row+1, col+1))`. By working bottom‑up, we reuse the already computed values from the row below, which are stored in `dp`. This avoids the need for a 2D DP array.

### Return
```python
return dp[0]
```
- **What**: After processing the top row, `dp[0]` contains the minimum sum from the top to the bottom.
- **Why**: This matches the problem definition.

---

## Core Conceptual Model

### State Representation
Define `dp[row][col]` as the minimum path sum from element `(row, col)` to the bottom. The recurrence is:

```
dp[row][col] = triangle[row][col] + min(dp[row+1][col], dp[row+1][col+1])
```

Base: `dp[last_row][col] = triangle[last_row][col]`.

### Mental Model: Bottom‑Up Aggregation
Think of the triangle as a pyramid. Start at the bottom row: the minimum path from any bottom cell to the bottom is just that cell's value. Now move one level up. For a cell, you look at the two cells directly below it. The minimum path from that cell is its own value plus the smaller of the two minimal paths from the cells below. As you go up, you propagate the minimum sums upward, effectively "collapsing" the pyramid into a single value at the top.

The DP array `dp` initially holds the last row's values. After processing the row above, `dp` is updated in place to represent the minimum sums for that row. This continues until only the top element remains in `dp[0]`. The key is that `dp` always holds the minimal sums for the current row being processed.

### Why This Works
The problem exhibits optimal substructure: the optimal path from a cell depends only on the optimal paths from the cells directly below. Because we compute from the bottom up, each subproblem is solved exactly once, and we avoid redundant calculations (overlapping subproblems).

---

## Algorithm Design

### Approach: Bottom‑Up Dynamic Programming (In‑Place)
- **Rationale**: The recurrence `dp[row][col] = triangle[row][col] + min(dp[row+1][col], dp[row+1][col+1])` allows us to compute values row by row from bottom to top. Using a 1D array for the current row suffices because we only need the row below (stored in `dp`). This reduces space to O(n) where n is the number of columns in the last row.

### Step‑by‑Step Algorithm
1. If `triangle` is empty, return 0.
2. Copy the last row into `dp`.
3. For `row` from `len(triangle)-2` down to 0:
   - For `col` from 0 to `row`:
     - `dp[col] = triangle[row][col] + min(dp[col], dp[col+1])`
4. Return `dp[0]`.

### Pseudo‑Flow
```
if triangle empty: return 0
dp = copy of triangle last row
for row = len(triangle)-2 down to 0:
    for col = 0 to row:
        dp[col] = triangle[row][col] + min(dp[col], dp[col+1])
return dp[0]
```

---

## Execution Flow

### ASCII Representation of the Process (triangle = [[2],[3,4],[6,5,7],[4,1,8,3]])

```
Start
  |
  v
triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
  |
  v
Edge case? Not empty → continue
  |
  v
Initialize dp = last row = [4,1,8,3]
  |
  v
Process rows from bottom up:
  |
  +-- row = 2 (third row: [6,5,7])
  |   col=0: dp[0] = 6 + min(dp[0], dp[1]) = 6 + min(4,1) = 6+1 = 7
  |   col=1: dp[1] = 5 + min(dp[1], dp[2]) = 5 + min(1,8) = 5+1 = 6
  |   col=2: dp[2] = 7 + min(dp[2], dp[3]) = 7 + min(8,3) = 7+3 = 10
  |   dp now = [7,6,10,3]   (the last element 3 remains as it is never overwritten)
  |
  +-- row = 1 (second row: [3,4])
  |   col=0: dp[0] = 3 + min(dp[0], dp[1]) = 3 + min(7,6) = 3+6 = 9
  |   col=1: dp[1] = 4 + min(dp[1], dp[2]) = 4 + min(6,10) = 4+6 = 10
  |   dp now = [9,10,10,3]
  |
  +-- row = 0 (top row: [2])
      col=0: dp[0] = 2 + min(dp[0], dp[1]) = 2 + min(9,10) = 2+9 = 11
      dp now = [11,10,10,3]
  |
  v
Return dp[0] = 11
  |
  v
End
```

### Data Transformations Over Rows

| Row (index) | Row values       | dp before update | dp after update |
|-------------|------------------|------------------|-----------------|
| 3 (last)    | [4,1,8,3]        | –                | [4,1,8,3]       |
| 2           | [6,5,7]          | [4,1,8,3]        | [7,6,10,3]      |
| 1           | [3,4]            | [7,6,10,3]       | [9,10,10,3]     |
| 0           | [2]              | [9,10,10,3]      | [11,10,10,3]    |

---

## Recursion Tree

The problem can be expressed recursively: `f(row, col)` returns the minimum sum from that cell to the bottom. For a small triangle, the recursion tree would have many overlapping subproblems. For example, the call `f(0,0)` for the sample triangle would expand as:

```
                           f(0,0)
                     /               \
            f(1,0)                     f(1,1)
         /        \                 /        \
   f(2,0)         f(2,1)         f(2,1)      f(2,2)
  /      \       /      \       /      \    /      \
f(3,0) f(3,1) f(3,1) f(3,2) f(3,1) f(3,2) f(3,2) f(3,3)
```

Notice that `f(2,1)` and `f(3,1)`, `f(3,2)` appear multiple times. The bottom‑up DP computes each cell exactly once, eliminating this exponential redundancy.

---

## Complexity Analysis

### Time Complexity
- **O(m)**, where `m` is the total number of elements in the triangle. More precisely, it is O(n²) if the triangle has n rows (since total elements = n(n+1)/2). The algorithm processes each element exactly once (each inner loop runs once per element of each row except the last). Therefore, time is proportional to the number of elements.

### Space Complexity
- **O(n)**, where `n` is the number of rows (or equivalently the length of the last row). The `dp` array holds at most `n` integers. No additional data structures scale with the total number of elements.

### Why These Complexities
- The algorithm iterates over all elements of the triangle exactly once, performing constant work per element. The space is optimal because we only need to store one row at a time, not the entire triangle.

---

## Example Usage

```python
# Example triangle
triangle = [
    [2],
    [3, 4],
    [6, 5, 7],
    [4, 1, 8, 3]
]
print(minimumTotal(triangle))   # Output: 11

# Another triangle
print(minimumTotal([[-10]]))    # Output: -10
```

---

## Expected Output

For `triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]`:

- The minimum path sum is `2 → 3 → 5 → 1 = 11`. The algorithm computes this as shown in the execution flow.

Derivation: The path `2 → 3 → 5 → 1` yields sum 11. Other paths: `2 → 3 → 6 → 4` gives 15, `2 → 4 → 5 → 1` gives 12, etc. So 11 is correct.

---

## Edge Cases and Constraints

### Boundary Inputs
- **Empty triangle**: `[]` → returns 0.
- **Single row**: `[[x]]` → returns `x`.
- **All negative numbers**: Works correctly because the DP uses `min`, which will select the least negative sum.
- **Large triangle**: Handles up to thousands of rows efficiently, as long as integer overflow is not an issue (Python handles big integers).

### Invalid Inputs
- The input must be a valid triangle: row `i` has length `i+1`. The function does not validate this; it assumes correct input. If the triangle is malformed, the loops may raise index errors.

### Performance Limits
- For very large triangles, the O(total elements) time is optimal, as each element must be examined at least once.

---

## Key Observations
- This bottom‑up DP technique is a classic example of in‑place 1D DP for a triangular structure.
- The problem can also be solved top‑down with memoization, but bottom‑up avoids recursion overhead and is more space‑efficient.
- The recurrence shows that the path sum at each cell depends only on the two cells directly below, which makes the 1D array update straightforward.
- The algorithm modifies the `dp` array in place; we copy the last row to avoid altering the original input, but if input modification is allowed, we could work directly on the triangle to save even more space (though it's less safe).