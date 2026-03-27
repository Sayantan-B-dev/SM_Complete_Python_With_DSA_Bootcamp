# Unique Paths in a Grid – Space‑Optimized Dynamic Programming

## Problem / Purpose

Given an `m x n` grid, calculate the number of unique paths from the top‑left corner to the bottom‑right corner, moving only **right** or **down** at each step. The function returns the total count of such paths.

The provided solution uses iterative dynamic programming (DP) with a single 1‑D array, achieving O(m×n) time and O(n) space.

---

## Full Code (Commented)

```python
def uniquePaths(m, n):
    """
    Calculate the number of unique paths in an m x n grid moving only right or down.

    :param m: int -> Number of rows
    :param n: int -> Number of columns
    :return: int -> Number of unique paths
    """
    # Handle edge cases where the grid has no rows or no columns
    if m == 0 or n == 0:
        return 0

    # dp array for the current row; initially set to 1 because the first row
    # has exactly one way to reach each cell (all right moves).
    dp = [1] * n

    # Process rows from the second row (index 1) to the last (index m-1)
    for row in range(1, m):
        # Process columns from the second column (index 1) to the last (index n-1)
        for col in range(1, n):
            # Number of ways to reach (row, col) = ways to come from above
            # (dp[col]) + ways to come from left (dp[col-1]).
            # dp[col] currently holds the value for the previous row at this column.
            dp[col] = dp[col] + dp[col - 1]

    # After processing all rows, dp[-1] holds the value for (m-1, n-1)
    return dp[-1]
```

---

## Explanation

The problem exhibits optimal substructure: the number of ways to reach cell `(i, j)` is the sum of ways to reach the cell above `(i-1, j)` and the cell to the left `(i, j-1)`, because any path to `(i, j)` must come from one of those two adjacent cells. This recurrence forms the basis of a dynamic programming solution.

A full 2‑D DP table would require O(m×n) space. However, because the recurrence only needs the previous row’s values to compute the current row, we can reduce space to O(n). The implementation uses a single array `dp` of length `n` to represent the current row. Initially, `dp` is set to all 1s because the first row has only one way to reach each cell (by moving right continuously).

For each subsequent row (from `1` to `m-1`), the array is updated from left to right. The update `dp[col] = dp[col] + dp[col-1]` works because:
- `dp[col]` before the update holds the value for the previous row at column `col` (the “above” cell).
- `dp[col-1]` has already been updated in this row and now holds the value for the current row at column `col-1` (the “left” cell). Thus, the new `dp[col]` correctly sums the above and left contributions.

After processing all rows, `dp[-1]` (the last element) contains the number of paths to the bottom‑right corner.

---

## Mental Model

Imagine walking through the grid row by row, keeping a single row of “counts” that you continuously refine. Start with the top row: every cell has exactly 1 way (you can only reach it by moving right from the start). As you move down to the next row, the first cell (column 0) is reachable only from above (since no left cell), so its count remains 1. For each subsequent cell, its count is the sum of the count directly above (from the previous row) and the count to its left (already computed in the current row). This incremental update propagates the number of paths through the grid.

The single array `dp` acts as a rolling window, holding the cumulative counts for the current row as they are built.

---

## Algorithm

**Approach:** Bottom‑up dynamic programming with 1‑D space optimization.

**Procedure:**
1. If either `m` or `n` is zero, return 0 (invalid grid).
2. Create an array `dp` of size `n` and initialize every element to `1` (first row).
3. For `row` from `1` to `m-1`:
   - For `col` from `1` to `n-1`:
     - Set `dp[col] = dp[col] + dp[col-1]`.
   - (The element at `dp[0]` remains `1` for every row because the leftmost column can only be reached from above.)
4. Return `dp[-1]`.

---

## Execution Flow (ASCII Representation)

For `m = 3`, `n = 7`:

```
Initialize dp = [1, 1, 1, 1, 1, 1, 1]

Row 1 (index 1):
  col 1: dp[1] = dp[1] + dp[0] = 1 + 1 = 2
  col 2: dp[2] = dp[2] + dp[1] = 1 + 2 = 3
  col 3: dp[3] = dp[3] + dp[2] = 1 + 3 = 4
  col 4: dp[4] = dp[4] + dp[3] = 1 + 4 = 5
  col 5: dp[5] = dp[5] + dp[4] = 1 + 5 = 6
  col 6: dp[6] = dp[6] + dp[5] = 1 + 6 = 7
  dp becomes [1, 2, 3, 4, 5, 6, 7]

Row 2 (index 2):
  col 1: dp[1] = dp[1] + dp[0] = 2 + 1 = 3
  col 2: dp[2] = dp[2] + dp[1] = 3 + 3 = 6
  col 3: dp[3] = dp[3] + dp[2] = 4 + 6 = 10
  col 4: dp[4] = dp[4] + dp[3] = 5 + 10 = 15
  col 5: dp[5] = dp[5] + dp[4] = 6 + 15 = 21
  col 6: dp[6] = dp[6] + dp[5] = 7 + 21 = 28
  dp becomes [1, 3, 6, 10, 15, 21, 28]

Return dp[-1] = 28
```

---

## Recursion Tree

The solution is iterative and does not use recursion. A recursive approach would compute the same values with overlapping subproblems (e.g., `uniquePaths(m-1, n) + uniquePaths(m, n-1)`), leading to exponential time without memoization. The DP implementation avoids that by storing intermediate results.

---

## Complexity Analysis

**Time Complexity:** O(m × n). The nested loops iterate over each cell of the grid exactly once, performing constant‑time work per cell. This is optimal because any algorithm must at least consider each cell to account for all paths.

**Space Complexity:** O(n) due to the single 1‑D array of size `n`. The input parameters and a few integer variables use O(1) additional space.

---

## Example Usage and Output

```python
print(uniquePaths(3, 7))   # Output: 28
print(uniquePaths(3, 2))   # Output: 3
```

For `m = 3`, `n = 2`, the grid has 3 rows and 2 columns. The paths are:  
(1) Right, Down, Down  
(2) Down, Right, Down  
(3) Down, Down, Right  
Total = 3, which matches the output.

---

## Edge Cases

- **Zero rows or columns** (`m == 0` or `n == 0`): Returns 0 because there is no valid grid.
- **Single row** (`m == 1`): The only way is to move right across all columns, so the answer is 1. The algorithm initializes `dp = [1] * n` and never enters the row loop, returning `dp[-1]` = 1.
- **Single column** (`n == 1`): Similarly, only one way (move down all rows). The algorithm returns 1 because `dp[-1]` is the first element, which remains 1 throughout.
- **Large values**: The numbers can become large (binomial coefficients). Python integers handle arbitrary precision, so no overflow occurs.