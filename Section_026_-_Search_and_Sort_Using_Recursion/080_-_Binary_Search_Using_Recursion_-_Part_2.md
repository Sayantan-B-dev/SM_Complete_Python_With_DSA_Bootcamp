## 1. Basic Binary Search (Boolean)
### Explanation
This variant returns a boolean indicating whether the target element exists in a sorted array. It’s the simplest form of binary search using recursion.

### Approach
- Use a helper function that takes the array, target, start, and end indices.
- Base case: if start > end, the element is not present → return False.
- Calculate mid index.
- If the element at mid equals target, return True.
- If target is greater than mid element, search the right half; otherwise, search the left half.

### Algorithm
1. Define `binary_search_bool(arr, target, start, end)`.
2. If `start > end`, return False.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] == target`, return True.
5. If `target > arr[mid]`, return `binary_search_bool(arr, target, mid+1, end)`.
6. Else, return `binary_search_bool(arr, target, start, mid-1)`.

### Pseudocode
```
function binary_search_bool(arr, target, start, end):
    if start > end:
        return False
    mid = start + (end - start) // 2
    if arr[mid] == target:
        return True
    if target > arr[mid]:
        return binary_search_bool(arr, target, mid+1, end)
    else:
        return binary_search_bool(arr, target, start, mid-1)
```

### Code
```python
def binary_search_bool(arr, target, start, end):
    # Base case: search space exhausted
    if start > end:
        return False

    # Calculate mid to avoid overflow
    mid = start + (end - start) // 2

    # Check mid element
    if arr[mid] == target:
        return True
    # If target is greater, search right half
    elif target > arr[mid]:
        return binary_search_bool(arr, target, mid + 1, end)
    # Otherwise, search left half
    else:
        return binary_search_bool(arr, target, start, mid - 1)

# Wrapper for ease of use
def search_bool(arr, target):
    return binary_search_bool(arr, target, 0, len(arr) - 1)

# Example
arr = [1, 3, 5, 7, 9, 11]
print(search_bool(arr, 7))  # Output: True
print(search_bool(arr, 4))  # Output: False
```

---

## 2. Return Index of Target
### Explanation
This variant returns the index of the target if found, otherwise -1. It’s the standard recursive binary search.

### Approach
- Same as boolean search, but return the index when found, and -1 when not found.

### Algorithm
1. Define `binary_search_index(arr, target, start, end)`.
2. If `start > end`, return -1.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] == target`, return `mid`.
5. If `target > arr[mid]`, return `binary_search_index(arr, target, mid+1, end)`.
6. Else, return `binary_search_index(arr, target, start, mid-1)`.

### Pseudocode
```
function binary_search_index(arr, target, start, end):
    if start > end:
        return -1
    mid = start + (end - start) // 2
    if arr[mid] == target:
        return mid
    if target > arr[mid]:
        return binary_search_index(arr, target, mid+1, end)
    else:
        return binary_search_index(arr, target, start, mid-1)
```

### Code
```python
def binary_search_index(arr, target, start, end):
    # Base case: not found
    if start > end:
        return -1

    mid = start + (end - start) // 2

    if arr[mid] == target:
        return mid
    elif target > arr[mid]:
        # Search in right half
        return binary_search_index(arr, target, mid + 1, end)
    else:
        # Search in left half
        return binary_search_index(arr, target, start, mid - 1)

def search_index(arr, target):
    return binary_search_index(arr, target, 0, len(arr) - 1)

# Example
arr = [2, 4, 6, 8, 10, 12]
print(search_index(arr, 8))   # Output: 3
print(search_index(arr, 5))   # Output: -1
```

---

## 3. First Occurrence (Lower Bound)
### Explanation
In a sorted array with duplicates, this variant finds the index of the first occurrence of the target. If not found, returns -1 (or could return insertion point).

### Approach
- When `arr[mid] == target`, we don’t stop immediately; instead, we continue searching in the left half for any earlier occurrence.
- The first time we find a match, we record it and then search left.

