# Search in Rotated Sorted Array

**Explanation:**  
The function searches for a target value in a sorted array that has been rotated at an unknown pivot. It returns the index of the target if found, otherwise -1. A modified binary search is used to handle the rotation by identifying which half of the array is properly sorted and then determining if the target lies within that sorted half.

**Approach:**  
Use binary search with two pointers. At each step, compare the middle element with the left element to determine whether the left half is sorted. If the left half is sorted, check if the target lies within that sorted range; if so, narrow the search to the left half, otherwise search the right half. If the left half is not sorted, then the right half must be sorted; similarly, check if the target lies in the sorted right half and adjust pointers accordingly.

**Algorithm:**  
1. Initialize `left = 0`, `right = len(nums) - 1`.  
2. While `left <= right`:  
   - Compute `mid = (left + right) // 2`.  
   - If `nums[mid] == target`, return `mid`.  
   - If `nums[left] <= nums[mid]` (left half is sorted):  
        - If `nums[left] <= target < nums[mid]`, set `right = mid - 1`.  
        - Else, set `left = mid + 1`.  
   - Else (right half is sorted):  
        - If `nums[mid] < target <= nums[right]`, set `left = mid + 1`.  
        - Else, set `right = mid - 1`.  
3. Return `-1` (target not found).

**Pseudocode:**  
```
function search(nums, target):
    left = 0
    right = length(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        if nums[left] <= nums[mid]:          // left half is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:                                // right half is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return -1
```
## Binary Search with Left/Right Comparison (Original)
```python
def search(nums, target):
    left = 0
    right = len(nums) - 1
    # Main logic: binary search on rotated array
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid  # Target found, return index
        if nums[left] <= nums[mid]:  # Left half is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1  # Target in left sorted half
            else:
                left = mid + 1   # Target in right half
        else:  # Right half is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1   # Target in right sorted half
            else:
                right = mid - 1  # Target in left half
    return -1  # Target not found

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Recursive Binary Search
```python
def search(nums, target):
    def recursive_search(left, right):
        if left > right:
            return -1  # Base case: not found
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid  # Found
        # Main logic: decide which half to search
        if nums[left] <= nums[mid]:  # Left half sorted
            if nums[left] <= target < nums[mid]:
                return recursive_search(left, mid - 1)  # Search left
            else:
                return recursive_search(mid + 1, right)  # Search right
        else:  # Right half sorted
            if nums[mid] < target <= nums[right]:
                return recursive_search(mid + 1, right)  # Search right
            else:
                return recursive_search(left, mid - 1)  # Search left
    return recursive_search(0, len(nums) - 1)

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Linear Scan with Enumerate
```python
def search(nums, target):
    # Main logic: iterate through list with index
    for i, val in enumerate(nums):
        if val == target:
            return i  # Return index when found
    return -1  # Not found

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Using `list.index()` with Exception Handling
```python
def search(nums, target):
    # Main logic: use built-in index() method
    try:
        return nums.index(target)  # Returns index if found
    except ValueError:
        return -1  # Not found

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Using `next()` with Generator Expression
```python
def search(nums, target):
    # Main logic: generator yields index when value matches, next() returns first
    # Special: next() with default -1 if no match
    return next((i for i, val in enumerate(nums) if val == target), -1)

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Two-Pass: Find Pivot then Binary Search
```python
def search(nums, target):
    # First, find the pivot (index of smallest element) using binary search
    def find_pivot():
        left, right = 0, len(nums) - 1
        while left < right:
            mid = (left + right) // 2
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid
        return left  # Pivot index
    
    pivot = find_pivot()
    # Determine which half to search based on target comparison with nums[0]
    if target >= nums[0]:
        # Search left half (from 0 to pivot-1)
        left, right = 0, pivot - 1
    else:
        # Search right half (from pivot to end)
        left, right = pivot, len(nums) - 1
    
    # Standard binary search on the chosen sorted half
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Using `filter()` and `enumerate()`
```python
def search(nums, target):
    # Main logic: filter indices where value equals target
    indices = list(filter(lambda i: nums[i] == target, range(len(nums))))
    return indices[0] if indices else -1  # Return first index or -1

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Using NumPy (Professional/External Library)
```python
import numpy as np

def search(nums, target):
    # Main logic: convert to numpy array, use where to get indices
    arr = np.array(nums)
    indices = np.where(arr == target)[0]  # Returns array of indices
    return indices[0] if indices.size > 0 else -1

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Two-Pointer Linear Scan from Both Ends
```python
def search(nums, target):
    # Main logic: use two pointers moving inward from both ends
    left, right = 0, len(nums) - 1
    while left <= right:
        if nums[left] == target:
            return left  # Found at left pointer
        if nums[right] == target:
            return right  # Found at right pointer
        left += 1
        right -= 1
    return -1  # Not found

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```

## Binary Search with Single Condition (Checking Mid and Edges)
```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    # Main logic: binary search with additional condition to handle rotation
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        # If left half is sorted and target is in it, search left
        if nums[left] <= nums[mid] and nums[left] <= target < nums[mid]:
            right = mid - 1
        # If right half is sorted and target is in it, search right
        elif nums[mid] <= nums[right] and nums[mid] < target <= nums[right]:
            left = mid + 1
        # Otherwise, if left half is sorted but target not in it, go right
        elif nums[left] <= nums[mid]:
            left = mid + 1
        # Otherwise, go left
        else:
            right = mid - 1
    return -1

# Call the function and print output
result = search([4,5,6,7,0,1,2], 0)
print(result)  # Output: 4
```