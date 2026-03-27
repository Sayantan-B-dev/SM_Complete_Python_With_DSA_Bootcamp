```py
def count_balanced_bts_memoization(h,memo):
    # Base Case
    if ( h==0 or h==1):
        return 1   # For 0, we will say that a single tree of value None is possible
    
    if(memo[h] != -1):
        return memo[h]

    h1 = count_balanced_bts_memoization(h-1,memo)
    h2 = count_balanced_bts_memoization(h-2,memo)
    memo[h-1] = h1 
    memo[h-2] = h2
    ans = h1*h1 + 2*h1*h2
    memo[h] = ans
    return ans


def count_balanced_bts_recursive(h):
    # Base Case
    if ( h==0 or h==1):
        return 1   # For 0, we will say that a single tree of value None is possible
    
    h1 = count_balanced_bts_recursive(h-1)
    h2 = count_balanced_bts_recursive(h-2)

    ans = h1*h1 + 2*h1*h2

    return ans

def count_balanced_bts_tabulation(n):
    if(n<=1):
        return 1

    dp = [0] * (n+1)

    dp[0] = 1
    dp[1] = 1

    for i in range(2,n+1):
        dp[i] = dp[i-1]*dp[i-1] + 2*dp[i-1]*dp[i-2]

    return dp[n]
h=5
memo = [-1] * (h+1)
print(count_balanced_bts_tabulation(4))
```

# Counting Balanced Binary Trees of Height `h`

## Recurrence Foundation

Let `count[h]` be the number of distinct balanced binary trees of height `h` (height defined as the number of nodes on the longest path from root to leaf; height 0 is the empty tree, height 1 is a single node).  
The recurrence is derived from the possible heights of the left and right subtrees of the root:

- Both subtrees must be balanced.
- The overall height is `h`, so at least one subtree must have height `h-1`. The other can be `h-1` or `h-2`.
- Cases:
  1. Both subtrees of height `h-1` → `count[h-1] * count[h-1]`
  2. Left subtree height `h-1`, right `h-2` → `count[h-1] * count[h-2]`
  3. Left `h-2`, right `h-1` → `count[h-2] * count[h-1]`

Summing:  
`count[h] = count[h-1]² + 2 * count[h-1] * count[h-2]`  
Base: `count[0] = 1` (empty tree), `count[1] = 1` (single node).

---

## 1. Naïve Recursive Function

### Code with Detailed Comments

```python
def count_balanced_bts_recursive(h):
    # Base cases: height 0 or 1 – only one tree each (empty or single node)
    if h == 0 or h == 1:
        return 1

    # Recursively compute the number of trees for heights h-1 and h-2
    h1 = count_balanced_bts_recursive(h - 1)  # number of trees of height h-1
    h2 = count_balanced_bts_recursive(h - 2)  # number of trees of height h-2

    # Apply the recurrence formula
    ans = h1 * h1 + 2 * h1 * h2
    return ans
```

### Algorithm Description

1. **Base case**: If `h` is 0 or 1, return 1.
2. **Recursive step**:  
   - Compute `count(h-1)` and `count(h-2)` by recursively calling the function.  
   - Compute the result using the recurrence: `ans = count(h-1)² + 2·count(h-1)·count(h-2)`.
3. Return `ans`.

### Step-by-Step Visualization (for h = 4)

We can simulate the recursion tree:

```
                       count(4)
                         |
          +--------------+--------------+
          |                             |
      count(3)                        count(2)
        |                               |
    +---+---+                       +---+---+
    |       |                       |       |
 count(2) count(1)                count(1) count(0)
    |       |                       |       |
 +---+---+  1                        1       1
 |       |
count(1) count(0)
   1       1
```

Each node recomputes the same sub‑problems multiple times. For `h=4`, `count(2)` is computed twice, `count(1)` many times, etc.

### Time Complexity