### Algorithm
1. Define `binary_search_first(arr, target, start, end, result=-1)`.
2. If `start > end`, return `result`.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] == target`:
   - Update `result = mid`.
   - Search left: `binary_search_first(arr, target, start, mid-1, result)`.
5. If `target > arr[mid]`, search right half.
6. Else, search left half.

### Pseudocode
```
function binary_search_first(arr, target, start, end, result=-1):
    if start > end:
        return result
    mid = start + (end - start) // 2
    if arr[mid] == target:
        result = mid
        return binary_search_first(arr, target, start, mid-1, result)
    else if target > arr[mid]:
        return binary_search_first(arr, target, mid+1, end, result)
    else:
        return binary_search_first(arr, target, start, mid-1, result)
```

### Code
```python
def binary_search_first(arr, target, start, end, result=-1):
    # Base case: search space exhausted
    if start > end:
        return result

    mid = start + (end - start) // 2

    if arr[mid] == target:
        # Found a match, but there may be earlier ones
        result = mid
        # Continue searching left
        return binary_search_first(arr, target, start, mid - 1, result)
    elif target > arr[mid]:
        # Search right half
        return binary_search_first(arr, target, mid + 1, end, result)
    else:
        # Search left half
        return binary_search_first(arr, target, start, mid - 1, result)

def first_occurrence(arr, target):
    return binary_search_first(arr, target, 0, len(arr) - 1)

# Example
arr = [1, 2, 2, 2, 3, 4]
print(first_occurrence(arr, 2))  # Output: 1
print(first_occurrence(arr, 5))  # Output: -1
```

---

## 4. Last Occurrence (Upper Bound)
### Explanation
This variant finds the index of the last occurrence of the target in a sorted array with duplicates. If not found, returns -1.

### Approach
- Similar to first occurrence, but when a match is found, we search the right half for later occurrences.

### Algorithm
1. Define `binary_search_last(arr, target, start, end, result=-1)`.
2. If `start > end`, return `result`.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] == target`:
   - Update `result = mid`.
   - Search right: `binary_search_last(arr, target, mid+1, end, result)`.
5. If `target > arr[mid]`, search right half.
6. Else, search left half.

### Pseudocode
```
function binary_search_last(arr, target, start, end, result=-1):
    if start > end:
        return result
    mid = start + (end - start) // 2
    if arr[mid] == target:
        result = mid
        return binary_search_last(arr, target, mid+1, end, result)
    else if target > arr[mid]:
        return binary_search_last(arr, target, mid+1, end, result)
    else:
        return binary_search_last(arr, target, start, mid-1, result)
```

### Code
```python
def binary_search_last(arr, target, start, end, result=-1):
    if start > end:
        return result

    mid = start + (end - start) // 2

    if arr[mid] == target:
        result = mid
        # Look for later occurrences in right half
        return binary_search_last(arr, target, mid + 1, end, result)
    elif target > arr[mid]:
        # Target is in right half
        return binary_search_last(arr, target, mid + 1, end, result)
    else:
        # Target is in left half
        return binary_search_last(arr, target, start, mid - 1, result)

def last_occurrence(arr, target):
    return binary_search_last(arr, target, 0, len(arr) - 1)

# Example
arr = [1, 2, 2, 2, 3, 4]
print(last_occurrence(arr, 2))  # Output: 3
print(last_occurrence(arr, 5))  # Output: -1
```

---

## 5. Count Occurrences
### Explanation
This variant counts how many times the target appears in a sorted array using recursion. It can be implemented by finding first and last occurrences and subtracting, or by a recursive counting approach.

### Approach
- Use binary search to find any occurrence, then recursively count left and right.
- When a match is found at mid, count it, then add counts from left subarray (mid-1) and right subarray (mid+1).

### Algorithm
1. Define `binary_search_count(arr, target, start, end)`.
2. If `start > end`, return 0.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] == target`:
   - Return `1 + binary_search_count(arr, target, start, mid-1) + binary_search_count(arr, target, mid+1, end)`.
5. If `target > arr[mid]`, return `binary_search_count(arr, target, mid+1, end)`.
6. Else, return `binary_search_count(arr, target, start, mid-1)`.

### Pseudocode
```
function binary_search_count(arr, target, start, end):
    if start > end:
        return 0
    mid = start + (end - start) // 2
    if arr[mid] == target:
        return 1 + binary_search_count(arr, target, start, mid-1) + binary_search_count(arr, target, mid+1, end)
    else if target > arr[mid]:
        return binary_search_count(arr, target, mid+1, end)
    else:
        return binary_search_count(arr, target, start, mid-1)
