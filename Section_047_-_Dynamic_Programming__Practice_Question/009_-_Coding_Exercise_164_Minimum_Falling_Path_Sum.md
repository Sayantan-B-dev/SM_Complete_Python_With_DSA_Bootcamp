# Minimum Falling Path Sum – Dynamic Programming Solution

## Problem / Purpose

Given a square matrix of integers, find the minimum sum of a falling path. A falling path starts at any element in the first row and progresses row by row, moving to the element directly below, or to the left or right diagonal below (i.e., from `(r,c)` you can move to `(r+1,c-1)`, `(r+1,c)`, or `(r+1,c+1)`). The goal is to compute the minimum total sum over all such paths from top to bottom.

The provided solution uses iterative dynamic programming (DP) to compute the minimum path sum row by row, reusing a single DP array to achieve O(n²) time and O(n) space.

---

## Full Code (Commented)

```python
def minFallingPathSum(matrix):
    """
    Compute the minimum falling path sum in a square matrix.

    :param matrix: List[List[int]] -> Square matrix of integers
    :return: int -> Minimum falling path sum
    """
    # Edge case: empty matrix or first row missing
    if not matrix or not matrix[0]:
        return 0

    rows = len(matrix)
    cols = len(matrix[0])

    # DP array for the previous row; initialize with the first row
    dp = matrix[0][:]   # copy to avoid mutating input

    # Process rows from second to last
    for r in range(1, rows):
        new_dp = [0] * cols   # holds DP values for current row

        for c in range(cols):
            # Start with the value directly above (c)
            best_prev = dp[c]

            # Check left diagonal (c-1) if within bounds
            if c > 0:
                best_prev = min(best_prev, dp[c - 1])

            # Check right diagonal (c+1) if within bounds
            if c < cols - 1:
                best_prev = min(best_prev, dp[c + 1])

            # Add current cell value to the best previous sum
            new_dp[c] = matrix[r][c] + best_prev

        # Move to next row
        dp = new_dp

    # Answer is the minimum value in the last computed DP row
    return min(dp)
```

---

## Explanation

The problem exhibits optimal substructure: the minimum falling path sum ending at position `(r,c)` depends only on the minimum sums ending at the three possible previous positions `(r-1, c-1)`, `(r-1, c)`, and `(r-1, c+1)`. Therefore, a dynamic programming approach can compute these values row by row.

The implementation follows a bottom‑up (tabular) DP strategy. It maintains a one‑dimensional array `dp` representing the minimum falling path sums for the *previous* row. Initially, `dp` is set to the first row’s values because a path starting at any cell in the first row has a sum equal to the cell’s value.

For each subsequent row, a new array `new_dp` is computed. For each column `c`, the algorithm identifies the smallest value among the three reachable predecessors from the previous row (accounting for boundaries). The current cell’s value is added to that predecessor’s minimum, producing the minimum falling path sum ending at `(r,c)`.

After processing all rows, `dp` holds the minimum sums ending in the last row. The overall answer is the minimum value in that array.

By reusing arrays and processing row by row, the solution avoids storing the full DP table, reducing space complexity from O(n²) to O(n).

---

## Mental Model

Imagine walking through the matrix row by row from top to bottom. At each step in the new row, you look at the three cells immediately above (top, top‑left, top‑right) and choose the smallest path sum you have recorded for them. That value, plus the current cell’s value, becomes the minimum path sum to reach that cell.

The DP array acts as a running “scoreboard” for the current row’s path sums. As you move down, you update the scoreboard with the new row’s values, discarding the previous row’s information because it is no longer needed. This mimics a sliding window over the rows.

---

## Algorithm

**Approach:** Iterative Dynamic Programming with space optimization (1D DP array).

**Procedure:**
1. If the matrix is empty, return 0.
2. Set `rows = len(matrix)`, `cols = len(matrix[0])`.
3. Initialize `dp` as a copy of the first row.
4. For `r` from 1 to `rows-1`:
   - Create a new array `new_dp` of length `cols`.
   - For `c` from 0 to `cols-1`:
     - `best = dp[c]`
     - If `c > 0`, `best = min(best, dp[c-1])`
     - If `c < cols-1`, `best = min(best, dp[c+1])`
     - `new_dp[c] = matrix[r][c] + best`
   - Set `dp = new_dp`
5. Return the minimum value in `dp`.

---

## Execution Flow (ASCII Representation)

For the matrix:
```
[[2, 1, 3],
 [6, 5, 4],
 [7, 8, 9]]
```

Initial `dp = [2, 1, 3]`

Row 1 (index 1):
- c=0: best = min(dp[0]=2, dp[1]=1) = 1  → 6+1=7
- c=1: best = min(dp[1]=1, dp[0]=2, dp[2]=3) = 1 → 5+1=6
- c=2: best = min(dp[2]=3, dp[1]=1) = 1 → 4+1=5
  `new_dp = [7, 6, 5]` → `dp = [7, 6, 5]`

Row 2 (index 2):
- c=0: best = min(dp[0]=7, dp[1]=6) = 6 → 7+6=13
- c=1: best = min(dp[1]=6, dp[0]=7, dp[2]=5) = 5 → 8+5=13
- c=2: best = min(dp[2]=5, dp[1]=6) = 5 → 9+5=14
  `new_dp = [13, 13, 14]` → `dp = [13, 13, 14]`

Return `min(dp) = 13`

---

## Recursion Tree

This solution is iterative and does not use recursion. A recursive implementation would compute overlapping subproblems exponentially; the DP approach avoids that by storing intermediate results.

---

## Complexity Analysis

**Time Complexity:** O(n²) where n is the number of rows (and columns, because the matrix is square). Each of the n rows is processed, and each of the n columns is examined, performing a constant number of operations per cell (three lookups and a min operation). The overall number of operations is proportional to the total number of cells, i.e., n².

**Space Complexity:** O(n) due to the two 1‑D arrays of length n (`dp` and `new_dp`). The input matrix is not modified. No additional data structures that grow with input size are used.

---

## Example Usage and Output

```python
matrix = [[2, 1, 3],
          [6, 5, 4],
          [7, 8, 9]]

print(minFallingPathSum(matrix))   # Output: 13
```

The output 13 corresponds to the path: 1 (row0 col1) → 5 (row1 col1) → 7 (row2 col0) → sum = 1+5+7 = 13. (Other paths with sum 13 exist.)

---

## Edge Cases

- **Empty matrix**: `[]` or `[[]]` → returns 0 (handled by early check).
- **Single row**: The DP array is the row itself, and the answer is the minimum of that row.
- **Single column**: The only possible moves are directly down; the DP effectively sums the column’s values.
- **Negative numbers**: The algorithm works correctly because it uses min operations and simple addition.
- **Large values**: No overflow is considered in Python; the algorithm remains correct.