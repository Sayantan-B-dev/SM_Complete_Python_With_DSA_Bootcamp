```py
def min_cost_path_memoization(cost,m,n,memo):
    # Base Case
    if(m==0 and n==0):
        return cost[0][0]

    #Out of Bounds
    if(m<0 or n<0):
        return float('inf')
    
    if(memo[m][n] !=-1):
        return memo[m][n]
    
    # Recursive Calls
    cost_left = min_cost_path_memoization(cost,m,n-1,memo)
    cost_top = min_cost_path_memoization(cost,m-1,n,memo)
    cost_diag = min_cost_path_memoization(cost,m-1,n-1,memo)

    ans = cost[m][n] + min(cost_left,cost_top,cost_diag)

    memo[m][n] = ans
    return ans
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
# Minimum Cost Path – Memoization (Top‑Down Dynamic Programming)





## 1. Problem Restatement

We are given a 2‑D grid `cost` of positive integers. Starting at `(0,0)`, we want to reach a target cell `(m, n)` moving only **right**, **down**, or **diagonally down‑right**. The path cost is the sum of the costs of all visited cells. The goal is to find the **minimum possible cost**.

The function `min_cost_path_memoization(cost, m, n, memo)` uses a **memoization** (top‑down DP) approach to compute this minimum efficiently by storing intermediate results.

---

## 2. Code with Line‑by‑Line Explanation

```python
def min_cost_path_memoization(cost, m, n, memo):
    # Base Case
    if (m == 0 and n == 0):
        return cost[0][0]

    # Out of Bounds
    if (m < 0 or n < 0):
        return float('inf')

    # Check memo
    if memo[m][n] != -1:
        return memo[m][n]

    # Recursive Calls
    cost_left = min_cost_path_memoization(cost, m, n-1, memo)
    cost_top  = min_cost_path_memoization(cost, m-1, n, memo)
    cost_diag = min_cost_path_memoization(cost, m-1, n-1, memo)

    # Our Work
    ans = cost[m][n] + min(cost_left, cost_top, cost_diag)

    # Store result
    memo[m][n] = ans
    return ans
```

### Quick note:
1. Solve problem using recursion.
2. Crete recursion tree and see if subproblems are oerlapping.
3. Find out unique sub problems.
4. Create the memo space.
5. Definewhat each box or indevidual memory os space signify.
6. Write down code and solve the solution.


### Test
```py
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

### Line‑by‑Line Explanation

- **Line 1**: Function definition – takes the cost matrix, target coordinates `(m, n)`, and a memoization table `memo`.
- **Line 3‑4**: **Base case**: If we are at the start cell, the minimum cost is just the cost of that cell.
- **Line 7‑8**: **Out‑of‑bounds check**: If either coordinate is negative, this path is invalid. We return infinity so that it will never be chosen as a minimum.
- **Line 10‑11**: **Memoization check**: If the result for `(m, n)` has already been computed and stored (i.e., `memo[m][n] != -1`), we return it immediately. This avoids recomputation.
- **Line 14‑16**: **Recursive calls**:
  - `cost_left`: minimum cost to reach the cell to the left, `(m, n-1)`. From there we can move **right** to `(m, n)`.
  - `cost_top`: minimum cost to reach the cell above, `(m-1, n)`. From there we can move **down**.
  - `cost_diag`: minimum cost to reach the cell diagonally up‑left, `(m-1, n-1)`. From there we can move **diagonally**.
- **Line 19**: The cost to reach `(m, n)` is the cost of this cell plus the **minimum** of the three possible predecessor costs.
- **Line 21‑22**: Store the computed result in the memo table and return it.

---

## 3. Algorithm Description (Top‑Down Dynamic Programming)

This function implements the same recurrence as the naive recursive solution:

```
minCost(i, j) = cost[i][j] + min(
    minCost(i, j-1),
    minCost(i-1, j),
    minCost(i-1, j-1)
)
```

with base `minCost(0,0) = cost[0][0]` and invalid cells having infinite cost.

The key addition is **memoization**: results for each `(i, j)` are stored in a 2‑D table `memo` (initialised with a sentinel, e.g., `-1`). Before recursing, we check if the answer is already known; if so, we return it directly. This transforms the exponential recursion into a linear one because each subproblem is solved only once.

The function is **top‑down** because it starts from the target and recursively moves towards the base case, storing results along the way.

---