```

### Code
```python
def binary_search_count(arr, target, start, end):
    # Base case: empty subarray
    if start > end:
        return 0

    mid = start + (end - start) // 2

    if arr[mid] == target:
        # Count current + left + right occurrences
        return (1 +
                binary_search_count(arr, target, start, mid - 1) +
                binary_search_count(arr, target, mid + 1, end))
    elif target > arr[mid]:
        # Search right half
        return binary_search_count(arr, target, mid + 1, end)
    else:
        # Search left half
        return binary_search_count(arr, target, start, mid - 1)

def count_occurrences(arr, target):
    return binary_search_count(arr, target, 0, len(arr) - 1)

# Example
arr = [1, 2, 2, 2, 3, 4, 4, 4, 4, 5]
print(count_occurrences(arr, 2))  # Output: 3
print(count_occurrences(arr, 4))  # Output: 4
print(count_occurrences(arr, 6))  # Output: 0
```

---

## 6. Search in Rotated Sorted Array
### Explanation
A rotated sorted array (e.g., [4,5,6,7,0,1,2]) is a sorted array that has been rotated at some pivot. This variant searches for a target in such an array using recursion.

### Approach
- Find mid; one half is always sorted.
- Determine which half is sorted.
- Check if target lies in the sorted half; if yes, search that half; otherwise, search the other half.

### Algorithm
1. Define `binary_search_rotated(arr, target, start, end)`.
2. If `start > end`, return -1.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] == target`, return `mid`.
5. If `arr[start] <= arr[mid]` (left half is sorted):
   - If `target >= arr[start] and target < arr[mid]`, search left half: `binary_search_rotated(arr, target, start, mid-1)`.
   - Else, search right half: `binary_search_rotated(arr, target, mid+1, end)`.
6. Else (right half is sorted):
   - If `target > arr[mid] and target <= arr[end]`, search right half.
   - Else, search left half.

### Pseudocode
```
function binary_search_rotated(arr, target, start, end):
    if start > end:
        return -1
    mid = start + (end - start) // 2
    if arr[mid] == target:
        return mid
    if arr[start] <= arr[mid]:  # left half sorted
        if target >= arr[start] and target < arr[mid]:
            return binary_search_rotated(arr, target, start, mid-1)
        else:
            return binary_search_rotated(arr, target, mid+1, end)
    else:  # right half sorted
        if target > arr[mid] and target <= arr[end]:
            return binary_search_rotated(arr, target, mid+1, end)
        else:
            return binary_search_rotated(arr, target, start, mid-1)
```

### Code
```python
def binary_search_rotated(arr, target, start, end):
    if start > end:
        return -1

    mid = start + (end - start) // 2

    if arr[mid] == target:
        return mid

    # Check if left half is sorted
    if arr[start] <= arr[mid]:
        # Target lies in sorted left half?
        if arr[start] <= target < arr[mid]:
            return binary_search_rotated(arr, target, start, mid - 1)
        else:
            return binary_search_rotated(arr, target, mid + 1, end)
    else:
        # Right half is sorted
        if arr[mid] < target <= arr[end]:
            return binary_search_rotated(arr, target, mid + 1, end)
        else:
            return binary_search_rotated(arr, target, start, mid - 1)

def search_rotated(arr, target):
    return binary_search_rotated(arr, target, 0, len(arr) - 1)

# Example
arr = [4, 5, 6, 7, 0, 1, 2]
print(search_rotated(arr, 0))  # Output: 4
print(search_rotated(arr, 3))  # Output: -1
```

---

## 7. Search in Nearly Sorted Array
### Explanation
A nearly sorted array is one where an element that should be at index i can be at i-1, i, or i+1. This variant searches for a target in such an array.

### Approach
- Compute mid as usual.
- Check mid, mid-1, and mid+1 for the target.
- If target is smaller than mid, search left half; if larger, search right half. The range adjustments need care because of possible misplacement.

