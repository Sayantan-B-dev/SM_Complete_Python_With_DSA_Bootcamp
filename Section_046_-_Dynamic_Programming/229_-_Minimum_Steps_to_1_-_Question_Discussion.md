# Minimum Steps to 1: Dynamic Programming Approaches

## Problem Statement

Given a positive integer `n`, the goal is to reduce it to `1` using the following allowed operations:

1. Subtract 1.
2. Divide by 2 (only if `n` is divisible by 2).
3. Divide by 3 (only if `n` is divisible by 3).

Each operation counts as one step. The task is to find the **minimum number of steps** required.

---

## Optimal Substructure

Let `dp[i]` denote the minimum number of steps needed to reduce `i` to `1`. The recurrence relation is:

- **Base case**: `dp[1] = 0` (already at 1).
- For `i > 1`:
  ```
  dp[i] = 1 + min(
      dp[i-1],
      dp[i//2] if i%2 == 0,
      dp[i//3] if i%3 == 0
  )
  ```
  The `+1` accounts for the operation taken to reach the smaller number.

This problem exhibits **overlapping subproblems** because the same smaller numbers are reached via different paths. Dynamic programming solves it efficiently.

---

## 1. Top‑Down Approach (Memoization)

### Algorithm

1. Create a memo array `memo` of size `n+1`, initialized with a sentinel value (e.g., `-1`) to indicate uncomputed states.
2. Define a recursive function `solve(x)`:
   - **Base case**: If `x == 1`, return `0`.
   - If `memo[x]` is not the sentinel, return the stored value.
   - Compute the result by recursively evaluating the three possible moves:
     - `steps = solve(x-1)`
     - If `x%2 == 0`, `steps = min(steps, solve(x//2))`
     - If `x%3 == 0`, `steps = min(steps, solve(x//3))`
   - Store `ans = 1 + steps` in `memo[x]`.
   - Return `ans`.
3. The answer for the original `n` is `solve(n)`.

### Characteristics

- **Time Complexity**: `O(n)`. Each number from `1` to `n` is computed at most once.
- **Space Complexity**: `O(n)` for the memo array plus `O(n)` recursion stack depth (worst case when repeatedly subtracting 1).
- **Pros**: Only computes states that are actually reachable from `n`; natural for problems with a recursive structure.
- **Cons**: Recursion overhead; may hit Python recursion depth limit for very large `n`.

---

## 2. Bottom‑Up Approach (Tabulation)

### Algorithm

1. Create a DP array `dp` of length `n+1` (indices `0..n`). Index `0` is unused (or can be treated as infinite steps).
2. Initialize `dp[1] = 0`.
3. Iterate `i` from `2` to `n`:
   - Start with `best = dp[i-1] + 1`.
   - If `i % 2 == 0`, update `best = min(best, dp[i//2] + 1)`.
   - If `i % 3 == 0`, update `best = min(best, dp[i//3] + 1)`.
   - Set `dp[i] = best`.
4. Return `dp[n]`.

### Characteristics

- **Time Complexity**: `O(n)`. A single loop processes each number once.
- **Space Complexity**: `O(n)` for the DP table.
- **Pros**: No recursion; avoids function call overhead; can be easily optimized to use only a few variables (space `O(1)`) if the recurrence only depends on earlier values (though here dependencies are not strictly sequential because division jumps, so a full table is needed unless the order is processed carefully).
- **Cons**: Computes `dp[i]` for every `i` from `2` to `n`, even if some values are never needed for the final answer. For this problem, all values up to `n` are needed anyway because subtraction reaches every integer.

---

## Comparison of the Two DP Approaches

| Aspect               | Top‑Down (Memoization)            | Bottom‑Up (Tabulation)             |
|----------------------|-----------------------------------|------------------------------------|
| **Implementation**   | Recursive with caching            | Iterative with loop                |
| **Order**            | Starts from `n`, goes down        | Starts from base case, builds up   |
| **Subproblem Computation** | Only those reachable from `n` | All from `2` to `n`                |
| **Stack Usage**      | `O(n)` depth (risk of recursion limit) | No recursion, constant stack       |
| **Memory**           | `O(n)` memo + stack overhead      | `O(n)` DP array                     |
| **Speed**            | Slightly slower due to recursion calls | Faster due to simple loop          |

Both achieve the same asymptotic complexity. The choice often depends on personal preference, recursion limits, and whether the problem naturally lends itself to recursion or iteration.

---

## Example Walkthrough (n = 7)

**Top‑down reasoning** (without code):

- To compute steps(7):
  - Options: subtract → steps(6); divide by 2 not possible; divide by 3 not possible.
  - So steps(7) = 1 + steps(6).
- steps(6):
  - Subtract → steps(5); divide by 2 → steps(3); divide by 3 → steps(2).
  - Compute steps(5): subtract → steps(4); divide by 2 → steps(2) (since 5 not divisible by 3).
  - steps(4): subtract → steps(3); divide by 2 → steps(2).
  - steps(3): subtract → steps(2); divide by 3 → steps(1) = 0.
  - steps(2): subtract → steps(1) = 0; divide by 2 → steps(1) = 0.
- Working upwards yields the minimal steps:  
  steps(1)=0  
  steps(2)=1 (subtract)  
  steps(3)=1 (divide by 3)  
  steps(4)=2 (divide by 2 → steps(2) +1 = 2)  
  steps(5)=3 (subtract → steps(4) +1 = 3)  
  steps(6)=2 (divide by 3 → steps(2) +1 = 2)  
  steps(7)=3 (subtract → steps(6) +1 = 3)  
  So answer = 3.

**Bottom‑up** would compute the same values in order from 2 to 7, building the table.

---

## Summary

The “minimum steps to 1” problem is a classic example of a dynamic programming problem with overlapping subproblems. Both memoization (top‑down) and tabulation (bottom‑up) provide efficient `O(n)` solutions, transforming an exponential recursive brute force into a linear‑time algorithm. The choice between them depends on the problem’s constraints and the programmer’s style, but understanding both is essential for mastering DP.