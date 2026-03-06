# Find Minimum in Rotated Sorted Array

**Explanation:**  
The function finds the smallest element in a sorted array that has been rotated at an unknown pivot. It employs a binary search strategy by comparing the middle element with the rightmost element to determine which half contains the minimum. This runs in O(log n) time.

**Approach:**  
Use binary search with two pointers. At each step, compare the middle element with the rightmost element. If the middle element is larger than the rightmost, the minimum must be in the right half (because the array is rotated, the smaller values are on the right side). Otherwise, the minimum is in the left half (including the middle). Narrow the search until left and right converge, which gives the minimum.

**Algorithm:**  
1. Initialize `left = 0`, `right = len(nums) - 1`.  
2. While `left < right`:  
   - Compute `mid = (left + right) // 2`.  
   - If `nums[mid] > nums[right]`:  
        - Minimum is in the right half → set `left = mid + 1`.  
   - Else:  
        - Minimum is in the left half (including mid) → set `right = mid`.  
3. Return `nums[left]` (or `nums[right]`).

**Pseudocode:**  
```
function findMin(nums):
    left = 0
    right = length(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            right = mid
    return nums[left]
```
## Binary Search with Right Comparison (Original)
```python
def findMin(nums):
    left = 0
    right = len(nums) - 1
    # Main logic: binary search comparing mid with right to find rotation point
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[right]:
            # Minimum is in right half
            left = mid + 1
        else:
            # Minimum is in left half (including mid)
            right = mid
    return nums[left]  # Return the minimum element

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## Linear Scan
```python
def findMin(nums):
    # Main logic: iterate through array and track minimum
    min_val = nums[0]  # Initialize with first element
    for num in nums:
        if num < min_val:
            min_val = num  # Update if smaller found
    return min_val  # Return the overall minimum

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## Using Built-in min()
```python
def findMin(nums):
    # Main logic: use Python's built-in min function
    return min(nums)  # Directly returns the smallest element

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## Recursive Binary Search
```python
def findMin(nums):
    def binary_search(left, right):
        # Base case: single element
        if left == right:
            return nums[left]
        mid = (left + right) // 2
        # Main logic: decide which half contains the minimum
        if nums[mid] > nums[right]:
            # Minimum in right half
            return binary_search(mid + 1, right)
        else:
            # Minimum in left half (including mid)
            return binary_search(left, mid)
    return binary_search(0, len(nums) - 1)

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## While Loop with Pivot Detection (Two-Pointer)
```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    # Main logic: find the pivot where order breaks
    while left < right:
        if nums[left] < nums[right]:
            # Array is sorted, minimum is at left
            return nums[left]
        mid = (left + right) // 2
        if nums[mid] >= nums[left]:
            # Pivot is in right half
            left = mid + 1
        else:
            # Pivot is in left half
            right = mid
    return nums[left]

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## Using sorted() and First Element
```python
def findMin(nums):
    # Main logic: sort the array and return first element
    sorted_nums = sorted(nums)  # Sorts in ascending order
    return sorted_nums[0]  # Minimum is the first element

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## Using List Comprehension with Index of Minimum
```python
def findMin(nums):
    # Main logic: find index of minimum using min with key
    min_index = min(range(len(nums)), key=lambda i: nums[i])
    return nums[min_index]  # Return element at that index

# Alternative: directly use min on enumerate
# return min((val, i) for i, val in enumerate(nums))[0]
# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## Using NumPy (Professional/External Library)
```python
import numpy as np

def findMin(nums):
    # Main logic: convert to numpy array and use np.min
    arr = np.array(nums)
    return np.min(arr)  # Return minimum element

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## Using functools.reduce
```python
from functools import reduce

def findMin(nums):
    # Main logic: reduce applies min comparison cumulatively
    return reduce(lambda x, y: x if x < y else y, nums)

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```

## Binary Search with Left Comparison (Alternative)
```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    # Main logic: compare mid with left to determine which half is sorted
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < nums[left]:
            # Minimum is in left half (including mid)
            right = mid
        elif nums[mid] > nums[right]:
            # Minimum is in right half
            left = mid + 1
        else:
            # Array is sorted from left to right
            return nums[left]
    return nums[left]

# Call the function and print output
result = findMin([4,5,6,7,0,1,2])
print(result)  # Output: 0
```