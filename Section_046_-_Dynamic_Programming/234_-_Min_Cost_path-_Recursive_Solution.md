```py
def min_cost_path_recursive(cost, m, n):
    # Base Case
    if ( m==0 and n==0):
        return cost[0][0]

    if(m<0 or n<0):
        return float('inf')
    
    # Recursive Calls
    cost_left = min_cost_path_recursive(cost,m,n-1)
    cost_top = min_cost_path_recursive(cost,m-1,n)
    cost_diag = min_cost_path_recursive(cost,m-1,n-1)
    # our Work

    ans = cost[m][n] + min(cost_left,cost_diag,cost_top)

    return ans
```
# Minimum Cost Path – Recursive Solution (Naïve)

## 1. Problem Restatement

We are given a 2‑D matrix `cost` where each cell contains a positive integer. Starting at the top‑left cell `(0,0)`, we want to reach a target cell `(m, n)` by moving only **right**, **down**, or **diagonally down‑right**. The cost of a path is the sum of the values of all visited cells (including start and end). We need to find the **minimum possible cost**.

The recursive function `min_cost_path_recursive(cost, m, n)` given in the code attempts to compute this minimum cost using a straightforward recursion, without any memoization.

---

## 2. Code with Line‑by‑Line Explanation

```python
def min_cost_path_recursive(cost, m, n):
    # Base Case
    if ( m==0 and n==0):
        return cost[0][0]

    if(m<0 or n<0):
        return float('inf')
    
    # Recursive Calls
    cost_left = min_cost_path_recursive(cost,m,n-1)
    cost_top = min_cost_path_recursive(cost,m-1,n)
    cost_diag = min_cost_path_recursive(cost,m-1,n-1)
    
    # our Work
    ans = cost[m][n] + min(cost_left, cost_diag, cost_top)

    return ans
```

### Line‑by‑Line Explanation

- **Line 1**: Function definition – takes the cost matrix, and the target coordinates `(m, n)`.
- **Line 3‑4**: First base case: if both `m` and `n` are zero, we are at the starting cell. The cost to reach `(0,0)` is simply `cost[0][0]`. Return that.
- **Line 6‑7**: Second base case: if either index is negative, this path is invalid. Return `infinity` (a large number) to indicate that this path cannot be taken (so it will never be chosen as the minimum).
- **Line 10‑12**: Recursive calls:
  - `cost_left`: compute the minimum cost to reach `(m, n-1)` – that is, the cell immediately to the left of the target. A move from there to `(m, n)` would be a **right** move.
  - `cost_top`: compute the minimum cost to reach `(m-1, n)` – the cell above. A move from there to `(m, n)` would be a **down** move.
  - `cost_diag`: compute the minimum cost to reach `(m-1, n-1)` – the cell diagonally up‑left. A move from there to `(m, n)` would be a **diagonal** move.
- **Line 15**: The cost to reach `(m, n)` is the cost of the cell itself plus the minimum among the three possible predecessor costs (because we can come from any of those three directions, and we want the cheapest overall path).
- **Line 17**: Return the computed answer.

---

## 3. Algorithm Description (Conceptual)

The function implements the **recurrence relation** that defines the minimum cost to reach a cell:

```
minCost(i, j) = cost[i][j] + min(
    minCost(i, j-1),   // from left
    minCost(i-1, j),   // from top
    minCost(i-1, j-1)  // from diagonal
)
```

with the base condition `minCost(0,0) = cost[0][0]` and any cell with negative indices has infinite cost (invalid path).

The function works by **splitting the problem** into smaller subproblems: to find the minimum cost to `(m, n)`, we first need the minimum costs to its three possible predecessors. Those predecessors are computed recursively. The recursion continues until it hits the base case `(0,0)` or an invalid cell.

---

## 4. Step‑by‑Step Example with Visualization

Let's use a small 3×3 cost matrix:

```
cost = [
    [1, 2, 3],
    [4, 8, 2],
    [1, 5, 3]
]
```

We want to find the minimum cost to reach `(2,2)`. We'll trace the recursion tree for `min_cost_path_recursive(cost, 2, 2)`.

### Step 1: Call `(2,2)`

- Not base case (both positive).
- Compute:
  - `cost_left = min_cost_path_recursive(2,1)`
  - `cost_top = min_cost_path_recursive(1,2)`
  - `cost_diag = min_cost_path_recursive(1,1)`

We'll expand each.

### Step 2: Expand `(2,1)`

```
(2,1) → not base
    cost_left = (2,0)
    cost_top = (1,1)
    cost_diag = (1,0)
```

### Step 3: Expand `(2,0)`

```
(2,0) → m>0, n=0
    cost_left = (2,-1) → invalid → inf
    cost_top = (1,0)
    cost_diag = (1,-1) → invalid → inf
```

Now we need `(1,0)`.

### Step 4: Expand `(1,0)`

```
(1,0) → m>0, n=0
    cost_left = (1,-1) → inf
    cost_top = (0,0)
    cost_diag = (0,-1) → inf
```

`(0,0)` returns 1.

Thus:
- `cost_top` for `(1,0)` = 1
- So `(1,0)` returns: `cost[1][0] + min(inf, 1, inf) = 4 + 1 = 5`

Back to `(2,0)`:
- `cost_top = (1,0) = 5`
- `(2,0)` returns: `cost[2][0] + min(inf, 5, inf) = 1 + 5 = 6`

Now back to `(2,1)`: we have `(2,0)=6`. We also need `(1,1)` and `(1,0)`.

### Step 5: Expand `(1,1)`