### Algorithm
1. Define `binary_search_nearly_sorted(arr, target, start, end)`.
2. If `start > end`, return -1.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] == target`, return `mid`.
5. If `mid-1 >= start and arr[mid-1] == target`, return `mid-1`.
6. If `mid+1 <= end and arr[mid+1] == target`, return `mid+1`.
7. If `target < arr[mid]`, search left half excluding mid-1 (since we already checked it): `binary_search_nearly_sorted(arr, target, start, mid-2)`.
8. Else, search right half: `binary_search_nearly_sorted(arr, target, mid+2, end)`.

### Pseudocode
```
function binary_search_nearly_sorted(arr, target, start, end):
    if start > end:
        return -1
    mid = start + (end - start) // 2
    if arr[mid] == target:
        return mid
    if mid-1 >= start and arr[mid-1] == target:
        return mid-1
    if mid+1 <= end and arr[mid+1] == target:
        return mid+1
    if target < arr[mid]:
        return binary_search_nearly_sorted(arr, target, start, mid-2)
    else:
        return binary_search_nearly_sorted(arr, target, mid+2, end)
```

### Code
```python
def binary_search_nearly_sorted(arr, target, start, end):
    if start > end:
        return -1

    mid = start + (end - start) // 2

    # Check mid and its neighbors
    if arr[mid] == target:
        return mid
    if mid - 1 >= start and arr[mid - 1] == target:
        return mid - 1
    if mid + 1 <= end and arr[mid + 1] == target:
        return mid + 1

    # Decide which half to search
    if target < arr[mid]:
        # Search left half, skipping mid-1 (already checked)
        return binary_search_nearly_sorted(arr, target, start, mid - 2)
    else:
        # Search right half, skipping mid+1 (already checked)
        return binary_search_nearly_sorted(arr, target, mid + 2, end)

def search_nearly_sorted(arr, target):
    return binary_search_nearly_sorted(arr, target, 0, len(arr) - 1)

# Example: nearly sorted array (each element may be off by ±1)
arr = [10, 3, 40, 20, 50, 80, 70]  # Sorted would be [3,10,20,40,50,70,80]
print(search_nearly_sorted(arr, 40))  # Output: 2 (index of 40)
print(search_nearly_sorted(arr, 70))  # Output: 6
print(search_nearly_sorted(arr, 5))   # Output: -1
```

---

## 8. Find Floor of Target
### Explanation
Floor of a target in a sorted array is the largest element ≤ target. If target is smaller than all elements, floor is -∞ (or -1 index). This variant returns the index of the floor.

### Approach
- Standard binary search, but when target is less than mid, we go left; when greater or equal, we update result and go right to find a larger floor (closer to target).

### Algorithm
1. Define `binary_search_floor(arr, target, start, end, result=-1)`.
2. If `start > end`, return `result`.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] <= target`:
   - Update `result = mid` (potential floor).
   - Search right for a larger floor: `binary_search_floor(arr, target, mid+1, end, result)`.
5. Else:
   - Search left: `binary_search_floor(arr, target, start, mid-1, result)`.

### Pseudocode
```
function binary_search_floor(arr, target, start, end, result=-1):
    if start > end:
        return result
    mid = start + (end - start) // 2
    if arr[mid] <= target:
        result = mid
        return binary_search_floor(arr, target, mid+1, end, result)
    else:
        return binary_search_floor(arr, target, start, mid-1, result)
```

### Code
```python
def binary_search_floor(arr, target, start, end, result=-1):
    if start > end:
        return result

    mid = start + (end - start) // 2

    if arr[mid] <= target:
        # Current element is a candidate for floor
        result = mid
        # Try to find a larger floor (closer to target)
        return binary_search_floor(arr, target, mid + 1, end, result)
    else:
        # Current element too large, search left
        return binary_search_floor(arr, target, start, mid - 1, result)

def floor_index(arr, target):
    # Returns index of floor element; if no floor, returns -1
    return binary_search_floor(arr, target, 0, len(arr) - 1)

# Example
arr = [1, 2, 4, 6, 8, 10]
print(floor_index(arr, 5))   # Output: 2 (value 4)
print(floor_index(arr, 1))   # Output: 0 (value 1)
print(floor_index(arr, 0))   # Output: -1 (no floor)
```

---

## 9. Find Ceil of Target
### Explanation
Ceil of a target in a sorted array is the smallest element ≥ target. If target is larger than all elements, ceil is +∞ (or -1 index). This variant returns the index of the ceil.

