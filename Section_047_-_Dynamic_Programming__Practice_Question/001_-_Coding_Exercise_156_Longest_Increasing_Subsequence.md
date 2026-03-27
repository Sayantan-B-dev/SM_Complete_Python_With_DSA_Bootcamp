# Longest Increasing Subsequence – Detailed Explanation

## Table of Contents

1. [Full Commented Code](#full-commented-code)
2. [Algorithm Description](#algorithm-description)
3. [Step-by-Step Walkthrough with Example](#step-by-step-walkthrough-with-example)
4. [Recursion Tree for Naive Approach](#recursion-tree-for-naive-approach)
5. [From Lower Layer to Higher Layer (Bottom‑Up DP)](#from-lower-layer-to-higher-layer-bottom-up-dp)
6. [Complexity Analysis](#complexity-analysis)
7. [Example Usage and Output](#example-usage-and-output)
8. [Additional Notes](#additional-notes)

---

## Full Commented Code

```python
def length_of_lis(nums):
    """
    Function to return the length of the longest strictly increasing subsequence.
    
    :param nums: List[int] -> List of integers
    :return: int -> Length of the longest increasing subsequence
    """
    # Handle edge case where input list is empty
    if not nums:
        return 0

    # Initialize dp array where dp[i] represents the length of LIS ending at index i
    # Each element alone is a subsequence of length 1
    dp = [1] * len(nums)

    # Iterate through each element as the potential end of a subsequence
    for current_index in range(len(nums)):
        # Compare with all previous elements to see if we can extend a subsequence
        for previous_index in range(current_index):
            # If the previous element is smaller than the current one,
            # we can append current element to the subsequence ending at previous_index
            if nums[previous_index] < nums[current_index]:
                # Update dp[current_index] to the maximum length we can achieve
                dp[current_index] = max(dp[current_index], dp[previous_index] + 1)

    # The LIS length is the maximum value in the dp array
    return max(dp)
```

---

## Algorithm Description

The problem asks for the length of the **longest strictly increasing subsequence** (LIS) in a given sequence of integers. A subsequence is obtained by deleting some (possibly none) elements without changing the order of the remaining elements.

**Dynamic Programming Approach**  
We define `dp[i]` as the length of the longest increasing subsequence that **ends at index `i`** (i.e., the last element is `nums[i]`). The recurrence relation is:

```
dp[i] = 1 + max( dp[j] )   for all j < i such that nums[j] < nums[i]
```

If no such `j` exists, then `dp[i] = 1` (the subsequence consists of only `nums[i]`).

We compute `dp` from left to right, and the answer is `max(dp)`.

---

## Step-by-Step Walkthrough with Example

Let’s walk through the example:  
`nums = [10, 9, 2, 5, 3, 7, 101, 18]`

### Initial State
- `dp = [1, 1, 1, 1, 1, 1, 1, 1]`
- Indices: 0   1   2   3   4   5   6   7

### Iteration over `current_index`

#### **i = 0** (nums[0] = 10)
- Compare with previous indices (none).
- `dp[0]` remains 1.

#### **i = 1** (nums[1] = 9)
- Compare with j = 0: nums[0] = 10 > 9 → no update.
- `dp[1]` remains 1.

#### **i = 2** (nums[2] = 2)
- j = 0: 10 > 2 → no.
- j = 1: 9 > 2 → no.
- `dp[2]` remains 1.

#### **i = 3** (nums[3] = 5)
- j = 0: 10 > 5 → no.
- j = 1: 9 > 5 → no.
- j = 2: 2 < 5 → yes. `dp[2] + 1 = 1 + 1 = 2`. Update `dp[3] = max(1, 2) = 2`.
- `dp[3]` now 2. (Subsequence: [2,5])

#### **i = 4** (nums[4] = 3)
- j = 0: 10 > 3 → no.
- j = 1: 9 > 3 → no.
- j = 2: 2 < 3 → yes. `dp[2] + 1 = 2`. Update `dp[4] = max(1, 2) = 2`.
- j = 3: 5 > 3 → no.
- `dp[4]` = 2. (Subsequence: [2,3])

#### **i = 5** (nums[5] = 7)
- j = 0: 10 > 7 → no.
- j = 1: 9 > 7 → no.
- j = 2: 2 < 7 → yes. `dp[2] + 1 = 2`. Update `dp[5] = max(1, 2) = 2`.
- j = 3: 5 < 7 → yes. `dp[3] + 1 = 2 + 1 = 3`. Update `dp[5] = max(2, 3) = 3`.
- j = 4: 3 < 7 → yes. `dp[4] + 1 = 2 + 1 = 3`. Update `dp[5] = max(3, 3) = 3`.
- `dp[5]` = 3. (One LIS ending at index 5: [2,5,7] or [2,3,7])

#### **i = 6** (nums[6] = 101)
- j = 0: 10 < 101 → yes. `dp[0] + 1 = 1 + 1 = 2`. Update `dp[6] = max(1, 2) = 2`.
- j = 1: 9 < 101 → yes. `dp[1] + 1 = 2`. `dp[6] = max(2, 2) = 2`.
- j = 2: 2 < 101 → yes. `dp[2] + 1 = 2`. `dp[6] = max(2, 2) = 2`.
- j = 3: 5 < 101 → yes. `dp[3] + 1 = 2 + 1 = 3`. `dp[6] = max(2, 3) = 3`.
- j = 4: 3 < 101 → yes. `dp[4] + 1 = 2 + 1 = 3`. `dp[6] = max(3, 3) = 3`.
- j = 5: 7 < 101 → yes. `dp[5] + 1 = 3 + 1 = 4`. `dp[6] = max(3, 4) = 4`.
- `dp[6]` = 4. (Subsequence: [2,5,7,101] or [2,3,7,101])

#### **i = 7** (nums[7] = 18)
- j = 0: 10 < 18 → yes. `dp[0] + 1 = 2`. Update `dp[7] = max(1, 2) = 2`.
- j = 1: 9 < 18 → yes. `dp[1] + 1 = 2`. `dp[7] = max(2, 2) = 2`.
- j = 2: 2 < 18 → yes. `dp[2] + 1 = 2`. `dp[7] = max(2, 2) = 2`.
- j = 3: 5 < 18 → yes. `dp[3] + 1 = 2 + 1 = 3`. `dp[7] = max(2, 3) = 3`.
- j = 4: 3 < 18 → yes. `dp[4] + 1 = 2 + 1 = 3`. `dp[7] = max(3, 3) = 3`.
- j = 5: 7 < 18 → yes. `dp[5] + 1 = 3 + 1 = 4`. `dp[7] = max(3, 4) = 4`.
- j = 6: 101 > 18 → no.
- `dp[7]` = 4. (Subsequence: [2,5,7,18] or [2,3,7,18])

### Final `dp` array
```
dp = [1, 1, 1, 2, 2, 3, 4, 4]
```

The maximum value is **4**. So the LIS length is 4.

### ASCII Workflow for i=5 (nums[5]=7)

```
Indices:       0   1   2   3   4   5
nums:         10   9   2   5   3   7
dp before:     1   1   1   2   2   1

Check j=2: nums[2]=2 < 7 → dp[2]+1=2 → dp[5]=max(1,2)=2
Check j=3: nums[3]=5 < 7 → dp[3]+1=3 → dp[5]=max(2,3)=3
Check j=4: nums[4]=3 < 7 → dp[4]+1=3 → dp[5]=max(3,3)=3

dp after:      1   1   1   2   2   3
```

---

## Recursion Tree for Naive Approach

The naive recursive solution tries all subsequences. For a small array `[3, 1, 2]` the recursion tree looks like this (function `lis(i)` returns LIS length ending at index i):

```
                          lis(2)  (nums[2]=2)
                         /    |    \
           lis(0)      lis(1)      base case
           (nums[0]=3) (nums[1]=1)
               |           |
            lis(-1)     lis(-1)
             base        base
```

Each node calls `lis(j)` for all `j < i` with `nums[j] < nums[i]`. Overlapping subproblems appear: e.g., `lis(-1)` (empty subsequence) is computed many times. This leads to exponential time complexity O(2^n).

The DP version eliminates recomputation by storing results in `dp` array.

---

## From Lower Layer to Higher Layer (Bottom‑Up DP)

**Lower Layer (Base)**:  
For each index `i`, the smallest possible subsequence ending at `i` is just the element itself, so `dp[i] = 1`. This is the lowest layer of subproblems.

**Building Up**:  
We process indices in increasing order. For each `i`, we examine all `j < i`. If `nums[j] < nums[i]`, we can **extend** the subsequence that ends at `j` by appending `nums[i]`. The new length would be `dp[j] + 1`. We take the maximum over all such `j`. This uses previously computed (smaller index) results to compute the current one.

**Higher Layer**:  
The final answer is the maximum over all `dp[i]`, which represents the longest increasing subsequence anywhere in the array.

**Bottom-Up Flow** (for example array):

```
Index:     0   1   2   3   4   5   6   7
nums:     10   9   2   5   3   7 101  18

dp init:   1   1   1   1   1   1   1   1
i=0:       ← (no previous)
i=1:       ← (no smaller)
i=2:       ← (no smaller)
i=3:       2   (from j=2)
i=4:       2   (from j=2)
i=5:       3   (from j=3 or j=4)
i=6:       4   (from j=5)
i=7:       4   (from j=5)
```

The arrows indicate from which previous `dp` value the current `dp` got its update.

---

## Complexity Analysis

- **Time Complexity**: O(n²) – double nested loops over `n` elements.
- **Space Complexity**: O(n) – the `dp` array of size `n`.

For larger inputs, there exists an O(n log n) binary search‑based method, but the O(n²) DP is straightforward and often sufficient for moderate‑sized inputs.

---

## Example Usage and Output

### Example 1: Provided example
```python
print(length_of_lis([10,9,2,5,3,7,101,18]))
# Output: 4
```

### Example 2: Empty list
```python
print(length_of_lis([]))
# Output: 0
```

### Example 3: Single element
```python
print(length_of_lis([42]))
# Output: 1
```

### Example 4: Already increasing
```python
print(length_of_lis([1,2,3,4,5]))
# Output: 5
```

### Example 5: Strictly decreasing
```python
print(length_of_lis([5,4,3,2,1]))
# Output: 1
```

### Example 6: All equal
```python
print(length_of_lis([7,7,7,7]))
# Output: 1   (strictly increasing requires <)
```

### Example 7: Mixed case
```python
print(length_of_lis([3,10,2,1,20]))
# Output: 3   (subsequence [3,10,20] or [2,10,20] or [1,10,20])
```

### Example 8: Larger numbers
```python
print(length_of_lis([0,8,4,12,2,10,6,14,1,9,5,13,3,11,7,15]))
# Output: 6   (e.g., [0,2,6,9,11,15] or [0,4,6,9,11,15] etc.)
```

---

## Additional Notes

- The DP algorithm described returns only the **length** of the LIS. To reconstruct the actual subsequence, we can keep a `prev` array that records the previous index in the best subsequence ending at each `i`.
- The problem assumes **strictly increasing** subsequences. If non‑decreasing were allowed, the condition would be `nums[previous_index] <= nums[current_index]`.
- This DP solution is a classic example of **optimal substructure** (the LIS ending at `i` depends on LIS ending at previous indices) and **overlapping subproblems** (same `j` may be examined many times), which are both exploited by dynamic programming.

---