## 4. Step‑by‑Step Example with Visualization

Let's use the same 3×3 cost matrix as before:

```
cost = [
    [1, 2, 3],
    [4, 8, 2],
    [1, 5, 3]
]
```

We want to find the minimum cost to reach `(2,2)`. We'll trace the memoization process.

### Initialisation

We create a `memo` table of size `3×3` filled with `-1` (meaning not computed). The function is called with `(2,2)`.

### Call `(2,2)`

- Not base case; `memo[2][2] == -1`, so we compute.
- Recursive calls:
  1. `cost_left = min_cost_path_memoization(2,1)`
  2. `cost_top = min_cost_path_memoization(1,2)`
  3. `cost_diag = min_cost_path_memoization(1,1)`

We'll follow the leftmost branch first.

### Branch: `(2,1)`

- `memo[2][1] == -1`.
- Recursive calls:
  - `(2,0)`
  - `(1,1)`
  - `(1,0)`

#### `(2,0)`

- `memo[2][0] == -1`.
- Calls: `(2,-1)` → inf, `(1,0)`, `(1,-1)` → inf.
- We need `(1,0)`.

##### `(1,0)`

- `memo[1][0] == -1`.
- Calls: `(1,-1)` → inf, `(0,0)`, `(0,-1)` → inf.
- `(0,0)` returns 1.
- `ans = cost[1][0] + min(inf, 1, inf) = 4 + 1 = 5`
- Store `memo[1][0] = 5`, return 5.

Back to `(2,0)`:
- `cost_left = inf`, `cost_top = 5`, `cost_diag = inf`.
- `ans = cost[2][0] + min(inf, 5, inf) = 1 + 5 = 6`
- Store `memo[2][0] = 6`, return 6.

#### Back to `(2,1)`:

We have `(2,0) = 6`. Next need `(1,1)`.

##### `(1,1)`

- `memo[1][1] == -1`.
- Calls: `(1,0)`, `(0,1)`, `(0,0)`.
  - `(1,0)` is already in memo: 5.
  - `(0,0)` = 1.
  - Need `(0,1)`.

###### `(0,1)`

- `memo[0][1] == -1`.
- Calls: `(0,0)`, `(-1,1)` → inf, `(-1,0)` → inf.
- `ans = cost[0][1] + min(1, inf, inf) = 2 + 1 = 3`
- Store `memo[0][1] = 3`, return 3.

Back to `(1,1)`:
- `cost_left = 5`, `cost_top = 3`, `cost_diag = 1`.
- Min = 1.
- `ans = cost[1][1] + 1 = 8 + 1 = 9`
- Store `memo[1][1] = 9`, return 9.

#### Next `(1,0)` already memoized = 5.

Now `(2,1)` has:
- `cost_left = 6`
- `cost_top = 9`
- `cost_diag = 5`
- Min = 5.
- `ans = cost[2][1] + 5 = 5 + 5 = 10`
- Store `memo[2][1] = 10`, return 10.

### Back to original `(2,2)` – we have `(2,1) = 10`. Next we need `(1,2)`.

#### `(1,2)`

- `memo[1][2] == -1`.
- Calls: `(1,1)`, `(0,2)`, `(0,1)`.
  - `(1,1)` = 9.
  - `(0,1)` = 3.
  - Need `(0,2)`.

##### `(0,2)`

- `memo[0][2] == -1`.
- Calls: `(0,1)`, `(-1,2)` → inf, `(-1,1)` → inf.
- `ans = cost[0][2] + min(3, inf, inf) = 3 + 3 = 6`
- Store `memo[0][2] = 6`, return 6.

Back to `(1,2)`:
- `cost_left = 9`, `cost_top = 6`, `cost_diag = 3`.
- Min = 3.
- `ans = cost[1][2] + 3 = 2 + 3 = 5`
- Store `memo[1][2] = 5`, return 5.

#### Next `(1,1)` already = 9.

Now `(2,2)` has:
- `cost_left = 10`
- `cost_top = 5`
- `cost_diag = 9`
- Min = 5.
- `ans = cost[2][2] + 5 = 3 + 5 = 8`
- Store `memo[2][2] = 8`, return 8.

The answer is **8**, matching the naive recursive result.

### Memo Table After Computation

After the entire run, the memo table (values are minimum costs) looks like:

```
   0  1  2
0  1  3  6
1  5  9  5
2  6 10  8
```

