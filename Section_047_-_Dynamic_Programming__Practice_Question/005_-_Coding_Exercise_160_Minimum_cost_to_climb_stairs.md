## Table of Contents

1.  Full Commented Code
2.  Problem Statement and Intuition
3.  Algorithm Explanation (Bottom‑Up DP with O(1) Space)
    - 3.1  Core Recurrence
    - 3.2  Base Cases and Initialisation
    - 3.3  Step‑by‑Step Walkthrough for `cost = [10,15,20]` (ASCII Workflow)
    - 3.4  Step‑by‑Step Walkthrough for `cost = [1,100,1,1,1,100,1,1,100,1]`
4.  Recursion Tree for Naive Recursive Approach
    - 4.1  Naive Recursive Implementation
    - 4.2  Recursion Tree for a Small Example
5.  Complexity Analysis
6.  Example Usage and Output
7.  How to Use the Function
8.  Extended Usage and Customization
9.  Conclusion

---

## 1. Full Commented Code

```python
def minCostClimbingStairs(cost):
    """
    Function to calculate the minimum cost to reach the top of the staircase.
    
    :param cost: List[int] -> The cost associated with each step
    :return: int -> Minimum cost to reach the top
    """
    # Number of steps
    total_steps = len(cost)

    # Handle edge cases
    if total_steps == 0:
        return 0
    if total_steps == 1:
        return cost[0]

    # Initialize DP variables for the first two steps
    cost_to_reach_previous_step = cost[0]   # dp[i-1] for i=2
    cost_to_reach_current_step = cost[1]    # dp[i] for i=2

    # Iterate through the remaining steps
    for step_index in range(2, total_steps):
        # Calculate minimum cost to reach current step
        minimum_cost_to_current = cost[step_index] + min(
            cost_to_reach_previous_step,
            cost_to_reach_current_step
        )

        # Update values for next iteration
        cost_to_reach_previous_step = cost_to_reach_current_step
        cost_to_reach_current_step = minimum_cost_to_current

    # Return minimum cost to reach the top (beyond last step)
    return min(cost_to_reach_previous_step, cost_to_reach_current_step)
```

---

## 2. Problem Statement and Intuition

We are given an array `cost` where `cost[i]` is the cost of stepping on the i‑th stair (0‑based index). You can start either at step 0 or step 1, and from any step you can move one or two steps forward. The goal is to reach the **top**, which is one step beyond the last index (i.e., after the final stair). The total cost is the sum of costs of the stairs you **step on**. You do not pay the cost for the starting step? Actually, the problem statement often says you can start at index 0 or 1, and you pay the cost of that step if you start there. The given code initialises `cost_to_reach_previous_step = cost[0]` and `cost_to_reach_current_step = cost[1]`, meaning they assume starting at step 0 costs `cost[0]` and starting at step 1 costs `cost[1]`. Then the DP builds from there.

The problem is a classic dynamic programming one: at each step i, the minimum cost to reach that step is the cost of that step plus the minimum of the costs to reach the two preceding steps (since you can come from i-1 or i-2). The answer is the minimum cost to reach the "top", which is beyond the last step, so we need the minimum of the costs to reach the last two steps (because from either of them you can step off with no extra cost).

---

## 3. Algorithm Explanation (Bottom‑Up DP with O(1) Space)

### 3.1 Core Recurrence

Define `dp[i]` = minimum cost to reach step `i` (where `i` is an index in the cost array).  
Then:

```
dp[0] = cost[0]
dp[1] = cost[1]
dp[i] = cost[i] + min(dp[i-1], dp[i-2])   for i ≥ 2
```

The answer is `min(dp[n-1], dp[n-2])` where n = len(cost), because you can step off from either of the last two steps.

We only need the last two dp values at any time, so we can use two variables instead of an entire array.

### 3.2 Base Cases and Initialisation

- If `len(cost) == 0`: no stairs, cost = 0.
- If `len(cost) == 1`: only one stair, you must step on it (since you start at index 0), so return `cost[0]`.
- Otherwise, initialise `cost_to_reach_previous_step = cost[0]` (dp[i-2]) and `cost_to_reach_current_step = cost[1]` (dp[i-1]) for the first iteration (i=2).

### 3.3 Step‑by‑Step Walkthrough for `cost = [10,15,20]` (ASCII Workflow)

We have `n = 3`.

```
Initialise:
  previous = cost[0] = 10   (dp[0])
  current  = cost[1] = 15   (dp[1])
```

Loop `step_index` from 2 to 2 (only i=2):

```
i=2:
  min_prev = min(previous, current) = min(10,15) = 10
  minimum_cost_to_current = cost[2] + min_prev = 20 + 10 = 30
  Update:
    previous = current = 15
    current  = 30
```

After loop, we have `previous = 15` (dp[1]) and `current = 30` (dp[2]).

Answer = min(previous, current) = min(15,30) = 15.

ASCII representation:

```
Start:            step 0 (cost 10)  step 1 (cost 15)  step 2 (cost 20)   top
dp[0] = 10        [10]              [15]              [20]                ?
dp[1] = 15

i=2: compute dp[2] = 20 + min(10,15) = 30
State after i=2:   dp[0]=10, dp[1]=15, dp[2]=30

Answer = min(dp[1], dp[2]) = min(15,30) = 15
```

### 3.4 Step‑by‑Step Walkthrough for `cost = [1,100,1,1,1,100,1,1,100,1]`

