```py
def min_cost_path_tabulation(cost,m,n,dp):
    
    # base case
    dp[0][0] = cost[0][0]

    for row in range(m+1):
        for col in range(n+1):
            if(row==0 and col==0):
                dp[row][col] = cost[0][0]
            else:
                cost_left = dp[row][col-1] if col>0 else float('inf')
                cost_top = dp[row-1][col] if row >0 else float('inf')
                cost_diag = dp[row-1][col-1] if ( row>0 and col>0) else float('inf')

                dp[row][col] = cost[row][col] + min(cost_diag,cost_top,cost_left)
    '''
    for col in range(n+1):
        for row in range(m+1):

    '''

    return dp[m][n]
# Test

cost_matrix = [
    [1, 2, 3],
    [4, 8, 2],
    [1, 5, 3]
]
M, N = 2, 2
memo = [ [-1 for i in range(N+1)]   for j in range(M+1) ]
dp = [ [0 for i in range(N+1)]   for j in range(M+1) ]

print(f"Minimum cost to reach ({M}, {N}) using Recursion: {min_cost_path_tabulation(cost_matrix, M, N,dp)}")



for row in dp:
    print(row)

'''
[-1, 3, 6]
[5, 9, 5]
[6, 10, 8]
'''
```
# Minimum Cost Path – Tabulation (Bottom‑Up Dynamic Programming)

## 1. Problem Restatement

We are given a 2‑D grid `cost` of positive integers. Starting at the top‑left cell `(0,0)`, we want to reach the target cell `(m, n)` by moving only **right**, **down**, or **diagonally down‑right**. The total cost of a path is the sum of the costs of all visited cells (including start and end). The goal is to find the **minimum possible cost**.

The function `min_cost_path_tabulation(cost, m, n, dp)` implements a **bottom‑up** (tabular) dynamic programming solution, where we build a table `dp` that stores the minimum cost to reach each cell `(i, j)`.

---

## 2. Code with Line‑by‑Line Explanation

```python
def min_cost_path_tabulation(cost, m, n, dp):
    
    # base case
    dp[0][0] = cost[0][0]

    for row in range(m+1):
        for col in range(n+1):
            if(row == 0 and col == 0):
                dp[row][col] = cost[0][0]
            else:
                cost_left = dp[row][col-1] if col>0 else float('inf')
                cost_top  = dp[row-1][col] if row>0 else float('inf')
                cost_diag = dp[row-1][col-1] if (row>0 and col>0) else float('inf')

                dp[row][col] = cost[row][col] + min(cost_diag, cost_top, cost_left)
    
    return dp[m][n]
```

### Line‑by‑Line Explanation

