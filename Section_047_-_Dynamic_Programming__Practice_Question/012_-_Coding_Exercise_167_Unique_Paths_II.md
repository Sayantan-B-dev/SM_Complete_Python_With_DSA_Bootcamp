# Unique Paths in a Grid with Obstacles – Space‑Optimized Dynamic Programming

## Problem / Purpose

Given an `m x n` grid where each cell is either `0` (free) or `1` (obstacle), calculate the number of unique paths from the top‑left corner to the bottom‑right corner, moving only **right** or **down** at each step. If a cell contains an obstacle, it cannot be part of any path. The function returns the total count of such paths.

The provided solution uses iterative dynamic programming (DP) with a single 1‑D array, achieving O(m×n) time and O(n) space.

---

## Full Code (Commented)

```python
def uniquePathsWithObstacles(grid):
    """
    Calculate the number of unique paths in a grid with obstacles,
    moving only right or down.

    :param grid: List[List[int]] -> 0 = free, 1 = obstacle
    :return: int -> Number of unique paths
    """
    # Edge case: empty grid
    if not grid or not grid[0]:
        return 0

    rows = len(grid)
    cols = len(grid[0])

    # If the start cell is blocked, no paths exist
    if grid[0][0] == 1:
        return 0

    # dp array represents the number of ways to reach each column
    # in the current row. Initialize with zeros and set start cell to 1.
    dp = [0] * cols
    dp[0] = 1   # starting point

    # Process each row
    for r in range(rows):
        for c in range(cols):
            # If current cell is an obstacle, it contributes 0 ways
            if grid[r][c] == 1:
                dp[c] = 0
            else:
                # For columns beyond the first, add paths from the left
                # (dp[c-1] already updated for this row, representing left neighbor)
                if c > 0:
                    dp[c] += dp[c - 1]

    # After processing all rows, dp[-1] holds the number of ways to reach
    # the bottom‑right corner (provided the last cell is free).
    return dp[-1]
```

---

## Explanation

The problem is a direct extension of the classic “unique paths” problem with the added constraint that blocked cells cannot be traversed. The recurrence remains the same: the number of ways to reach cell `(r, c)` is the sum of ways to reach `(r-1, c)` (above) and `(r, c-1)` (left), but only if the cell itself is free. If the cell is blocked, the value is 0.

A full 2‑D DP table would use O(m×n) space. However, because the recurrence only needs the current row and the previous row’s values, space can be reduced to O(n) by reusing a single array `dp` that represents the current row. The key difference from the standard problem is that obstacles must be handled carefully: when encountering a blocked cell, its DP value is set to 0, which effectively propagates the obstruction through the grid.

The algorithm initializes `dp` with zeros and sets `dp[0] = 1` (the starting cell). It then iterates over all rows, and for each column, updates `dp[c]` as follows:

- If `grid[r][c] == 1`, set `dp[c] = 0`.
- Otherwise, if `c > 0`, add `dp[c-1]` to `dp[c]`. Here, `dp[c]` originally contains the value from the previous row (the “above” cell) because it hasn’t been overwritten yet in this row. The addition of `dp[c-1]` (already updated for the current row) incorporates the “left” contribution.

This in‑place update works because the algorithm processes columns from left to right, so `dp[c-1]` already reflects the updated value for the current row. The previous row’s value at column `c` is automatically carried over until overwritten by the new row’s calculation.

After processing all rows, `dp[-1]` contains the number of paths to the bottom‑right corner, provided that cell is free; otherwise, the algorithm would have set it to 0 during processing.

---

## Mental Model

Imagine walking through the grid row by row, maintaining a single array of “counts” for the current row. Initially, the top row is processed: the start cell gets a count of 1, and all subsequent cells in that row accumulate counts from the left, unless blocked. As you move down to the next row, the array is updated in place: for each column, the new count is the sum of the old count (representing the cell above) and the count from the left (which was just computed in this row). Obstacles act as “walls” that reset the count to 0, preventing any path from passing through.

This process mirrors the actual flow of paths: the number of ways to reach a cell equals the ways coming from above (previous row’s value) plus ways coming from the left (already computed in the same row). The single array efficiently tracks these values without storing the entire grid.

---

## Algorithm

**Approach:** Bottom‑up dynamic programming with 1‑D space optimization, handling obstacles by resetting blocked cells to 0.

**Procedure:**
1. If the grid is empty or the start cell is blocked, return 0.
2. Let `rows = len(grid)`, `cols = len(grid[0])`.
3. Create an array `dp` of length `cols`, initialized to `0`. Set `dp[0] = 1`.
4. For each row `r` from `0` to `rows-1`:
   - For each column `c` from `0` to `cols-1`:
     - If `grid[r][c] == 1`:
         `dp[c] = 0`
     - Else if `c > 0`:
         `dp[c] = dp[c] + dp[c-1]`   (dp[c] holds the value from the previous row)
   - (For `c = 0`, nothing is added; it retains its previous row’s value unless blocked.)
5. Return `dp[-1]`.

---

## Execution Flow (ASCII Representation)

For the grid:
```
[[0, 0, 0],
 [0, 1, 0],
 [0, 0, 0]]
```

Initialize `dp = [1, 0, 0]` (dp[0]=1, others 0)

**Row 0 (r=0):**
- c=0: free → (c>0? no) → dp[0] stays 1
- c=1: free → dp[1] = dp[1] + dp[0] = 0 + 1 = 1
- c=2: free → dp[2] = dp[2] + dp[1] = 0 + 1 = 1
  dp = [1, 1, 1]

**Row 1 (r=1):**
- c=0: free → dp[0] stays 1
- c=1: obstacle → dp[1] = 0
- c=2: free → dp[2] = dp[2] + dp[1] = 1 + 0 = 1
  dp = [1, 0, 1]

**Row 2 (r=2):**
- c=0: free → dp[0] stays 1
- c=1: free → dp[1] = dp[1] + dp[0] = 0 + 1 = 1
- c=2: free → dp[2] = dp[2] + dp[1] = 1 + 1 = 2
  dp = [1, 1, 2]

Return `dp[-1] = 2`

---

## Recursion Tree

The solution is iterative and does not use recursion. A recursive approach would suffer from overlapping subproblems and exponential time without memoization. The DP approach avoids that by storing and reusing intermediate results.

---

## Complexity Analysis

**Time Complexity:** O(m × n). The nested loops iterate over each cell exactly once, performing constant‑time operations per cell. This is optimal because every cell must be examined to account for obstacles.

**Space Complexity:** O(n) due to the single 1‑D array of size `n` (the number of columns). The input grid is not modified, and only a few integer variables are used.

---

## Example Usage and Output

```python
grid = [[0, 0, 0],
        [0, 1, 0],
        [0, 0, 0]]
print(uniquePathsWithObstacles(grid))   # Output: 2
```

The two paths are:  
1. Right → Right → Down → Down  
2. Down → Down → Right → Right  
(Obstacle at (1,1) blocks the direct path through the center.)

---

## Edge Cases

- **Empty grid**: `[]` or `[[]]` → returns 0.
- **Start cell blocked**: `grid[0][0] == 1` → immediately return 0.
- **End cell blocked**: The algorithm will set `dp[-1]` to 0 during the last row’s processing, resulting in a correct return of 0.
- **All cells free**: Reduces to the standard unique paths problem, which returns the binomial coefficient.
- **Single row or single column**: Works correctly because the left‑neighbor updates propagate along the row; obstacles in the only row or column block further progress.