# House Robber: Iterative Dynamic Programming Solution

## Table of Contents
1. [Problem Definition / Purpose](#problem-definition--purpose)
2. [Full Implementation](#full-implementation)
3. [Code Breakdown](#code-breakdown)
4. [Core Conceptual Model](#core-conceptual-model)
5. [Algorithm Design](#algorithm-design)
6. [Execution Flow](#execution-flow)
7. [Recursion Tree](#recursion-tree)
8. [Complexity Analysis](#complexity-analysis)
9. [Example Usage](#example-usage)
10. [Expected Output](#expected-output)
11. [Edge Cases and Constraints](#edge-cases-and-constraints)
12. [Key Observations](#key-observations)

---

## Problem Definition / Purpose
The function `rob(nums)` calculates the maximum sum of money that can be robbed from a row of houses, with the constraint that no two adjacent houses can be robbed (the alarm triggers if adjacent houses are robbed). The input is a list `nums` where `nums[i]` represents the amount of money in house `i`. The function returns the maximum achievable loot.

---

## Full Implementation

```python
def rob(nums):
    """
    Determine the maximum amount of money that can be robbed from a row of houses
    without robbing two adjacent houses.

    Parameters:
    nums (List[int]): List of non‑negative integers representing money in each house.

    Returns:
    int: Maximum total loot possible.
    """
    # Edge case: no houses → nothing to rob.
    if not nums:
        return 0

    # Edge case: only one house → rob it.
    if len(nums) == 1:
        return nums[0]

    # DP state variables:
    # max_loot_up_to_previous_house = max loot considering houses up to index i-2
    # max_loot_up_to_current_house  = max loot considering houses up to index i-1
    max_loot_up_to_previous_house = 0
    max_loot_up_to_current_house  = 0

    # Iterate through each house, updating the two DP variables.
    for current_house_money in nums:
        # Two choices at current house:
        # 1. Skip it → loot remains max_loot_up_to_current_house.
        # 2. Rob it → loot = max_loot_up_to_previous_house + current_house_money.
        # Take the maximum of the two.
        new_max_loot = max(
            max_loot_up_to_current_house,                     # skip current
            max_loot_up_to_previous_house + current_house_money   # rob current
        )

        # Shift the window: the house that was current becomes the new previous,
        # and the newly computed value becomes the current for the next iteration.
        max_loot_up_to_previous_house = max_loot_up_to_current_house
        max_loot_up_to_current_house = new_max_loot

    # After processing all houses, max_loot_up_to_current_house holds the answer.
    return max_loot_up_to_current_house
```

---

## Code Breakdown

### Edge Cases
```python
if not nums:
    return 0
```
- **What**: If the input list is empty, return 0.
- **Why**: No houses means no money can be robbed.

```python
if len(nums) == 1:
    return nums[0]
```
- **What**: If there is only one house, return its value.
- **Why**: The only option is to rob that house.

### DP Variable Initialization
```python
max_loot_up_to_previous_house = 0
max_loot_up_to_current_house  = 0
```
- **What**: Two variables hold the maximum loot achievable up to the house two steps behind (`previous`) and the house one step behind (`current`), relative to the house being considered.
- **Why**: At the start, before any house is processed, the loot up to index `-2` (nonexistent) is 0, and up to index `-1` (also nonexistent) is 0. These serve as the base for the recurrence.

### Iteration Loop
```python
for current_house_money in nums:
    new_max_loot = max(
        max_loot_up_to_current_house,
        max_loot_up_to_previous_house + current_house_money
    )
    max_loot_up_to_previous_house = max_loot_up_to_current_house
    max_loot_up_to_current_house = new_max_loot
```
- **What**: For each house, compute the best loot up to that house by considering two choices: skip it (loot remains what it was before) or rob it (loot = best loot up to two houses ago + current money). Then update the two variables to move the window forward.
- **Why**: This implements the DP recurrence:  
  `dp[i] = max(dp[i-1], dp[i-2] + nums[i])` for `i >= 0` (with `dp[-1]=0`). Instead of storing all `dp` values, we keep only the last two.

### Return
```python
return max_loot_up_to_current_house
```
- **What**: After the loop, `max_loot_up_to_current_house` holds the maximum loot considering all houses.
- **Why**: By definition, it is `dp[len(nums)-1]`.

---

## Core Conceptual Model

### State Representation
Let `dp[i]` be the maximum loot that can be obtained from the first `i+1` houses (indices `0..i`). The recurrence is:

```
dp[i] = max(dp[i-1], dp[i-2] + nums[i])   for i ≥ 0
```

with base cases:
- `dp[-1] = 0` (no houses considered)
- `dp[-2] = 0` (negative indices are treated as 0)

### Mental Model: Two‑State Sliding Window
Think of walking through the houses one by one. At each step, you maintain two pieces of information:
- **Prev**: the maximum loot if you stop at the house before the last one (i.e., considering houses up to `i-2`).
- **Curr**: the maximum loot if you stop at the last house (i.e., considering houses up to `i-1`).

When you arrive at the current house, you have two choices:
- Skip it → your loot is `Curr` (the best you had before).
- Rob it → your loot is `Prev + currentMoney` (because you must skip the adjacent house).

The new maximum loot for houses up to the current house is the larger of these two. Then you shift: the previous `Curr` becomes the new `Prev`, and the new computed value becomes the new `Curr`.

This mimics the recurrence without needing an array. The intuition is that you only need to remember the best outcomes for the last two “states” (skip last house vs. rob last house, but the DP combines them into a single value per index).

### Why This Works
The optimal decision for each house depends only on the optimal decisions for the previous one or two houses (optimal substructure). The problem has overlapping subproblems (e.g., computing the best for house `i` requires the best for `i-1` and `i-2`), which is why DP is appropriate. The iterative two‑variable approach eliminates redundant calculations and uses constant space.

---

## Algorithm Design

### Approach: Iterative Dynamic Programming (Space‑Optimized)
- **Rationale**: The problem exhibits optimal substructure and overlapping subproblems. A full DP table would be O(n) space, but because the recurrence only depends on the last two values, we can reduce space to O(1).

### Step‑by‑Step Algorithm
1. If `nums` is empty, return 0.
2. If `len(nums) == 1`, return `nums[0]`.
3. Initialize `prev = 0`, `curr = 0`.
4. For each `money` in `nums`:
   - `new = max(curr, prev + money)`
   - `prev = curr`
   - `curr = new`
5. Return `curr`.

### Pseudo‑Flow
```
if nums empty → return 0
if one element → return that element
prev = 0
curr = 0
for money in nums:
    next = max(curr, prev + money)
    prev = curr
    curr = next
return curr
```

---

## Execution Flow

### ASCII Representation of the Process (nums = [2,7,9,3,1])
```
Start
  |
  v
nums = [2,7,9,3,1]
  |
  v
Edge cases? Not empty, len > 1 → continue
  |
  v
Initialize: prev = 0, curr = 0
  |
  v
For each money in nums:
  |
  +-- money = 2
  |   next = max(curr, prev + 2) = max(0, 0+2) = 2
  |   prev = curr = 0
  |   curr = next = 2
  |
  +-- money = 7
  |   next = max(curr, prev + 7) = max(2, 0+7) = 7
  |   prev = curr = 2
  |   curr = next = 7
  |
  +-- money = 9
  |   next = max(curr, prev + 9) = max(7, 2+9) = 11
  |   prev = curr = 7
  |   curr = next = 11
  |
  +-- money = 3
  |   next = max(curr, prev + 3) = max(11, 7+3) = 11
  |   prev = curr = 11
  |   curr = next = 11
  |
  +-- money = 1
      next = max(curr, prev + 1) = max(11, 11+1) = 12
      prev = curr = 11
      curr = next = 12
  |
  v
Return curr = 12
  |
  v
End
```

### State Transitions Over Iterations
| i | nums[i] | prev (dp[i-2]) | curr (dp[i-1]) | next (dp[i]) |
|---|---------|----------------|----------------|--------------|
| 0 | 2       | 0              | 0              | 2            |
| 1 | 7       | 0              | 2              | 7            |
| 2 | 9       | 2              | 7              | 11           |
| 3 | 3       | 7              | 11             | 11           |
| 4 | 1       | 11             | 11             | 12           |

---

## Recursion Tree

Even though the implementation is iterative, the underlying recursive formulation with overlapping subproblems is:

```
rob(nums, i) = max(rob(nums, i-1), rob(nums, i-2) + nums[i])
```

For `nums = [2,7,9,3,1]`, a partial recursion tree (showing overlapping subproblems) would be:

```
                         rob(4)
                     /            \
               rob(3)               rob(2)+1
             /       \            /       \
       rob(2)         rob(1)+3   rob(1)   rob(0)+9
      /    \         /    \      /   \      /    \
 rob(1) rob(0)+9  rob(0) rob(-1)+7 ...   ...   ...
```

Many calls like `rob(1)` and `rob(0)` are repeated. The iterative DP computes each subproblem once, avoiding this exponential blowup.

---

## Complexity Analysis

### Time Complexity
- **O(n)**: The algorithm iterates through each house exactly once, performing a constant number of operations per house (comparison and addition). Therefore, time scales linearly with the number of houses.

### Space Complexity
- **O(1)**: Only two integer variables (`prev` and `curr`) are used, regardless of the input size. No additional data structures that grow with `n` are allocated.

### Why These Complexities
- The DP recurrence forces at least one pass over all houses to compute the result. The space‑optimized version avoids storing the full `dp` array, achieving constant space.

---

## Example Usage

```python
# Example 1: Four houses
print(rob([1,2,3,1]))   # Output: 4

# Example 2: Five houses
print(rob([2,7,9,3,1])) # Output: 12

# Example 3: Single house
print(rob([5]))         # Output: 5
```

---

## Expected Output

### Step‑by‑Step Derivation for `rob([1,2,3,1])`
- `i=0`, money=1: `next = max(0, 0+1) = 1` → `prev=0`, `curr=1`
- `i=1`, money=2: `next = max(1, 0+2) = 2` → `prev=1`, `curr=2`
- `i=2`, money=3: `next = max(2, 1+3) = 4` → `prev=2`, `curr=4`
- `i=3`, money=1: `next = max(4, 2+1) = 4` → `prev=4`, `curr=4`
- Result = 4

### Step‑by‑Step Derivation for `rob([2,7,9,3,1])`
- As shown in the Execution Flow, the result is 12.

---

## Edge Cases and Constraints

### Boundary Inputs
- **Empty list**: Returns 0.
- **Single house**: Returns its value.
- **Two houses**: Returns the larger of the two (since robbing both is prohibited). Example: `[5,3] → 5`.
- **All houses have 0**: Returns 0.

### Invalid Inputs
- The problem assumes `nums` contains non‑negative integers. If negative numbers were present, the DP recurrence `max(dp[i-1], dp[i-2] + nums[i])` would still work, but the problem typically expects non‑negative. The function does not explicitly validate this; it’s the caller’s responsibility.

### Performance Limits
- The algorithm handles very large lists efficiently (O(n) time, O(1) space). For n up to millions, it will run quickly, but the output can be large (up to sum of values), which Python can handle.

---

## Key Observations
- This problem is a classic example of a **1‑D DP with a space optimization** that uses only two variables.
- The recurrence can be interpreted as a **decision‑making process** at each step: either skip the current house (keeping the previous best) or rob it (adding its value to the best two steps ago).
- The solution is **order‑dependent**: houses are processed sequentially, and the adjacency constraint is local.
- The problem can also be solved using a **full DP array** for clarity, but the two‑variable version is more memory‑efficient.
- The iterative approach is essentially a **linear scan** with a rolling update, which is both intuitive and fast.