- **Line 1**: Function definition – takes the cost matrix, the target coordinates `(m, n)`, and a pre‑allocated 2‑D table `dp` (likely passed in as a list of lists of zeros).
- **Line 4**: Sets the starting cell's cost. This is the minimum cost to reach `(0,0)`.
- **Line 6‑7**: Outer loop over all rows from `0` to `m` inclusive; inner loop over all columns from `0` to `n` inclusive.
- **Line 8‑9**: For the start cell `(0,0)`, we simply set its value (redundant because we already set it, but it's fine).
- **Line 10‑15**: For all other cells, we compute the minimum of the three possible predecessors (left, top, diagonal) if they exist; if a predecessor does not exist (i.e., we are on the first row or first column), we treat its cost as `infinity` so it never becomes the minimum.
  - `cost_left` = the minimum cost to reach the cell to the left, `(row, col-1)`. If `col > 0`, we take `dp[row][col-1]`; else, `inf`.
  - `cost_top` = similarly from the cell above.
  - `cost_diag` = from the cell diagonally up‑left.
- **Line 15**: The minimum cost to reach `(row, col)` is the cost of the current cell plus the smallest of the three predecessor costs.
- **Line 18**: After filling the entire table, return the value at `(m, n)`.

Note: The loops are written such that we fill every cell in row‑major order. The commented alternative loops (col‑major) would also work; the order does not matter as long as we compute predecessors before the current cell.

---

## 3. Algorithm Description (Bottom‑Up DP)

The recurrence relation is the same as before:

```
minCost(i, j) = cost[i][j] + min(
    minCost(i, j-1),   // from left
    minCost(i-1, j),   // from top
    minCost(i-1, j-1)  // from diagonal
)
```

with base `minCost(0,0) = cost[0][0]` and any cell outside the grid has infinite cost.

**Bottom‑up approach**: Instead of starting from the target and recursing downwards, we start from the base cell `(0,0)` and iteratively fill the table for all cells in increasing order of rows and columns. Because each cell depends only on cells with smaller row and/or column indices, processing in row‑major order guarantees that when we compute `dp[i][j]`, all its predecessors `dp[i][j-1]`, `dp[i-1][j]`, and `dp[i-1][j-1]` have already been computed.

---

## 4. Step‑by‑Step Example with Visualization

Let's use the same 3×3 cost matrix:

```
cost = [
    [1, 2, 3],
    [4, 8, 2],
    [1, 5, 3]
]
```

We want to reach `(2,2)`. We'll fill the `dp` table.

### Initialisation

We create a `dp` table of size `3×3` (all zeros initially). The code sets `dp[0][0] = cost[0][0] = 1`.

### Iteration

We'll go row by row, column by column.

**Row 0, Col 0**: already set.

**Row 0, Col 1**:  
- `cost_left = dp[0][0] = 1` (col>0)  
- `cost_top = inf` (row=0)  
- `cost_diag = inf` (row=0 or col=0)  
- `min = 1`  
- `dp[0][1] = cost[0][1] + 1 = 2 + 1 = 3`

**Row 0, Col 2**:  
- `cost_left = dp[0][1] = 3`  
- `cost_top = inf`  
- `cost_diag = inf`  
- `min = 3`  
- `dp[0][2] = cost[0][2] + 3 = 3 + 3 = 6`

**Row 1, Col 0**:  
- `cost_left = inf` (col=0)  
- `cost_top = dp[0][0] = 1`  
- `cost_diag = inf` (col=0)  
- `min = 1`  
- `dp[1][0] = cost[1][0] + 1 = 4 + 1 = 5`

**Row 1, Col 1**:  
- `cost_left = dp[1][0] = 5`  
- `cost_top = dp[0][1] = 3`  
- `cost_diag = dp[0][0] = 1`  
- `min = 1`  
- `dp[1][1] = cost[1][1] + 1 = 8 + 1 = 9`

**Row 1, Col 2**:  
- `cost_left = dp[1][1] = 9`  
- `cost_top = dp[0][2] = 6`  
- `cost_diag = dp[0][1] = 3`  
- `min = 3`  
- `dp[1][2] = cost[1][2] + 3 = 2 + 3 = 5`

**Row 2, Col 0**:  
- `cost_left = inf` (col=0)  
- `cost_top = dp[1][0] = 5`  
- `cost_diag = inf` (col=0)  
- `min = 5`  
- `dp[2][0] = cost[2][0] + 5 = 1 + 5 = 6`

**Row 2, Col 1**:  
- `cost_left = dp[2][0] = 6`  
- `cost_top = dp[1][1] = 9`  
- `cost_diag = dp[1][0] = 5`  
- `min = 5`  
- `dp[2][1] = cost[2][1] + 5 = 5 + 5 = 10`

**Row 2, Col 2**:  
- `cost_left = dp[2][1] = 10`  
- `cost_top = dp[1][2] = 5`  
- `cost_diag = dp[1][1] = 9`  
- `min = 5`  
- `dp[2][2] = cost[2][2] + 5 = 3 + 5 = 8`

Final `dp` table:

```
   0  1  2
0  1  3  6
1  5  9  5
2  6 10  8
```

The answer is `dp[2][2] = 8`, matching our earlier results.

### Visualisation of Table Filling Order

We can visualise the order of computation (row‑major):

```
(0,0) → (0,1) → (0,2)
 ↓       ↓       ↓
(1,0) → (1,1) → (1,2)
 ↓       ↓       ↓
(2,0) → (2,1) → (2,2)
```

Every cell is computed after its left, top, and diagonal neighbours (which are already computed because we go row by row, left to right).

---

## 5. Complexity Analysis

### Time Complexity

We process each cell exactly once. For each cell, we perform a constant number of operations (three comparisons, one addition). There are `(m+1) × (n+1)` cells. Thus the time complexity is **O(m × n)**.

### Space Complexity

We store a 2‑D `dp` table of size `(m+1) × (n+1)`. Therefore space complexity is **O(m × n)**.  
*However*, we can reduce this to **O(n)** (or **O(min(m, n))** ) by noting that for each row we only need the previous row and the current row, because the recurrence only uses the current row's left cell, the previous row's same column, and the previous row's left cell. This is a typical space optimisation in grid DP.

---

## 6. Advantages and Disadvantages

### Advantages

- **No recursion**: Avoids function call overhead and recursion depth limits.
- **Predictable**: Iterative, easy to reason about.
- **Easily optimisable for space**: Can be implemented using only two rows (or even one row with careful handling of diagonal).
- **Faster in practice**: Loops are generally faster than recursive calls with memoization.

### Disadvantages

- **Computes all cells**: Even if we only need the value for `(m, n)`, we still compute every cell up to it. For large grids, this is unavoidable because the recurrence depends on all intermediate cells.
- **Less intuitive** for people who think recursively; the recursive memoization version may be easier to derive directly from the recurrence.

---

## 7. Mental Model – How Bottom‑Up Works

Imagine you are building a city grid. You want to know the cheapest way to get from the top‑left corner to each intersection. You start at the starting point: you already know the cost to be there. Then you expand outward:

- For the first row, you can only come from the left, so you just accumulate costs.
- For the first column, you can only come from above.
- For inner cells, you look at the three possible ways to reach them (from left, above, or diagonal), pick the cheapest, and add the cost of the current cell.

You fill the grid systematically so that when you get to a cell, all its predecessors are already filled. This is exactly what the nested loops do.

The `dp` table becomes a complete map of minimum costs to every cell. Finally, you look up the value at your destination.

---

## 8. Potential Improvements / Space Optimisation

We can reduce space from `O(m×n)` to `O(n)` by using two rows:

```python
def min_cost_path_tabulation_optimized(cost, m, n):
    prev_row = [float('inf')] * (n+1)
    curr_row = [float('inf')] * (n+1)
    prev_row[0] = cost[0][0]  # dp[0][0]
    for i in range(m+1):
        for j in range(n+1):
            if i == 0 and j == 0:
                curr_row[0] = cost[0][0]
            else:
                left = curr_row[j-1] if j > 0 else float('inf')
                top = prev_row[j] if i > 0 else float('inf')
                diag = prev_row[j-1] if i > 0 and j > 0 else float('inf')
                curr_row[j] = cost[i][j] + min(left, top, diag)
        prev_row, curr_row = curr_row, [float('inf')] * (n+1)
    return prev_row[n]  # after last iteration, prev_row holds last row
```

But the given code uses a full table for clarity.

---

## 9. Comparison with Memoization (Top‑Down)

| Feature               | Memoization (Top‑Down)            | Tabulation (Bottom‑Up)             |
|-----------------------|-----------------------------------|------------------------------------|
| **Implementation**    | Recursive with caching            | Iterative, explicit table          |
| **Order**             | Starts from target, goes down to base | Starts from base, builds up        |
| **Subproblem computation** | Only those reachable from target | All cells up to target             |
| **Stack usage**       | O(m+n) recursion depth            | No recursion, constant stack       |
| **Speed**             | Slightly slower due to function calls | Faster (simple loops)              |
| **Space**             | O(m×n) memo + stack overhead      | O(m×n) table (can be reduced to O(n)) |
| **Ease of understanding** | More intuitive for recurrences | More intuitive for iterative thinkers |

Both are valid DP solutions; the choice often depends on personal preference and the constraints (e.g., recursion depth limits favour tabulation).

---

## 10. Conclusion

The tabulation (bottom‑up) approach to the minimum cost path problem builds the solution iteratively, filling a table from the start cell outward. It is efficient (`O(m×n)` time) and avoids recursion overhead. The code provided correctly implements this algorithm, though it includes a redundant base case inside the loops. Understanding this approach is essential for tackling many grid‑based DP problems and serves as a foundation for more advanced dynamic programming techniques.