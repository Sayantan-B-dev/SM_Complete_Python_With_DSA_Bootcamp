# Minimum Falling Path Sum with Column Shift Constraint – Optimized Dynamic Programming Solution

## Problem / Purpose

Given a square matrix of integers, find the minimum sum of a falling path from top to bottom. A falling path starts at any element in the first row and progresses row by row. At each step, you may move to any element in the next row **except** the column directly below the current column (i.e., from `(r,c)` you can move to `(r+1, c')` for any `c' ≠ c`). This constraint forces a change of column in every step. The goal is to compute the minimum total sum over all such paths.

The solution uses dynamic programming with a space‑optimized approach that leverages the fact that for each row, we only need the two smallest values from the previous row to compute the current row’s DP values. This yields O(n²) time and O(n) space.

---

## Full Code (Commented)

```python
def minFallingPathSum(grid):
    """
    Compute the minimum falling path sum where you cannot use the same column
    in two consecutive rows.

    :param grid: List[List[int]] -> Square matrix of integers
    :return: int -> Minimum falling path sum with non‑zero column shifts
    """
    # Edge case: empty grid or first row missing
    if not grid or not grid[0]:
        return 0

    rows = len(grid)
    cols = len(grid[0])

    # DP array for the previous row; initialize with the first row
    dp = grid[0][:]   # copy to avoid mutating input

    # Process rows from second to last
    for r in range(1, rows):
        # Find the smallest and second smallest values in dp,
        # along with the index of the smallest.
        min_val = float('inf')
        second_min_val = float('inf')
        min_idx = -1

        for c in range(cols):
            val = dp[c]
            if val < min_val:
                # New minimum found: previous min becomes second min
                second_min_val = min_val
                min_val = val
                min_idx = c
            elif val < second_min_val:
                # Update second minimum only
                second_min_val = val

        # Build new_dp for the current row
        new_dp = [0] * cols
        for c in range(cols):
            # If current column equals the index of the minimum value,
            # we must use the second minimum because we cannot stay in the same column.
            if c == min_idx:
                new_dp[c] = grid[r][c] + second_min_val
            else:
                new_dp[c] = grid[r][c] + min_val

        # Move to next row
        dp = new_dp

    # Answer is the minimum value in the last computed DP row
    return min(dp)
```

---

## Explanation

The problem differs from the classic “falling path sum” by forbidding the use of the same column in consecutive rows. This constraint introduces a dependency: when computing the minimum sum ending at `(r,c)`, we must consider the minimum sum from the previous row **excluding** column `c`. Since we only need the smallest available value from the previous row for each current column, the natural DP transition becomes:

```
new_dp[c] = grid[r][c] + min(dp[c'] for all c' != c)
```

A naive implementation would compute this minimum for each `c` by scanning all columns of the previous row, leading to O(n³) time. However, note that the expression `min(dp[c'] for c' != c)` is either:

- The global minimum of `dp`, if the global minimum’s index is **not** `c`; or
- The second global minimum of `dp`, if the global minimum’s index **is** `c`.

Thus, for each row we only need to identify the smallest and second smallest values of the previous DP array, along with the index of the smallest. Then, for each column `c`, we add the current cell’s value to either the smallest (if `c` is not the index of the smallest) or to the second smallest (if `c` equals that index). This yields the correct `new_dp[c]` in O(1) per column after an O(n) pre‑scan of the previous row.

The implementation follows this optimized approach. It maintains a single DP array `dp` representing the best path sums for the previous row. For each subsequent row, it finds the two smallest values and their associated index, then builds `new_dp` accordingly. After processing all rows, the answer is the minimum value in the final `dp`.

---

## Mental Model

Think of the DP array as a “scoreboard” for the best sums ending in the current row. When moving to the next row, each cell in the new row must pair with the best available sum from the previous row, but cannot pair with the same column. The best available sum is either the global minimum or the second global minimum of the previous row, depending on whether the current column matches the column that gave the global minimum.