**Exponential**: `T(h) = T(h-1) + T(h-2) + O(1)`. This is the same recurrence as Fibonacci, so `T(h) = O(φʰ)` where φ ≈ 1.618. In practice, it’s roughly `O(1.618^h)`.

### Space Complexity

`O(h)` due to the recursion stack depth.

### Pros

- Extremely simple and directly mirrors the mathematical recurrence.
- Easy to understand for small `h`.

### Cons

- Computes the same subproblems many times, leading to exponential growth.
- Becomes unusable for `h` > 30 (values become huge quickly anyway, but the number of calls is astronomical).
- No caching, so each call repeats work.

### Theoretical Formula

Directly uses the recurrence:  
`count(h) = count(h-1)² + 2·count(h-1)·count(h-2)`.

---

## 2. Memoization (Top‑Down Dynamic Programming)

### Code with Detailed Comments

```python
def count_balanced_bts_memoization(h, memo):
    # Base cases: height 0 or 1 → return 1
    if h == 0 or h == 1:
        return 1

    # If already computed, return stored value
    if memo[h] != -1:
        return memo[h]

    # Recursively compute values for h-1 and h-2
    h1 = count_balanced_bts_memoization(h - 1, memo)
    h2 = count_balanced_bts_memoization(h - 2, memo)

    # Optional: store intermediate results (though they already get stored by their own calls)
    memo[h - 1] = h1
    memo[h - 2] = h2

    # Apply recurrence
    ans = h1 * h1 + 2 * h1 * h2

    # Store the result for this h
    memo[h] = ans
    return ans
```

### Algorithm Description

1. **Initialization**: Create a `memo` array of size `h+1` filled with a sentinel (e.g., `-1`) indicating uncomputed.
2. **Base cases**: `h <= 1` return 1.
3. **Memo check**: If `memo[h] != -1`, return it.
4. **Recursive computation**:
   - Compute `count(h-1)` and `count(h-2)` via the memoized function (they will be stored as side effects).
   - Optionally store `memo[h-1]` and `memo[h-2]` (though they are already stored by their own calls).
   - Compute the answer using the recurrence.
   - Store `ans` in `memo[h]` and return.

### Step-by-Step Visualization (for h = 4)

We fill the memo array as we compute. The recursion tree is pruned:

```
count(4) → check memo[4] == -1
    compute count(3) → check memo[3] == -1
        compute count(2) → check memo[2] == -1
            compute count(1) → base → return 1
            compute count(0) → base → return 1
            store memo[2]=1*1 + 2*1*1 = 3
            return 3
        compute count(1) → base → return 1
        store memo[3]=3*3 + 2*3*1 = 15
        return 15
    compute count(2) → memo[2] != -1 → return 3 directly
    store memo[4]=15*15 + 2*15*3 = 225 + 90 = 315
    return 315
```

Each height is computed exactly once.

### Time Complexity

`O(h)` because each distinct `h` from 0 to the input is processed once, and each step does constant work.

### Space Complexity

- `O(h)` for the memo array.
- `O(h)` recursion stack depth in worst case (when h > 1 and we keep subtracting 1).

### Pros

- Linear time, drastically faster than naïve recursion.
- Only computes necessary subproblems (but here all are needed anyway).
- Retains the recursive structure, which can be intuitive.

### Cons

- Still uses recursion, which may hit Python recursion limit for very large `h` (though `h` is typically limited by the huge values, not recursion depth; still, depth ~ h).
- Requires explicit memo management (global or passed).
- Slightly more code than tabulation.

---

## 3. Tabulation (Bottom‑Up Dynamic Programming)

### Code with Detailed Comments

```python
def count_balanced_bts_tabulation(n):
    # Base case: height 0 or 1 → return 1
    if n <= 1:
        return 1

    # DP table: dp[i] = number of balanced trees of height i
    dp = [0] * (n + 1)

    # Initialise base values
    dp[0] = 1   # height 0: empty tree
    dp[1] = 1   # height 1: single node

    # Build table from height 2 up to n
    for i in range(2, n + 1):
        # Apply recurrence: dp[i] = dp[i-1]² + 2*dp[i-1]*dp[i-2]
        dp[i] = dp[i-1] * dp[i-1] + 2 * dp[i-1] * dp[i-2]

    # Result for height n
    return dp[n]
```