```
(1,1) → m>0, n>0
    cost_left = (1,0) = 5 (already computed)
    cost_top = (0,1)
    cost_diag = (0,0) = 1
```

We need `(0,1)`.

### Step 6: Expand `(0,1)`

```
(0,1) → m=0, n>0
    cost_left = (0,0) = 1
    cost_top = (-1,1) → inf
    cost_diag = (-1,0) → inf
```

Thus `(0,1)` returns: `cost[0][1] + min(1, inf, inf) = 2 + 1 = 3`

Now back to `(1,1)`:
- `cost_left = 5`
- `cost_top = 3`
- `cost_diag = 1`
- Minimum = 1
- `(1,1)` returns: `cost[1][1] + 1 = 8 + 1 = 9`

### Step 7: Expand `(1,0)` we already have = 5.

Now back to `(2,1)`:
- `cost_left = (2,0) = 6`
- `cost_top = (1,1) = 9`
- `cost_diag = (1,0) = 5`
- Minimum = 5
- `(2,1)` returns: `cost[2][1] + 5 = 5 + 5 = 10`

So `(2,1) = 10`.

### Step 8: Expand `(1,2)` (part of the original `(2,2)` call)

```
(1,2) → m>0, n>0
    cost_left = (1,1) = 9
    cost_top = (0,2)
    cost_diag = (0,1) = 3
```

Need `(0,2)`.

### Step 9: Expand `(0,2)`

```
(0,2) → m=0, n>0
    cost_left = (0,1) = 3
    cost_top = (-1,2) → inf
    cost_diag = (-1,1) → inf
```

Thus `(0,2)` returns: `cost[0][2] + 3 = 3 + 3 = 6`

Back to `(1,2)`:
- `cost_left = 9`
- `cost_top = 6`
- `cost_diag = 3`
- Minimum = 3
- `(1,2)` returns: `cost[1][2] + 3 = 2 + 3 = 5`

### Step 10: Expand `(1,1)` already computed = 9.

Now back to `(2,2)`:
- `cost_left = (2,1) = 10`
- `cost_top = (1,2) = 5`
- `cost_diag = (1,1) = 9`
- Minimum = 5
- `(2,2)` returns: `cost[2][2] + 5 = 3 + 5 = 8`

Thus the final answer is **8**.

### Recursion Tree (simplified)

```
                                    (2,2)
                                   /  |  \
                                  /   |   \
                             (2,1)   (1,2)  (1,1)
                             /|\      /|\     /|\
                            … … …    … … …   … … …
```

Every node splits into three children, leading to an exponential explosion. The tree above already shows repeated subproblems (e.g., `(1,1)` is computed multiple times).

---

## 5. Complexity Analysis

### Time Complexity

Each call to `(m, n)` spawns three recursive calls: one to `(m, n-1)`, one to `(m-1, n)`, and one to `(m-1, n-1)`. This leads to a recurrence:

```
T(m, n) = T(m, n-1) + T(m-1, n) + T(m-1, n-1) + O(1)
```

The number of distinct subproblems is `O(m * n)`. However, because the recursion does not store results, the same subproblem is solved many times. The total number of calls is **exponential**. In fact, for an `m × n` grid, the worst‑case time is `O(3^(m+n))` – because at each step we branch up to three ways. For a square grid of size `N×N`, this is roughly `O(3^(2N)) = O(9^N)`, which is even worse than `2^(m+n)`. This is completely impractical for any moderately sized grid.

### Space Complexity

The recursion depth is at most `m + n` (if we always go left/up, we move one step at a time). Therefore the maximum stack size is `O(m + n)`. However, the number of simultaneous frames is limited by depth, not by total calls. So space complexity is `O(m + n)`.

---

## 6. Why This Recursive Approach is Inefficient

The problem exhibits **overlapping subproblems**: the same cell `(i, j)` is needed by many different paths. For example, in the 3×3 grid, `(1,1)` was computed multiple times (once from `(2,1)`, once from `(1,2)`, once from `(2,2)`, and also from deeper paths). The naive recursion recomputes it every time, leading to exponential growth.

This is exactly the kind of problem where **dynamic programming** (memoization or tabulation) shines. By storing results for each cell, we can reduce the time to `O(m * n)`.

---

## 7. Mental Model to Understand the Recursion

Think of the function as a **question**: "What is the minimum cost to get to `(m, n)`?" To answer, you ask three smaller questions:

- "What is the min cost to get to the cell **left** of me?" – because I can arrive from there by moving right.
- "What is the min cost to get to the cell **above** me?" – because I can arrive from there by moving down.
- "What is the min cost to get to the cell **diagonally above‑left**?" – because I can arrive from there by moving diagonally.

If you are at the start `(0,0)`, the answer is simply the cost of that cell. If you try to go outside the grid (negative indices), the answer is impossible (infinite cost). The final answer for `(m, n)` is its own cost plus the cheapest of those three possible ways to reach it.

This is a **divide and conquer** approach: break the problem into smaller instances of the same type, solve them recursively, and combine the results. But because the subproblems overlap, the naive combination leads to repeated work.

---

## 8. Conclusion

The recursive function `min_cost_path_recursive` correctly implements the recurrence for the minimum cost path problem. It is a direct translation of the mathematical definition and is easy to understand. However, its exponential time complexity makes it unusable for grids larger than about 10×10. For practical applications, we must use dynamic programming (either memoization or tabulation) to store intermediate results and achieve polynomial time `O(m × n)`. Nevertheless, the recursive version serves as a valuable stepping stone to understand the problem's structure before applying optimization.