# Find First and Last Position of Target in Sorted Array

**Explanation:**  
The function returns the starting and ending indices of a given target value in a sorted array. If the target is not found, it returns `[-1, -1]`. It uses two separate binary searches to locate the first and last occurrences, achieving O(log n) time complexity.

**Approach:**  
Perform two modified binary searches. For the first occurrence, when the middle element equals the target, record the position and continue searching in the left half to find an earlier occurrence. For the last occurrence, when the middle element equals the target, record the position and continue searching in the right half to find a later occurrence. The array is assumed to be sorted in non‑decreasing order.

**Algorithm:**  
1. Define `find_first(nums, target)`:
   - Initialize `left = 0`, `right = len(nums)-1`, `first = -1`.
   - While `left <= right`:
        - `mid = (left + right) // 2`
        - If `nums[mid] == target`:
            - `first = mid`
            - `right = mid - 1`   (search left for earlier occurrence)
        - Else if `nums[mid] < target`:
            - `left = mid + 1`
        - Else:
            - `right = mid - 1`
   - Return `first`

2. Define `find_last(nums, target)`:
   - Initialize `left = 0`, `right = len(nums)-1`, `last = -1`.
   - While `left <= right`:
        - `mid = (left + right) // 2`
        - If `nums[mid] == target`:
            - `last = mid`
            - `left = mid + 1`    (search right for later occurrence)
        - Else if `nums[mid] < target`:
            - `left = mid + 1`
        - Else:
            - `right = mid - 1`
   - Return `last`

3. In `searchRange(nums, target)`:
   - `start = find_first(nums, target)`
   - `end = find_last(nums, target)`
   - Return `[start, end]`

**Pseudocode:**  
```
function find_first(nums, target):
    left = 0
    right = length(nums) - 1
    first = -1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            first = mid
            right = mid - 1
        else if nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return first

function find_last(nums, target):
    left = 0
    right = length(nums) - 1
    last = -1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            last = mid
            left = mid + 1
        else if nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return last

function searchRange(nums, target):
    start = find_first(nums, target)
    end = find_last(nums, target)
    return [start, end]
```

## Using bisect Module
```python
import bisect

def searchRange(nums, target):
    # Main logic: use bisect_left to find first occurrence, bisect_right for last+1
    left = bisect.bisect_left(nums, target)
    # If target not found, left is out of bounds or nums[left] != target
    if left == len(nums) or nums[left] != target:
        return [-1, -1]
    # Right is last index: bisect_right - 1
    right = bisect.bisect_right(nums, target) - 1
    return [left, right]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## Linear Scan
```python
def searchRange(nums, target):
    # Main logic: scan from left to right to find first, then scan from right to left for last
    first = -1
    for i in range(len(nums)):
        if nums[i] == target:
            first = i
            break
    if first == -1:
        return [-1, -1]
    last = first
    for i in range(len(nums)-1, first-1, -1):
        if nums[i] == target:
            last = i
            break
    return [first, last]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## Single Binary Search + Expand
```python
def searchRange(nums, target):
    # First find any occurrence using binary search
    left, right = 0, len(nums) - 1
    any_index = -1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            any_index = mid
            break
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    if any_index == -1:
        return [-1, -1]
    # Expand left and right to find boundaries
    first, last = any_index, any_index
    while first > 0 and nums[first-1] == target:
        first -= 1
    while last < len(nums)-1 and nums[last+1] == target:
        last += 1
    return [first, last]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## Recursive Binary Search for Both Ends
```python
def searchRange(nums, target):
    def find_first(left, right):
        if left > right:
            return -1
        mid = (left + right) // 2
        if nums[mid] == target:
            # Check if it's the first occurrence
            if mid == 0 or nums[mid-1] != target:
                return mid
            else:
                return find_first(left, mid-1)
        elif nums[mid] < target:
            return find_first(mid+1, right)
        else:
            return find_first(left, mid-1)
    
    def find_last(left, right):
        if left > right:
            return -1
        mid = (left + right) // 2
        if nums[mid] == target:
            # Check if it's the last occurrence
            if mid == len(nums)-1 or nums[mid+1] != target:
                return mid
            else:
                return find_last(mid+1, right)
        elif nums[mid] < target:
            return find_last(mid+1, right)
        else:
            return find_last(left, mid-1)
    
    first = find_first(0, len(nums)-1)
    if first == -1:
        return [-1, -1]
    last = find_last(first, len(nums)-1)  # can start from first to optimize
    return [first, last]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## While Loops with Separate Binary Searches (Original Style)
```python
def searchRange(nums, target):
    def find_first():
        left, right = 0, len(nums)-1
        first = -1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                first = mid
                right = mid - 1  # keep searching left
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return first
    
    def find_last():
        left, right = 0, len(nums)-1
        last = -1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                last = mid
                left = mid + 1  # keep searching right
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return last
    
    first = find_first()
    if first == -1:
        return [-1, -1]
    last = find_last()
    return [first, last]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## Using List Comprehension and Enumerate
```python
def searchRange(nums, target):
    # Main logic: get indices of all matches using enumerate and list comprehension
    indices = [i for i, val in enumerate(nums) if val == target]
    if not indices:
        return [-1, -1]
    return [indices[0], indices[-1]]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## Two-Pointer Linear from Both Ends
```python
def searchRange(nums, target):
    # Main logic: use two pointers from start and end to find first and last simultaneously
    left, right = 0, len(nums) - 1
    first = last = -1
    while left <= right:
        if nums[left] == target and first == -1:
            first = left
        if nums[right] == target and last == -1:
            last = right
        # Move pointers inward only after both found or needed
        if first != -1 and last != -1:
            break
        if first == -1:
            left += 1
        if last == -1:
            right -= 1
    if first == -1:  # target not found
        return [-1, -1]
    return [first, last]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## Using filter and lambda
```python
def searchRange(nums, target):
    # Main logic: filter indices where value equals target
    indices = list(filter(lambda i: nums[i] == target, range(len(nums))))
    if not indices:
        return [-1, -1]
    return [indices[0], indices[-1]]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## Binary Search with Custom Lower/Upper Bound Functions
```python
def searchRange(nums, target):
    # Define function to find leftmost insertion point (first index where nums[i] >= target)
    def lower_bound():
        left, right = 0, len(nums)
        while left < right:
            mid = (left + right) // 2
            if nums[mid] >= target:
                right = mid
            else:
                left = mid + 1
        return left
    
    # Define function to find rightmost insertion point (first index where nums[i] > target)
    def upper_bound():
        left, right = 0, len(nums)
        while left < right:
            mid = (left + right) // 2
            if nums[mid] > target:
                right = mid
            else:
                left = mid + 1
        return left
    
    start = lower_bound()
    end = upper_bound() - 1
    if start <= end and end < len(nums) and nums[start] == target:
        return [start, end]
    return [-1, -1]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```

## Using NumPy (if available)
```python
import numpy as np

def searchRange(nums, target):
    # Main logic: convert to numpy array, use where to get indices
    arr = np.array(nums)
    indices = np.where(arr == target)[0]
    if len(indices) == 0:
        return [-1, -1]
    return [indices[0], indices[-1]]

# Call the function and print output
result = searchRange([5,7,7,8,8,10], 8)
print(result)  # Output: [3, 4]
```