This approach treats the “forbidden column” as a small variation: the optimal previous value for a column is simply the global minimum, except when that minimum would cause a column conflict, in which case we take the next best value. This insight reduces the problem to maintaining only two values per row, eliminating the need to check all predecessors for each cell.

---

## Algorithm

**Approach:** Optimized iterative dynamic programming with tracking of two smallest values.

**Procedure:**
1. If the grid is empty, return 0.
2. Set `rows = len(grid)`, `cols = len(grid[0])`.
3. Initialize `dp` as a copy of the first row.
4. For `r` from 1 to `rows-1`:
   - Find `min_val`, `second_min_val`, and `min_idx` by scanning `dp`.
   - Create a new array `new_dp` of length `cols`.
   - For `c` from 0 to `cols-1`:
     - If `c == min_idx`:
         `new_dp[c] = grid[r][c] + second_min_val`
     - Else:
         `new_dp[c] = grid[r][c] + min_val`
   - Set `dp = new_dp`.
5. Return the minimum value in `dp`.

---

## Execution Flow (ASCII Representation)

For the matrix:
```
[[1, 2, 3],
 [4, 5, 6],
 [7, 8, 9]]
```

Initial `dp = [1, 2, 3]`

**Row 1 (index 1):**
- Scan `dp`: min_val=1 (c=0), second_min_val=2 (c=1)
- Build `new_dp`:
  - c=0 (equals min_idx): 4 + 2 = 6
  - c=1 (≠ min_idx): 5 + 1 = 6
  - c=2 (≠ min_idx): 6 + 1 = 7
  `new_dp = [6, 6, 7]` → `dp = [6, 6, 7]`

**Row 2 (index 2):**
- Scan `dp`: min_val=6 (c=0 or 1), second_min_val=6 (c=1 or 0), min_idx=0 (first occurrence)
- Build `new_dp`:
  - c=0 (equals min_idx): 7 + second_min_val(6) = 13
  - c=1 (≠ min_idx): 8 + min_val(6) = 14
  - c=2 (≠ min_idx): 9 + min_val(6) = 15
  `new_dp = [13, 14, 15]` → `dp = [13, 14, 15]`

Return `min(dp) = 13`

---

## Recursion Tree

The solution is iterative and does not use recursion. A recursive implementation would explore all paths with the forbidden‑column constraint, leading to exponential time. The DP approach avoids that by caching results per row.

---

## Complexity Analysis

**Time Complexity:** O(n²) where n is the number of rows (and columns). For each of the n−1 rows, we scan the previous DP array once (O(n)) to find the two smallest values, and then build the new DP array (O(n)). Hence the total is O(n²). This is optimal for processing all cells.

**Space Complexity:** O(n) due to the two 1‑D arrays of length n (`dp` and `new_dp`). The input grid is not modified.

---

## Example Usage and Output

```python
grid = [[1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]]

print(minFallingPathSum(grid))   # Output: 13
```

The output 13 corresponds to the path: 1 (row0 col0) → 5 (row1 col1) → 7 (row2 col0) → sum = 1+5+7 = 13. (Other valid paths also yield 13, such as 1→4→8 = 13.)

---

## Edge Cases

- **Empty grid**: `[]` or `[[]]` → returns 0.
- **Single row**: The DP array is the row itself; answer is the minimum of that row. The constraint is irrelevant because no consecutive rows exist.
- **Single column**: There is only one column. Since you cannot stay in the same column in consecutive rows, it is impossible to move from the first row to the second. However, the problem statement assumes a square matrix; a single‑column matrix with more than one row would have no valid paths. The given algorithm will handle this: the first row’s DP is `[value]`. In the next row, `min_idx` will be 0, and `second_min_val` remains `inf`. Adding `inf` to the cell would produce `inf`, and `min(dp)` would be `inf`. This correctly indicates no valid path. (If such inputs are disallowed, the algorithm still behaves predictably.)
- **Negative numbers**: The algorithm works correctly because comparisons handle negatives.
- **Large values**: No overflow concerns in Python.