This is the second example. Let’s compute manually with the code.

n = 10  
Initialize:  
  previous = 1  
  current  = 100  

Loop i from 2 to 9:

i=2: cost[2]=1, min(1,100)=1 → new = 1+1=2  
  previous = 100, current = 2  

i=3: cost[3]=1, min(100,2)=2 → new = 1+2=3  
  previous = 2, current = 3  

i=4: cost[4]=1, min(2,3)=2 → new = 1+2=3  
  previous = 3, current = 3  

i=5: cost[5]=100, min(3,3)=3 → new = 100+3=103  
  previous = 3, current = 103  

i=6: cost[6]=1, min(3,103)=3 → new = 1+3=4  
  previous = 103, current = 4  

i=7: cost[7]=1, min(103,4)=4 → new = 1+4=5  
  previous = 4, current = 5  

i=8: cost[8]=100, min(4,5)=4 → new = 100+4=104  
  previous = 5, current = 104  

i=9: cost[9]=1, min(5,104)=5 → new = 1+5=6  
  previous = 104, current = 6  

After loop, answer = min(previous, current) = min(104,6) = 6.

So the minimum cost is 6.

---

## 4. Recursion Tree for Naive Recursive Approach

### 4.1 Naive Recursive Implementation

A direct recursive formulation would be:

```python
def minCostRecursive(cost, i):
    if i < 0:
        return 0
    if i == 0 or i == 1:
        return cost[i]
    return cost[i] + min(minCostRecursive(cost, i-1), minCostRecursive(cost, i-2))

# For the full problem, we would call min(minCostRecursive(cost, n-1), minCostRecursive(cost, n-2))
```

But this recomputes overlapping subproblems many times.

### 4.2 Recursion Tree for a Small Example

Take `cost = [10,15,20]` and compute `minCostRecursive(cost, 2)` (which corresponds to `dp[2]`). The recursion tree:

```
                     minCost(2)
                    /          \
            minCost(1)         minCost(0)
               |                  |
             cost[1]            cost[0]
               |                  |
               15                 10
```

Each node makes two recursive calls. The tree for `minCost(2)` has 5 calls (including leaves). For `n` large, the number of calls grows like the Fibonacci sequence (≈ 1.618^n). This is exponential and impractical for large `n`. The DP approach reduces this to linear time by storing results.

---

## 5. Complexity Analysis

- **Time Complexity:**  
  The iterative DP runs a single loop from 2 to n-1, performing O(1) work per iteration, so **O(n)** time.

- **Space Complexity:**  
  Only two integer variables are used, regardless of input size, so **O(1)** extra space. (The input array itself is not counted as extra space.)

---

## 6. Example Usage and Output

```python
print(minCostClimbingStairs([10,15,20]))                     # 15
print(minCostClimbingStairs([1,100,1,1,1,100,1,1,100,1]))   # 6
print(minCostClimbingStairs([0,0,0,0]))                     # 0
print(minCostClimbingStairs([5]))                           # 5
print(minCostClimbingStairs([]))                            # 0
```

Output:

```
15
6
0
5
0
```

---

## 7. How to Use the Function

1. **Input:** A list of integers `cost` representing the cost to step on each stair. The list can be empty.
2. **Output:** An integer representing the minimum total cost to reach the top (one step beyond the last stair).
3. The function handles edge cases: empty list → 0, one element → that element.
4. No input validation beyond list length; ensure the list contains non‑negative integers (costs are typically non‑negative, but the algorithm works with any integers).

---

## 8. Extended Usage and Customization

- **Returning the path:** If you need the actual sequence of steps, you can extend the DP to store predecessors and reconstruct the path.
- **Handling large costs:** The algorithm works with large integers; Python’s ints are unbounded.
- **Generalising step sizes:** If you could climb up to `k` steps at a time, you would keep a sliding window of the last `k` costs and take the minimum among them.
- **Starting from any step:** The problem often allows starting at index 0 or 1. If you wanted to allow starting at any step (or require starting at step 0), the initialisation would change accordingly.

Example of returning the minimum cost and the path:

```python
def minCostClimbingStairsWithPath(cost):
    n = len(cost)
    if n == 0: return 0, []
    if n == 1: return cost[0], [0]
    dp = [0]*n
    dp[0] = cost[0]
    dp[1] = cost[1]
    parent = [-1]*n
    parent[1] = 0  # hypothetical, not used in reconstruction
    for i in range(2, n):
        if dp[i-1] < dp[i-2]:
            dp[i] = cost[i] + dp[i-1]
            parent[i] = i-1
        else:
            dp[i] = cost[i] + dp[i-2]
            parent[i] = i-2
    # determine last step
    if dp[n-1] < dp[n-2]:
        last = n-1
    else:
        last = n-2
    # reconstruct path backwards
    path = []
    while last != -1:
        path.append(last)
        last = parent[last] if parent[last] != -1 else -1
    path.reverse()
    return min(dp[n-1], dp[n-2]), path
```

---

## 9. Conclusion

The `minCostClimbingStairs` function implements an efficient bottom‑up dynamic programming solution with **O(n) time** and **O(1) space**. It leverages the recurrence:

```
dp[i] = cost[i] + min(dp[i-1], dp[i-2])
```

and returns the minimum of the last two dp values to account for stepping off the final stair. The iterative approach with two variables is a prime example of how dynamic programming can solve a problem with overlapping subproblems in linear time, avoiding the exponential explosion of a naive recursive solution.