# Climbing Stairs: Iterative Dynamic Programming Solution

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
The function `climbStairs(n)` computes the number of distinct ways to reach the top of a staircase with `n` steps, given that each move can be either 1 step or 2 steps. This is a classic combinatorial problem whose solution follows the Fibonacci sequence. The purpose of the implementation is to provide an efficient, iterative solution using constant space, avoiding the exponential overhead of a naive recursive approach.

---

## Full Implementation

```python
def climbStairs(n):
    """
    Calculate the number of distinct ways to climb a staircase with n steps,
    where each step can be either 1 or 2 stairs at a time.

    Parameters:
    n (int): Total number of steps to reach the top.

    Returns:
    int: Number of distinct ways to climb to the top.
    """
    # Base cases: with 0 or 1 steps, there is exactly 1 way.
    # For 0 steps, the only way is to do nothing.
    # For 1 step, the only way is a single 1‑step move.
    if n == 0 or n == 1:
        return 1

    # Initialize two variables to represent the number of ways to reach the
    # previous two steps. This mimics the Fibonacci recurrence:
    # ways(i) = ways(i-1) + ways(i-2)
    # Start with:
    # ways_to_previous_step = ways(0) = 1
    # ways_to_current_step  = ways(1) = 1
    ways_to_previous_step = 1   # ways to reach step 0
    ways_to_current_step  = 1   # ways to reach step 1

    # Iterate from step 2 up to n, computing ways for each step.
    # The variable 'step_index' represents the current step being computed.
    for step_index in range(2, n + 1):
        # The number of ways to reach the current step is the sum of ways to
        # reach the two preceding steps (since from those you can take a 1‑step
        # or 2‑step move to land exactly on the current step).
        current_ways = ways_to_previous_step + ways_to_current_step

        # Shift the window: the step that was current becomes the new previous,
        # and the newly computed value becomes the current for the next iteration.
        ways_to_previous_step = ways_to_current_step
        ways_to_current_step = current_ways

    # After the loop, ways_to_current_step holds ways(n).
    return ways_to_current_step
```

---

## Code Breakdown

### Base Cases
```python
if n == 0 or n == 1:
    return 1
```
- **What**: If `n` is 0 or 1, the function returns 1.
- **Why**: By definition, there is exactly one way to climb 0 steps (do nothing) and one way to climb 1 step (a single 1‑step move). These base cases also serve as the initial seeds for the recurrence relation.

### Initialization
```python
ways_to_previous_step = 1   # ways to reach step 0
ways_to_current_step  = 1   # ways to reach step 1
```
- **What**: Two variables store the number of ways for the two most recent steps.
- **Why**: The recurrence `ways(i) = ways(i-1) + ways(i-2)` requires knowledge of the two previous values. Starting with `i=2`, we need `ways(0)` and `ways(1)`.

### Iterative Computation
```python
for step_index in range(2, n + 1):
    current_ways = ways_to_previous_step + ways_to_current_step
    ways_to_previous_step = ways_to_current_step
    ways_to_current_step = current_ways
```
- **What**: A loop computes the number of ways for each step from 2 up to `n`.
- **Why**: Instead of storing all values in an array, we only keep the last two values, updating them in a rolling fashion. This yields the Fibonacci sequence and uses O(1) space.

### Return
```python
return ways_to_current_step
```
- **What**: After the loop, `ways_to_current_step` holds the computed value for step `n`.
- **Why**: This matches the required output.

---

## Core Conceptual Model

### State Representation
The problem can be modeled as a sequence of states, where each state `i` (0 ≤ i ≤ n) represents the number of distinct ways to reach step `i`. The transition from one state to the next is governed by the allowed moves: from step `i`, you can go to `i+1` or `i+2`. Reversing the perspective, to reach step `i`, you must have come from either `i-1` (by taking 1 step) or `i-2` (by taking 2 steps). Therefore:

```
ways(i) = ways(i-1) + ways(i-2)   for i ≥ 2
```

### Mental Model: Sliding Window
Think of a window that slides over the sequence of step counts. At any moment, the window contains the two most recently computed values. The next value is their sum. After computing it, the window shifts to include the new value and drop the oldest one. This mirrors the Fibonacci sequence generation:

```
Step:       0   1   2   3   4   5   ...
Ways:       1   1   2   3   5   8   ...
```

The algorithm starts with `[1, 1]`, computes `2` → window becomes `[1, 2]`; then `3` → `[2, 3]`; then `5` → `[3, 5]`; and so on. The mental image is a compact, moving pair of numbers that always contains the last two computed results.

### Why This Works
The recurrence is derived from the combinatorial nature: any sequence of steps ending at `i` either ends with a 1‑step move from `i-1` or a 2‑step move from `i-2`. Because these two possibilities are disjoint and cover all possibilities, the total number of sequences is the sum.

---

## Algorithm Design

### Approach: Iterative Dynamic Programming (Fibonacci)
- **Rationale**: The problem exhibits optimal substructure (solution for `n` depends on solutions for `n-1` and `n-2`) and overlapping subproblems (the same sub‑problems are solved repeatedly in a naive recursion). Dynamic programming avoids recomputation by storing intermediate results. The iterative version with two variables is the most space‑efficient form of DP for this recurrence.

### Step‑by‑Step Algorithm
1. **Input validation (implicit)**: If `n` is 0 or 1, return 1.
2. **Initialize**: `prev = 1`, `curr = 1`.
3. **Iterate** from `i = 2` to `n`:
   - `next = prev + curr`
   - `prev = curr`
   - `curr = next`
4. **Return** `curr`.

