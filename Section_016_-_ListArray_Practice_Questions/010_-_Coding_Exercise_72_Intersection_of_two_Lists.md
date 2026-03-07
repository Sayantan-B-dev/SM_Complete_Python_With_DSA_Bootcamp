## Set Intersection: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
The intersection of two arrays is a new array containing distinct elements that appear in both input arrays. The original solution uses Python's set data structure to find common elements efficiently.

**Approach:**  
Convert both lists to sets to remove duplicates, then use the set intersection operator (`&`) to find common elements. Finally, convert the result back to a list.

**Algorithm:**  
1. Convert `nums1` to a set `set1`.  
2. Convert `nums2` to a set `set2`.  
3. Compute `intersection = set1 & set2`.  
4. Return `list(intersection)`.

**Pseudocode:**  
```
function intersection(nums1, nums2):
    set1 = set(nums1)
    set2 = set(nums2)
    common = set1 ∩ set2
    return list(common)
```

---

## Using Set Intersection Operator

```python
def intersection_set_op(nums1, nums2):
    # Main logic: convert to sets and use & operator
    return list(set(nums1) & set(nums2))

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_set_op(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```

---

## Using `set.intersection()` Method

```python
def intersection_method(nums1, nums2):
    # Use the intersection method of set
    return list(set(nums1).intersection(nums2))

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_method(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```

---

## List Comprehension with Membership Test

```python
def intersection_list_comp(nums1, nums2):
    # Convert nums2 to set for O(1) lookup
    set2 = set(nums2)
    # List comprehension with condition, then convert to set to remove duplicates
    return list({x for x in nums1 if x in set2})

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_list_comp(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```

---

## Using `filter()` Function

```python
def intersection_filter(nums1, nums2):
    set2 = set(nums2)
    # filter returns iterator, convert to set to remove duplicates, then to list
    return list(set(filter(lambda x: x in set2, nums1)))

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_filter(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```

---

## Nested Loops with Duplicate Handling

```python
def intersection_loops(nums1, nums2):
    result = []
    # Use a set to track seen elements to avoid duplicates in result
    seen = set()
    for x in nums1:
        for y in nums2:
            if x == y and x not in seen:
                result.append(x)
                seen.add(x)
                break
    return result

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_loops(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```

---

## Using `collections.Counter` (Multiset Intersection)

This variant preserves the minimum count of each common element.

```python
from collections import Counter

def intersection_counter(nums1, nums2):
    # Count elements in both lists
    count1 = Counter(nums1)
    count2 = Counter(nums2)
    # Find common elements with minimum count
    result = []
    for num in count1:
        if num in count2:
            result.extend([num] * min(count1[num], count2[num]))
    return result

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_counter(nums1, nums2)
print("Intersection with duplicates:", result)   # Output: Intersection with duplicates: [2, 2]
```

---

## Using `numpy.intersect1d()`

```python
import numpy as np

def intersection_numpy(nums1, nums2):
    # numpy's intersect1d returns sorted unique common elements
    return np.intersect1d(nums1, nums2).tolist()

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_numpy(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```

---

## Two-Pointer on Sorted Arrays (If sorted, or we sort them)

```python
def intersection_two_pointer(nums1, nums2):
    # Sort both arrays
    nums1.sort()
    nums2.sort()
    i = j = 0
    result = []
    while i < len(nums1) and j < len(nums2):
        if nums1[i] < nums2[j]:
            i += 1
        elif nums1[i] > nums2[j]:
            j += 1
        else:
            # Add to result if not duplicate of last added
            if not result or result[-1] != nums1[i]:
                result.append(nums1[i])
            i += 1
            j += 1
    return result

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_two_pointer(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```

---

## Recursive Intersection (Divide and Conquer)

This splits arrays and combines intersections.

```python
def intersection_recursive(nums1, nums2):
    # Base cases
    if not nums1 or not nums2:
        return []
    if len(nums1) == 1:
        return [nums1[0]] if nums1[0] in nums2 else []
    if len(nums2) == 1:
        return [nums2[0]] if nums2[0] in nums1 else []
    # Divide
    mid1 = len(nums1) // 2
    mid2 = len(nums2) // 2
    left = intersection_recursive(nums1[:mid1], nums2[:mid2])
    right = intersection_recursive(nums1[mid1:], nums2[mid2:])
    cross1 = intersection_recursive(nums1[:mid1], nums2[mid2:])
    cross2 = intersection_recursive(nums1[mid1:], nums2[:mid2])
    # Combine (using set to avoid duplicates)
    return list(set(left + right + cross1 + cross2))

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_recursive(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```

---

## Using `reduce` and Intersection

```python
from functools import reduce

def intersection_reduce(nums1, nums2):
    # Convert to sets and use reduce to intersect multiple sets (here only two)
    return list(reduce(lambda a, b: a & b, (set(nums1), set(nums2))))

# Example call
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
result = intersection_reduce(nums1, nums2)
print("Intersection:", result)   # Output: Intersection: [2]
```