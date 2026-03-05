## Two Pointer Merge
```python
def merge_two_sorted_lists(list1, list2):
    lp1 = 0                              # Initialize pointer for list1 at start
    lp2 = 0                              # Initialize pointer for list2 at start
    result = []                          # Initialize empty result list

    while lp1 < len(list1) and lp2 < len(list2):  # While both lists have elements
        if list1[lp1] <= list2[lp2]:       # Compare current elements
            result.append(list1[lp1])        # Add smaller element from list1
            lp1 += 1                          # Move list1 pointer forward
        else:                               # list2 element is smaller
            result.append(list2[lp2])        # Add smaller element from list2
            lp2 += 1                          # Move list2 pointer forward

    while lp1 < len(list1):                # Add remaining elements from list1
        result.append(list1[lp1])            # Append each remaining element
        lp1 += 1                              # Move pointer forward

    while lp2 < len(list2):                # Add remaining elements from list2
        result.append(list2[lp2])            # Append each remaining element
        lp2 += 1                              # Move pointer forward

    return result                          # Return merged sorted list

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: Classic O(n+m) two-pointer merge algorithm
```

## Bubble Sort Merge (Slow)
```python
def merge_two_sorted_lists_slow(list1, list2):
    new_list = list1 + list2               # Concatenate both lists
    for i in range(len(new_list)):          # Outer loop for bubble sort passes
        for j in range(len(new_list) - 1 - i):  # Inner loop for comparisons
            if new_list[j] > new_list[j + 1]:    # Check if adjacent elements out of order
                temp = new_list[j]                # Temporary storage for swap
                new_list[j] = new_list[j + 1]     # Swap smaller to left
                new_list[j + 1] = temp             # Complete the swap
    return new_list                          # Return sorted list

print(merge_two_sorted_lists_slow([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: O(n²) bubble sort after concatenation - inefficient but simple
```

## Built-in Sort
```python
def merge_two_sorted_lists(list1, list2):
    result = list1 + list2                 # Concatenate both lists
    result.sort()                          # Use Python's Timsort (O(n log n))
    return result                          # Return sorted merged list

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: Uses Python's optimized built-in sort
```

## Sorted Function
```python
def merge_two_sorted_lists(list1, list2):
    return sorted(list1 + list2)           # Concatenate and use sorted() function

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: Clean one-liner using built-in sorted()
```

## Extend and Sort
```python
def merge_two_sorted_lists(list1, list2):
    result = list1.copy()                  # Create copy of first list
    result.extend(list2)                   # Extend with second list
    result.sort()                          # Sort the combined list
    return result                          # Return merged sorted list

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: Uses extend method instead of concatenation
```

## Heapq Merge
```python
import heapq

def merge_two_sorted_lists(list1, list2):
    return list(heapq.merge(list1, list2))  # Use heapq.merge for efficient merging

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: Heapq's merge function optimized for merging sorted iterables
```

## Recursive Merge
```python
def merge_two_sorted_lists(list1, list2):
    if not list1:                          # Base case: list1 empty
        return list2                         # Return list2
    if not list2:                          # Base case: list2 empty
        return list1                         # Return list1
    
    if list1[0] <= list2[0]:                # Compare first elements
        return [list1[0]] + merge_two_sorted_lists(list1[1:], list2)  # Take from list1, recurse
    else:
        return [list2[0]] + merge_two_sorted_lists(list1, list2[1:])  # Take from list2, recurse

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: Recursive approach, elegant but may hit recursion depth
```

## While with Index Variables
```python
def merge_two_sorted_lists(list1, list2):
    i, j = 0, 0                           # Initialize indices for both lists
    result = []                            # Initialize result list
    
    while i < len(list1) and j < len(list2):  # While both have elements
        if list1[i] < list2[j]:               # Compare elements
            result.append(list1[i])             # Add smaller from list1
            i += 1                               # Increment list1 index
        else:                                   # list2 element is smaller or equal
            result.append(list2[j])             # Add from list2
            j += 1                               # Increment list2 index
    
    result.extend(list1[i:])                 # Add remaining list1 elements
    result.extend(list2[j:])                 # Add remaining list2 elements
    return result                            # Return merged list

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: Clean version with extend for remaining elements
```

## List Comprehension with Counter
```python
def merge_two_sorted_lists(list1, list2):
    result = []                            # Initialize result
    i = j = 0                               # Initialize pointers
    
    while i < len(list1) or j < len(list2):  # While either list has elements
        if j == len(list2) or (i < len(list1) and list1[i] <= list2[j]):  # Check conditions
            result.append(list1[i])            # Add from list1
            i += 1                               # Increment i
        else:                                   # Add from list2
            result.append(list2[j])            # Add from list2
            j += 1                               # Increment j
    
    return result                            # Return merged list

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: Single while loop with complex condition
```

## Numpy Concatenate and Sort
```python
import numpy as np

def merge_two_sorted_lists(list1, list2):
    arr1 = np.array(list1)                 # Convert list1 to numpy array
    arr2 = np.array(list2)                 # Convert list2 to numpy array
    merged = np.concatenate([arr1, arr2])   # Concatenate arrays
    merged.sort()                          # Sort in-place
    return merged.tolist()                  # Convert back to list

print(merge_two_sorted_lists([1, 3, 5], [2, 4, 6]))  # Call with sorted lists - [1, 2, 3, 4, 5, 6]
# Special: NumPy's optimized array operations
```