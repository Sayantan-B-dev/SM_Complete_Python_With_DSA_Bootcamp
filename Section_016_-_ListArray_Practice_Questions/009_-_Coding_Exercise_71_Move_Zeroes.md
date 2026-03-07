## Move Zeroes: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Given an integer array `nums`, move all zeros to the end while maintaining the relative order of non-zero elements. The operation should be performed in-place without creating a copy of the array.

**Approach:**  
A two-pointer technique is used: one pointer (`pos`) tracks where the next non-zero element should be placed. Iterate through the array; whenever a non-zero is encountered, swap it with the element at `pos` and increment `pos`. This shifts non-zero elements left and accumulates zeros at the end.

**Algorithm:**  
1. Initialize `pos = 0`.  
2. For `i = 0` to `len(nums)-1`:  
   - If `nums[i] != 0`, swap `nums[i]` and `nums[pos]`, then `pos += 1`.  
3. Return `nums`.

**Pseudocode:**  
```
function moveZeroes(nums):
    pos = 0
    for i = 0 to length(nums)-1:
        if nums[i] != 0:
            swap(nums[i], nums[pos])
            pos = pos + 1
    return nums
```

---

## Original Two-Pointer Swap

```python
def move_zeroes_original(nums):
    pos = 0                                   # position to place next non-zero
    for i in range(len(nums)):
        if nums[i] != 0:                       # found non-zero
            # swap current with pos
            nums[i], nums[pos] = nums[pos], nums[i]
            pos += 1                            # advance pos
    return nums

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_original(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```

---

## While Loop with Two Pointers

```python
def move_zeroes_while(nums):
    pos = 0
    i = 0
    while i < len(nums):
        if nums[i] != 0:
            # swap non-zero with position pos
            nums[i], nums[pos] = nums[pos], nums[i]
            pos += 1
        i += 1
    return nums

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_while(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```

---

## List Comprehension (Non In-Place)

```python
def move_zeroes_new_list(nums):
    # create list of non-zero elements
    non_zeros = [x for x in nums if x != 0]
    # count zeros
    zeros = [0] * (len(nums) - len(non_zeros))
    # return concatenated list (original unchanged)
    return non_zeros + zeros

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_new_list(arr)
print("Result:", result)               # Output: Result: [1, 3, 12, 0, 0]
print("Original unchanged:", arr)      # Original unchanged: [0, 1, 0, 3, 12]
```

---

## Using Filter and List Concatenation

```python
def move_zeroes_filter(nums):
    # filter non-zero elements
    non_zeros = list(filter(lambda x: x != 0, nums))
    # zeros are the rest
    zeros = [0] * (len(nums) - len(non_zeros))
    return non_zeros + zeros

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_filter(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```

---

## Stable Partition (Bubble Zeroes to End)

```python
def move_zeroes_bubble(nums):
    # bubble zeros to the end by repeatedly swapping adjacent zeros with next non-zero
    n = len(nums)
    for i in range(n):
        for j in range(n - i - 1):
            if nums[j] == 0 and nums[j + 1] != 0:
                # swap zero with non-zero
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
    return nums

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_bubble(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```

---

## Using Deque for Efficient Left Shifts

```python
from collections import deque

def move_zeroes_deque(nums):
    dq = deque()
    zero_count = 0
    for num in nums:
        if num != 0:
            dq.append(num)          # append non-zero to deque
        else:
            zero_count += 1
    # extend with zeros
    dq.extend([0] * zero_count)
    # convert back to list (in-place modification optional)
    nums[:] = list(dq)
    return nums

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_deque(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```

---

## Count Zeroes and Overwrite

```python
def move_zeroes_count(nums):
    # count non-zero elements
    non_zero_count = 0
    for num in nums:
        if num != 0:
            nums[non_zero_count] = num
            non_zero_count += 1
    # fill the rest with zeros
    for i in range(non_zero_count, len(nums)):
        nums[i] = 0
    return nums

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_count(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```

---

## Recursive Move Zeroes

```python
def move_zeroes_recursive(nums, index=0, pos=0):
    if index >= len(nums):
        return nums
    if nums[index] != 0:
        # swap with pos
        nums[index], nums[pos] = nums[pos], nums[index]
        return move_zeroes_recursive(nums, index + 1, pos + 1)
    else:
        return move_zeroes_recursive(nums, index + 1, pos)

# Wrapper to start recursion
def move_zeroes_rec(nums):
    return move_zeroes_recursive(nums, 0, 0)

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_rec(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```

---

## Using `sorted` with Custom Key

```python
def move_zeroes_sorted(nums):
    # sort by boolean of zero: zeros (True) go to end
    nums[:] = sorted(nums, key=lambda x: x == 0)
    return nums

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_sorted(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```

---

## Using `itertools.chain` to Combine Non-Zeros and Zeros

```python
from itertools import chain

def move_zeroes_chain(nums):
    # separate non-zeros and zeros
    non_zeros = [x for x in nums if x != 0]
    zeros = [0] * (len(nums) - len(non_zeros))
    # chain them together and update original list
    nums[:] = list(chain(non_zeros, zeros))
    return nums

# Example call
arr = [0, 1, 0, 3, 12]
result = move_zeroes_chain(arr)
print("Result:", result)   # Output: Result: [1, 3, 12, 0, 0]
```