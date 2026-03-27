# Minimum Cost Path in a Grid

## Problem Definition

We are given a 2‑D grid (matrix) `cost` of dimensions `(R, C)` where each cell `(i, j)` contains a positive integer cost. We start at the top‑left corner `(0, 0)` and want to reach the bottom‑right corner `(M, N)` (where `M = R-1`, `N = C-1`). Allowed moves are:

- **Right** from `(i, j)` to `(i, j+1)`
- **Down** from `(i, j)` to `(i+1, j)`
- **Diagonal (down‑right)** from `(i, j)` to `(i+1, j+1)`

The cost of a path is the sum of the costs of all visited cells (including the starting and ending cells). We are to find the **minimum possible cost** among all such paths.

---

## Mathematical Formulation

Let `minCost(i, j)` denote the minimum cost to reach cell `(i, j)` from `(0, 0)`.  
The recurrence relation is built from the fact that the last step to `(i, j)` must come from one of the three possible predecessor cells:

- From above: `(i-1, j)` via a **down** move.
- From left: `(i, j-1)` via a **right** move.
- From diagonal: `(i-1, j-1)` via a **diagonal** move.

Thus, if `i > 0` or `j > 0`, the minimum cost to `(i, j)` is:

```
minCost(i, j) = cost[i][j] + min(
    minCost(i-1, j)   if i > 0,
    minCost(i, j-1)   if j > 0,
    minCost(i-1, j-1) if i > 0 and j > 0
)
```

**Base case:**  
`minCost(0, 0) = cost[0][0]` (starting cell cost included).

**Boundary conditions:**  
For cells on the first row (`i = 0, j > 0`), only the left move is possible:  
`minCost(0, j) = cost[0][j] + minCost(0, j-1)`.  
For cells on the first column (`j = 0, i > 0`), only the down move is possible:  
`minCost(i, 0) = cost[i][0] + minCost(i-1, 0)`.

---

## Dynamic Programming Solution

The recurrence exhibits **optimal substructure** (the optimal solution to a larger problem depends on optimal solutions to subproblems) and **overlapping subproblems** (the same sub‑problem `minCost(i, j)` is needed multiple times). Therefore, dynamic programming is the natural choice.

### Top‑Down (Memoization) Approach

- Define a recursive function `minCost(i, j)` that returns the minimum cost to reach `(i, j)`.
- Use a memo table `dp` of size `(M+1) × (N+1)` initialized with a sentinel (e.g., `-1`).
- Base case: `dp[0][0] = cost[0][0]`.
- For other cells, compute recursively using the recurrence and store the result in `dp[i][j]`.
- Time complexity: `O(M*N)` because each cell is computed at most once.
- Space complexity: `O(M*N)` for the memo table, plus recursion stack depth `O(M+N)`.

### Bottom‑Up (Tabulation) Approach

- Create a 2‑D array `dp` of size `(M+1) × (N+1)`.
- Initialize `dp[0][0] = cost[0][0]`.
- Fill the first row: `dp[0][j] = dp[0][j-1] + cost[0][j]` for `j = 1..N`.
- Fill the first column: `dp[i][0] = dp[i-1][0] + cost[i][0]` for `i = 1..M`.
- For each cell `(i, j)` with `i ≥ 1` and `j ≥ 1`, compute:
  ```
  dp[i][j] = cost[i][j] + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
  ```
- Answer is `dp[M][N]`.
- Time complexity: `O(M*N)`.
- Space complexity: `O(M*N)` (can be reduced to `O(N)` if we only keep the previous row because the recurrence uses only the current row, previous row, and diagonal from previous row).

---

## Step‑by‑Step Example (with ASCII Diagram)

Suppose the cost matrix is:

```
cost = [
    [1, 2, 3],
    [4, 8, 2],
    [1, 5, 3]
]
M = 2, N = 2.
```

We fill the dp table:

1. **Initialize** `dp[0][0] = 1`.

2. **First row** (j = 1..2):
   - `dp[0][1] = dp[0][0] + cost[0][1] = 1 + 2 = 3`
   - `dp[0][2] = dp[0][1] + cost[0][2] = 3 + 3 = 6`

3. **First column** (i = 1..2):
   - `dp[1][0] = dp[0][0] + cost[1][0] = 1 + 4 = 5`
   - `dp[2][0] = dp[1][0] + cost[2][0] = 5 + 1 = 6`

4. **Cell (1,1)**:  
   Options: from (0,1)=3, from (1,0)=5, from (0,0)=1.  
   Minimum = 1.  
   `dp[1][1] = cost[1][1] + 1 = 8 + 1 = 9`

5. **Cell (1,2)**:  
   Options: from (0,2)=6, from (1,1)=9, from (0,1)=3.  
   Minimum = 3.  
   `dp[1][2] = cost[1][2] + 3 = 2 + 3 = 5`

6. **Cell (2,1)**:  
   Options: from (1,1)=9, from (2,0)=6, from (1,0)=5.  
   Minimum = 5.  
   `dp[2][1] = cost[2][1] + 5 = 5 + 5 = 10`

7. **Cell (2,2)**:  
   Options: from (1,2)=5, from (2,1)=10, from (1,1)=9.  
   Minimum = 5.  
   `dp[2][2] = cost[2][2] + 5 = 3 + 5 = 8`

The minimum cost to reach `(2,2)` is `8`.  
One optimal path: `(0,0) → (0,1) → (1,2) → (2,2)` with costs: 1 + 2 + 2 + 3 = 8.

**ASCII Grid** (cost values, then dp values):

```
Cost matrix:
  0   1   2
0 1   2   3
1 4   8   2
2 1   5   3

DP table:
  0   1   2
0 1   3   6
1 5   9   5
2 6  10   8
```

---

## Complexity and Optimization

- **Time**: `O(M*N)` – we process each cell once.
- **Space**: `O(M*N)` for the DP table.  
  **Space optimization**: Since each cell depends only on the current row, the previous row, and the diagonal from the previous row, we can keep just two rows (or even one row if we store diagonal separately). The optimised space is `O(N)`.

---

## Variants and Extensions

- If diagonal moves are not allowed, the recurrence simplifies to `min(dp[i-1][j], dp[i][j-1])`. This is the classic "minimum path sum" problem.
- If costs can be negative, the problem becomes a shortest path in a DAG, and the DP still works because the graph is acyclic.
- For very large grids (e.g., 10⁶ cells), the `O(M*N)` time is still acceptable; space can be reduced as above.

---

## Conclusion

The minimum cost path in a grid with moves right, down, and diagonal is a classic dynamic programming problem. The recurrence expresses the optimal cost to each cell as the cost of the cell plus the minimum of the three possible predecessors. Both top‑down and bottom‑up DP yield `O(M*N)` solutions, with the bottom‑up approach being more common due to its iterative nature and ease of space optimisation. Understanding this problem lays the foundation for many other grid‑based DP problems.