### Approach
- Similar to floor but with opposite condition.

### Algorithm
1. Define `binary_search_ceil(arr, target, start, end, result=-1)`.
2. If `start > end`, return `result`.
3. `mid = start + (end - start) // 2`.
4. If `arr[mid] >= target`:
   - Update `result = mid` (potential ceil).
   - Search left for a smaller ceil: `binary_search_ceil(arr, target, start, mid-1, result)`.
5. Else:
   - Search right: `binary_search_ceil(arr, target, mid+1, end, result)`.

### Pseudocode
```
function binary_search_ceil(arr, target, start, end, result=-1):
    if start > end:
        return result
    mid = start + (end - start) // 2
    if arr[mid] >= target:
        result = mid
        return binary_search_ceil(arr, target, start, mid-1, result)
    else:
        return binary_search_ceil(arr, target, mid+1, end, result)
```

### Code
```python
def binary_search_ceil(arr, target, start, end, result=-1):
    if start > end:
        return result

    mid = start + (end - start) // 2

    if arr[mid] >= target:
        # Current element is a candidate for ceil
        result = mid
        # Try to find a smaller ceil (closer to target)
        return binary_search_ceil(arr, target, start, mid - 1, result)
    else:
        # Current element too small, search right
        return binary_search_ceil(arr, target, mid + 1, end, result)

def ceil_index(arr, target):
    # Returns index of ceil element; if no ceil, returns -1
    return binary_search_ceil(arr, target, 0, len(arr) - 1)

# Example
arr = [1, 2, 4, 6, 8, 10]
print(ceil_index(arr, 5))   # Output: 3 (value 6)
print(ceil_index(arr, 10))  # Output: 5 (value 10)
print(ceil_index(arr, 11))  # Output: -1 (no ceil)
```

---

## 10. Binary Search with Custom Comparator
### Explanation
This variant allows searching in a sorted list of custom objects using a comparator function that defines the ordering and equality. It’s useful for complex data structures.

### Approach
- Accept a comparator function `cmp` that takes two arguments (element and target) and returns -1, 0, or 1 (like in C’s qsort).
- Use `cmp(arr[mid], target)` to decide the direction.

### Algorithm
1. Define `binary_search_custom(arr, target, cmp, start, end)`.
2. If `start > end`, return -1.
3. `mid = start + (end - start) // 2`.
4. `comparison = cmp(arr[mid], target)`.
5. If `comparison == 0`, return `mid`.
6. If `comparison < 0`, search right half (mid+1, end).
7. Else, search left half (start, mid-1).

### Pseudocode
```
function binary_search_custom(arr, target, cmp, start, end):
    if start > end:
        return -1
    mid = start + (end - start) // 2
    c = cmp(arr[mid], target)
    if c == 0:
        return mid
    if c < 0:
        return binary_search_custom(arr, target, cmp, mid+1, end)
    else:
        return binary_search_custom(arr, target, cmp, start, mid-1)
```

### Code
```python
def binary_search_custom(arr, target, cmp, start, end):
    if start > end:
        return -1

    mid = start + (end - start) // 2
    # Compare mid element with target using provided comparator
    c = cmp(arr[mid], target)

    if c == 0:
        return mid
    elif c < 0:
        # arr[mid] < target, search right
        return binary_search_custom(arr, target, cmp, mid + 1, end)
    else:
        # arr[mid] > target, search left
        return binary_search_custom(arr, target, cmp, start, mid - 1)

def search_custom(arr, target, cmp):
    return binary_search_custom(arr, target, cmp, 0, len(arr) - 1)

# Example: search for a person by age in a list of Person objects sorted by age
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __repr__(self):
        return f"{self.name}({self.age})"

# Comparator that compares by age
def compare_by_age(person, target_age):
    if person.age < target_age:
        return -1
    elif person.age == target_age:
        return 0
    else:
        return 1

people = [Person("Alice", 25), Person("Bob", 30), Person("Charlie", 35), Person("Diana", 40)]
# Sorted by age
target_age = 35
index = search_custom(people, target_age, compare_by_age)
print(index)  # Output: 2
print(people[index] if index != -1 else "Not found")  # Output: Charlie(35)

# Non-existent age
print(search_custom(people, 42, compare_by_age))  # Output: -1
```