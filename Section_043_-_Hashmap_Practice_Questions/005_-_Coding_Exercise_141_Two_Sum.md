# Detailed Analysis of `two_sum` Function

## Table of Contents

1. [Full Commented Code](#full-commented-code)
2. [Function Description and Purpose](#function-description-and-purpose)
3. [Algorithm Explanation](#algorithm-explanation)
   - [3.1 Lower Layer: Data Structures and Operations](#31-lower-layer-data-structures-and-operations)
   - [3.2 Middle Layer: Hashmap Construction and Lookup](#32-middle-layer-hashmap-construction-and-lookup)
   - [3.3 Upper Layer: Iterative Search with Early Exit](#33-upper-layer-iterative-search-with-early-exit)
4. [Workflow Visualization (ASCII)](#workflow-visualization-ascii)
   - [4.1 Step-by-Step Execution](#41-step-by-step-execution)
   - [4.2 Hashmap Evolution](#42-hashmap-evolution)
5. [Complexity Analysis](#complexity-analysis)
6. [Example Usage and Output](#example-usage-and-output)
   - [6.1 Basic Example](#61-basic-example)
   - [6.2 Additional Examples](#62-additional-examples)
   - [6.3 Edge Cases](#63-edge-cases)
7. [How to Use and Integration](#how-to-use-and-integration)
8. [Alternative Approaches (Optional)](#alternative-approaches-optional)
9. [Conclusion](#conclusion)

---

## Full Commented Code

```python
def two_sum(nums, target):
    """
    Finds two numbers in the list that add up to a target sum.
    Returns the indices of the two numbers (0‑based) as a list [index1, index2].
    Assumes exactly one solution exists (per problem constraints).
    If no solution, returns an empty list.

    :param nums: List[int] - Input list of integers
    :param target: int - Target sum to achieve
    :return: List[int] - List of two indices, or [] if none found
    """
    
    # Step 1: Initialize hashmap to store value -> index
    value_to_index = {}
    
    # Step 2: Iterate through the list
    for current_index, current_value in enumerate(nums):
        # Step 3: Compute the complement needed to reach target
        complement = target - current_value
        
        # Step 4: Check if complement already exists in hashmap
        if complement in value_to_index:
            # If found, return indices of complement and current value
            return [value_to_index[complement], current_index]
        
        # Step 5: Store current value and its index in hashmap
        value_to_index[current_value] = current_index
    
    # Step 6: If no solution found, return empty list
    return []
```

---

## Function Description and Purpose

The `two_sum` function solves the classic "Two Sum" problem: given a list of integers `nums` and a target integer `target`, find two distinct elements in the list whose sum equals the target, and return their indices. The algorithm guarantees that each input has exactly one solution (as per typical problem constraints), but the code gracefully handles the case of no solution by returning an empty list.

**Output:** A list of two indices `[i, j]` (with i < j) such that `nums[i] + nums[j] == target`, or `[]` if no such pair exists.

---

## Algorithm Explanation

We will explore the algorithm from the lowest layer (data structures and primitive operations) up to the highest layer (overall logic).

### 3.1 Lower Layer: Data Structures and Operations

At the lowest level, the algorithm uses:

- **Dictionary (`value_to_index`)**: A hash map that associates each encountered value with its index. Dictionaries in Python provide O(1) average-case lookup, insertion, and membership tests.
- **Enumeration**: `enumerate(nums)` yields pairs `(index, value)` while iterating, eliminating manual index tracking.
- **Arithmetic**: Subtraction to compute the complement (`complement = target - current_value`).
- **Conditionals**: `if complement in value_to_index` checks if the required partner has already been seen.
- **Return**: Returns a list of two indices immediately upon finding a match.

### 3.2 Middle Layer: Hashmap Construction and Lookup

The algorithm processes the list sequentially. For each element `current_value` at index `current_index`, it computes the required complement: `complement = target - current_value`.

If the complement already exists in `value_to_index`, that means we have previously seen an element that can pair with the current one to reach the target. Because we are iterating from left to right, the complement's index is guaranteed to be less than the current index (since it was inserted earlier). This satisfies the requirement of distinct indices.

If the complement is not yet in the map, we store the current value and its index into the map, so that future elements can check against it.

### 3.3 Upper Layer: Iterative Search with Early Exit

The algorithm performs a single pass through the list. At each step, it tries to find a complement for the current element among the elements already processed. If found, it returns immediately. This early exit ensures optimal runtime because we do not need to process the rest of the list.

If the loop finishes without finding a pair, the function returns an empty list.

**Key Insight:** By storing previously seen values, we can check for the complement in O(1) time, avoiding a nested loop.

---

## Workflow Visualization (ASCII)

### 4.1 Step-by-Step Execution

Using `nums = [2, 7, 11, 15]`, `target = 9`:

```
Initialize value_to_index = {}

i = 0, value = 2:
  complement = 9 - 2 = 7
  Is 7 in value_to_index? No.
  Store value_to_index[2] = 0
  -> value_to_index = {2:0}

i = 1, value = 7:
  complement = 9 - 7 = 2
  Is 2 in value_to_index? Yes, at index 0.
  Return [value_to_index[2], 1] = [0, 1]
```

### 4.2 Hashmap Evolution

```
+-------+-------+------------+---------------------------------------------+
| Index | Value | Complement | value_to_index (after processing)            |
+-------+-------+------------+---------------------------------------------+
|   0   |   2   |     7      | {2:0}                                       |
|   1   |   7   |     2      | Found match; return [0,1] (no storage)      |
+-------+-------+------------+---------------------------------------------+
```

---

## Complexity Analysis

- **Time Complexity:** O(n) where n = length of `nums`.
  - The algorithm performs a single pass through the list.
  - Each iteration does constant-time operations: hash map lookup, insertion, arithmetic.
  - No nested loops.

- **Space Complexity:** O(n) in the worst case.
  - In the worst case (no solution), the hash map stores all n elements before the loop ends.
  - In the average case, the map may hold fewer elements because we return early.

---

## Example Usage and Output

### 6.1 Basic Example

```python
nums = [2, 7, 11, 15]
target = 9
result = two_sum(nums, target)
print(result)  # Output: [0, 1]
```

### 6.2 Additional Examples

**Example 1:** Unsorted list

```python
nums = [3, 2, 4]
target = 6
print(two_sum(nums, target))  # Output: [1, 2]   (because 2+4=6)
```

**Example 2:** Negative numbers

```python
nums = [-1, -2, -3, -4, -5]
target = -8
print(two_sum(nums, target))  # Output: [2, 4]   (-3 + -5 = -8)
```

**Example 3:** Duplicate numbers

```python
nums = [3, 3]
target = 6
print(two_sum(nums, target))  # Output: [0, 1]
```

**Example 4:** No solution

```python
nums = [1, 2, 3]
target = 7
print(two_sum(nums, target))  # Output: []
```

### 6.3 Edge Cases

- **Empty list:** Loop does not run, returns `[]`.
- **Single element:** No possible pair, returns `[]`.
- **Large numbers:** Works as long as integers fit in Python's arbitrary precision.
- **Zero values:** Works correctly (e.g., `[0, 0]` with target 0 returns `[0,1]`).

---

## How to Use and Integration

To use the function:

1. Copy the function definition into your code.
2. Call it with a list of integers and a target integer.
3. The function returns a list of indices or an empty list.

**Example integration:**

```python
def find_two_sum(nums, target):
    indices = two_sum(nums, target)
    if indices:
        print(f"Indices: {indices}, Values: {nums[indices[0]]} + {nums[indices[1]]} = {target}")
    else:
        print("No two numbers sum to the target.")
```

**Note:** The implementation assumes that each input has at most one solution (or returns the first encountered). If you need to handle multiple solutions or return all pairs, modifications are required.

---

## Alternative Approaches (Optional)

**Brute force (nested loops):**

```python
def two_sum_bruteforce(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i+1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```

- Time: O(n^2), Space: O(1). Not efficient for large lists.

**Using two-pointer after sorting (requires indices tracking):**

```python
def two_sum_sorted(nums, target):
    # This approach loses original indices, so we need to store them.
    # It works only if we need the values, not the indices.
    sorted_nums = sorted(enumerate(nums), key=lambda x: x[1])
    left, right = 0, len(nums)-1
    while left < right:
        s = sorted_nums[left][1] + sorted_nums[right][1]
        if s == target:
            return [sorted_nums[left][0], sorted_nums[right][0]]
        elif s < target:
            left += 1
        else:
            right -= 1
    return []
```

- Time: O(n log n) due to sorting, Space: O(n) for storing indices. Useful when we need to preserve original order.

**Using a dictionary (the provided approach) is the most efficient and straightforward for this problem.**

---

## Conclusion

The `two_sum` function efficiently solves the two-sum problem using a single pass with a hash map, achieving O(n) time and O(n) space. The algorithm is intuitive: for each element, it checks whether its complement (target minus current value) has been seen before; if so, it returns the indices of the complement and the current element. This approach scales well even for large inputs. The code is clear, well-commented, and handles edge cases gracefully. It is a classic example of using space to trade off time.