### Pseudo‑Flow
```
if n <= 1: return 1
prev = 1
curr = 1
for i from 2 to n:
    next = prev + curr
    prev = curr
    curr = next
return curr
```

---

## Execution Flow

### ASCII Representation of the Process (n = 5)
```
Start
  |
  v
n = 5
  |
  v
Base case? n > 1 → continue
  |
  v
Initialize: prev = 1 (ways(0)), curr = 1 (ways(1))
  |
  v
For i = 2 to 5:
  |
  +-- i = 2
  |   next = prev + curr = 1 + 1 = 2
  |   prev = curr = 1
  |   curr = next = 2
  |
  +-- i = 3
  |   next = prev + curr = 1 + 2 = 3
  |   prev = curr = 2
  |   curr = next = 3
  |
  +-- i = 4
  |   next = prev + curr = 2 + 3 = 5
  |   prev = curr = 3
  |   curr = next = 5
  |
  +-- i = 5
      next = prev + curr = 3 + 5 = 8
      prev = curr = 5
      curr = next = 8
  |
  v
Return curr = 8
  |
  v
End
```

### Data Transformations Over Iterations
| i | prev (ways(i-2)) | curr (ways(i-1)) | next (ways(i)) |
|---|------------------|------------------|----------------|
| 2 | 1                | 1                | 2              |
| 3 | 1                | 2                | 3              |
| 4 | 2                | 3                | 5              |
| 5 | 3                | 5                | 8              |

---

## Recursion Tree

Even though the implemented solution is iterative, understanding the recursive structure reveals overlapping subproblems. The recursion tree for `climbStairs(4)` is shown below (assuming naive recursion without memoization):

```
                     climbStairs(4)
                    /             \
          climbStairs(3)          climbStairs(2)
          /          \            /         \
  climbStairs(2)  climbStairs(1) climbStairs(1) climbStairs(0)
   /       \          |             |             |
climbStairs(1) climbStairs(0)      1             1             1
   |             |
   1             1
```

- **Overlapping subproblems**: The calls to `climbStairs(2)`, `climbStairs(1)`, and `climbStairs(0)` appear multiple times. The iterative DP eliminates this redundancy by computing each value once.

---

## Complexity Analysis

### Time Complexity
- **O(n)**: The loop runs from 2 to `n`, performing a constant number of operations per iteration. Thus the time grows linearly with `n`.

### Space Complexity
- **O(1)**: Only three integer variables (`prev`, `curr`, `next`) are used regardless of `n`. No additional data structures scale with input size.

### Why These Complexities
- The recurrence `ways(i) = ways(i-1) + ways(i-2)` forces at least O(n) operations to compute `ways(n)` if we start from the base cases. The iterative approach achieves exactly that while using only constant space by discarding older results that are no longer needed.

---

## Example Usage

```python
# Example 1: 2 steps
print(climbStairs(2))   # Output: 2

# Example 2: 3 steps
print(climbStairs(3))   # Output: 3

# Example 3: 5 steps
print(climbStairs(5))   # Output: 8
```

---

## Expected Output

### Step‑by‑Step Derivation for `climbStairs(3)`
1. `n = 3` > 1, proceed.
2. Initialize `prev = 1` (ways(0)), `curr = 1` (ways(1)).
3. `i = 2`: `next = 1 + 1 = 2`, `prev = 1`, `curr = 2`. (ways(2) = 2)
4. `i = 3`: `next = 1 + 2 = 3`, `prev = 2`, `curr = 3`. (ways(3) = 3)
5. Return `curr = 3`.

Thus:
- `climbStairs(2) → 2`
- `climbStairs(3) → 3`

---

## Edge Cases and Constraints

### Boundary Inputs
- **n = 0**: Returns 1 (by base case). This is often defined as the number of ways to do nothing.
- **n = 1**: Returns 1.
- **n = 2**: Returns 2 (1+1, 2).
- **Large n**: The algorithm handles large `n` efficiently (O(n) time, O(1) space). However, the result grows exponentially; for `n` > 92, the integer may overflow in languages with fixed‑size integers, but Python handles arbitrary‑precision integers.

### Invalid Inputs
- The problem assumes `n` is a non‑negative integer. If negative input is passed, the base case condition `n == 0 or n == 1` will not capture it, and the loop `range(2, n+1)` will be empty if `n` is less than 2. For `n = -1`, the loop runs from 2 to 0 (empty), and the function would return `ways_to_current_step` which is initialized to 1 – that would be incorrect. Therefore, the function should be used only with `n ≥ 0`. Production code could include an explicit check for negative inputs.

### Performance Limits
- The iterative solution is suitable for any `n` up to millions, provided enough time (O(n)). For extremely large `n`, matrix exponentiation or closed‑form (Binet’s formula) could achieve O(log n) time, but the iterative method is simpler and sufficient for typical constraints (e.g., `n ≤ 45` in many coding problems).

---

## Key Observations
- The problem reduces to computing the `(n+1)`‑th Fibonacci number if we start with F(0) = 1, F(1) = 1 (or equivalently the `n`‑th Fibonacci number with F(1)=1, F(2)=2). This highlights the deep connection between combinatorial counting and the Fibonacci sequence.
- The iterative two‑variable approach is a classic example of space‑optimized dynamic programming, demonstrating that not all DP problems require a full table.
- Understanding the recurrence from first principles (combinatorial reasoning) provides a solid foundation for extending the solution to variants, such as allowing step sizes other than 1 and 2 (e.g., 1, 2, 3 steps would lead to a tribonacci‑like recurrence).