Every cell was computed exactly once, and subsequent lookups (like `(1,1)` needed from multiple branches) were served from the memo.

---

## 5. Recursion Tree vs. Memoization Tree (Visual)

**Naïve Recursion** (without memo) would generate a full tree where nodes repeat:

```
                              (2,2)
                            /   |   \
                       (2,1)   (1,2)   (1,1)
                      / | \     / | \    / | \
                (2,0)(1,1)(1,0) ... etc.
```

Many subtrees are identical and are recomputed multiple times.

**Memoization** prunes the tree: once a node is computed, its value is stored and subsequent calls to that node do not recurse further; they simply return the stored value. The recursion tree becomes a directed acyclic graph (DAG) where each node is visited only once.

```
                              (2,2)
                            /   |   \
                       (2,1)   (1,2)   (1,1)
                      / | \     / | \    (already done, just use stored)
                (2,0)(1,1)(1,0) ... 
                (computed) (stored) (computed)
```

The total number of distinct nodes is `(m+1)*(n+1)`.

---

## 6. Complexity Analysis

### Time Complexity

Each cell `(i, j)` (with `0 ≤ i ≤ m`, `0 ≤ j ≤ n`) is computed at most once. Inside the computation, we make three recursive calls that each either return from memo or compute new cells. The work per cell is constant (a few comparisons and additions). Thus the total time is `O(m * n)`.

### Space Complexity

- **Memo table**: `O(m * n)` to store results for all cells.
- **Recursion stack**: In the worst case (going straight left or up), the depth can be `m + n`, so `O(m + n)` stack space.
- Overall: `O(m * n)` space.

If the grid is large, we could reduce the memo to only needed rows, but the top‑down approach naturally uses the full table.

---

## 7. Advantages and Disadvantages

### Advantages

- **Efficiency**: Reduces exponential time to polynomial (linear in number of cells).
- **Preserves recursive clarity**: The code still directly reflects the recurrence, making it easy to understand and modify.
- **Only computes needed subproblems**: In some problems, not all subproblems are needed; top‑down avoids computing unnecessary ones. Here, with the recurrence, all are needed anyway, but this is still a benefit in other problems.

### Disadvantages

- **Recursion overhead**: Function call overhead may be higher than an iterative solution.
- **Stack depth**: For very large grids (e.g., 1000×1000), recursion depth can exceed Python's default recursion limit (`m+n` could be 2000, which is near the limit). This may require increasing the limit or switching to bottom‑up.
- **Space**: Stores the entire memo table, which might be large for huge grids. Bottom‑up can be optimized to use `O(n)` space.

---

## 8. Mental Model – How Memoization Works

Imagine you are asked to find the cost to a certain cell. Instead of solving the problem from scratch every time, you keep a notebook (`memo`) where you write down the answer for each cell the first time you figure it out. Later, when someone asks for the same cell, you just look it up in your notebook and give the answer instantly.

In the recursion:
- When you are asked for a cell, you first check the notebook. If it's there, you're done.
- If not, you ask the three predecessor cells (left, top, diagonal) for their answers (they will also use the notebook). After you receive those answers, you compute the answer for the current cell, write it in the notebook, and return it.

This way, the notebook ensures that no cell's cost is computed more than once. The recursion still happens, but the total number of cell calculations is limited to the number of cells in the grid.

---

## 9. Comparison with Naïve Recursion

| Feature               | Naïve Recursion                       | Memoization (Top‑Down DP)            |
|-----------------------|---------------------------------------|--------------------------------------|
| **Time**              | Exponential O(3^(m+n))                | Polynomial O(m × n)                  |
| **Space**             | O(m+n) (stack)                        | O(m × n) (memo) + O(m+n) stack      |
| **Repeated work**     | Yes, many times                       | No, each cell solved once            |
| **Usability**         | Only for very small grids (≤8×8)      | Handles grids up to thousands of cells (within recursion limit) |

---

## 10. Conclusion

The memoization approach elegantly solves the minimum cost path problem by storing intermediate results, turning an exponential recursion into a linear‑time dynamic programming solution. It preserves the intuitive recursive structure while achieving dramatic performance gains. The key is the use of a memo table to cache results, ensuring each cell is computed only once. For large grids, one might prefer the bottom‑up iterative version to avoid recursion depth limits and to further optimise memory, but memoization remains a powerful and easy‑to‑implement technique for many DP problems.