### Algorithm Description

1. **Base check**: If `n <= 1`, return 1.
2. **Create DP array**: of length `n+1`, index `i` stores the count for height `i`.
3. **Initialize**: `dp[0] = 1`, `dp[1] = 1`.
4. **Iterate**: For `i` from 2 to `n`:
   - Compute `dp[i]` using the recurrence `dp[i-1]² + 2·dp[i-1]·dp[i-2]`.
5. **Return** `dp[n]`.

### Step-by-Step Visualization (DP Table Filling for n = 4)

| i  | dp[i] (calculation)                 | Result |
|----|--------------------------------------|--------|
| 0  | base                                 | 1      |
| 1  | base                                 | 1      |
| 2  | dp[1]² + 2·dp[1]·dp[0] = 1² + 2·1·1 | 3      |
| 3  | dp[2]² + 2·dp[2]·dp[1] = 3² + 2·3·1 | 9+6=15 |
| 4  | dp[3]² + 2·dp[3]·dp[2] = 15² + 2·15·3 | 225+90=315 |

### Time Complexity

`O(n)` – a single loop of `n-1` iterations, each doing a few multiplications.

### Space Complexity

- `O(n)` for the DP table.
- **Can be reduced to O(1)** because only the last two values are needed:
  ```python
  def count_balanced_bts_optimized(n):
      if n <= 1: return 1
      prev2, prev1 = 1, 1   # dp[0], dp[1]
      for i in range(2, n+1):
          current = prev1*prev1 + 2*prev1*prev2
          prev2, prev1 = prev1, current
      return prev1
  ```
  This uses constant space.

### Pros

- No recursion, so no stack overflow.
- Very fast; only a loop with simple arithmetic.
- Easy to understand and implement.
- Can be easily optimized for space.

### Cons

- Computes all intermediate values, even if only the final one is needed (but the recurrence forces that anyway).
- May be less intuitive than the recursive version for those who think recursively.

---

## Comparison of the Three Approaches

| Approach           | Time Complexity | Space Complexity | Recursion Depth | Suitable for Large h |
|--------------------|-----------------|------------------|-----------------|----------------------|
| Naïve Recursive    | O(φⁿ) ≈ O(1.618ⁿ) | O(h)            | O(h)            | No (exponential)     |
| Memoization        | O(h)            | O(h)            | O(h)            | Yes, but recursion depth may be an issue |
| Tabulation         | O(h)            | O(h) (or O(1) optimized) | None       | Yes, and space can be minimal |

All produce the same sequence: `h:0→1, 1→1, 2→3, 3→15, 4→315, 5→315? Actually 5: 315² + 2*315*15 = 99225 + 9450 = 108675`, etc.

---

## Preventing Overflow (Optional)

In languages with fixed-size integers (e.g., C++, Java), the numbers can grow extremely quickly. The values for `h=10` are already huge. To avoid overflow, the problem often asks to compute the answer modulo a large prime (e.g., `10⁹+7`). In Python, integers are arbitrarily large, so no mod is needed. If required, the recurrence would be adapted as:

```
dp[i] = (dp[i-1]*dp[i-1] + 2*dp[i-1]*dp[i-2]) % MOD
```

---

## Conclusion

The problem of counting balanced binary trees of a given height is a classic combinatorial DP. The recurrence is derived from the possible height configurations of the root's subtrees. Three implementations show the progression from a simple but exponential recursive solution to efficient linear-time DP using memoization or tabulation. The tabulation approach is usually preferred because it avoids recursion and can be space-optimized to O(1). Understanding these approaches is crucial for applying DP to counting problems.