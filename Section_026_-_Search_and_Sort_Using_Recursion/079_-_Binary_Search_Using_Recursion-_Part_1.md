## 1. Basic Recursive Binary Search (Boolean Return)

**Explanation**  
Determines whether a target value exists in a sorted list using recursion. Returns `True` if found, otherwise `False`.

**Approach**  
- Use a helper function that accepts the list, target, and current search boundaries (start and end indices).  
- Compute the middle index.  
- Base case: if start exceeds end, the element is not present → return `False`.  
- If the middle element equals the target, return `True`.  
- If the target is greater than the middle element, search the right half recursively.  
- Otherwise, search the left half.

**Algorithm**  
1. `binary_search(arr, target, left, right)`:  
   - If `left > right`, return `False`.  
   - `mid = left + (right - left) // 2`.  
   - If `arr[mid] == target`, return `True`.  
   - If `target > arr[mid]`, return `binary_search(arr, target, mid+1, right)`.  
   - Else return `binary_search(arr, target, left, mid-1)`.

**Pseudocode**
```
function binary_search_bool(arr, target, left, right):
    if left > right:
        return False
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return True
    if target > arr[mid]:
        return binary_search_bool(arr, target, mid+1, right)
    else:
        return binary_search_bool(arr, target, left, mid-1)
```

**Implementation**
```python
def binary_search_bool(arr, target, left, right):
    # Base case: empty search space
    if left > right:
        return False
    
    # Calculate mid to avoid integer overflow
    mid = left + (right - left) // 2
    
    # Target found at mid
    if arr[mid] == target:
        return True
    
    # If target is greater, search the right half
    if target > arr[mid]:
        return binary_search_bool(arr, target, mid + 1, right)
    # Otherwise search the left half
    else:
        return binary_search_bool(arr, target, left, mid - 1)
```

**Complexity Analysis**  
- **Time:** O(log n) – each recursive call halves the search space.  
- **Space:** O(log n) – recursion stack depth equals the number of recursive calls.

---

## 2. Recursive Binary Search Returning Index (First Occurrence Found)

**Explanation**  
Returns the index of the target if present in the sorted list, otherwise -1. This variant returns the index of the first occurrence encountered during recursion, which is not necessarily the leftmost occurrence; it returns the index of the element that matches during the standard binary search.

**Approach**  
- Same recursive halving as in boolean search.  
- Instead of returning `True`, return the index when a match occurs.  
- If not found, propagate -1 upward.

**Algorithm**  
1. `binary_search_index(arr, target, left, right)`:  
   - If `left > right`, return -1.  
   - `mid = left + (right - left) // 2`.  
   - If `arr[mid] == target`, return `mid`.  
   - If `target > arr[mid]`, return `binary_search_index(arr, target, mid+1, right)`.  
   - Else return `binary_search_index(arr, target, left, mid-1)`.

**Pseudocode**
```
function binary_search_index(arr, target, left, right):
    if left > right:
        return -1
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    if target > arr[mid]:
        return binary_search_index(arr, target, mid+1, right)
    else:
        return binary_search_index(arr, target, left, mid-1)
```

**Implementation**
```python
def binary_search_index(arr, target, left, right):
    # Base case: element not found
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    # Target found, return its index
    if arr[mid] == target:
        return mid
    
    # Decide direction based on comparison
    if target > arr[mid]:
        return binary_search_index(arr, target, mid + 1, right)
    else:
        return binary_search_index(arr, target, left, mid - 1)
```

**Complexity Analysis**  
- **Time:** O(log n).  
- **Space:** O(log n).

---

## 3. Recursive Binary Search Returning Any Occurrence (Typical)

**Explanation**  
Standard binary search that returns the index of any occurrence of the target. This is identical to variant 2; the distinction is that it returns the index of the element when found, which may not be the first or last if duplicates exist. It is included here for completeness.

**Approach**  
- Identical to variant 2. Returns the first index found during recursion.

**Algorithm**  
Same as variant 2.

**Pseudocode**
```
function binary_search_any(arr, target, left, right):
    if left > right:
        return -1
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    if target > arr[mid]:
        return binary_search_any(arr, target, mid+1, right)
    else:
        return binary_search_any(arr, target, left, mid-1)
```

**Implementation**
```python
def binary_search_any(arr, target, left, right):
    # Standard binary search returning any matching index
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        return mid
    
    if target > arr[mid]:
        return binary_search_any(arr, target, mid + 1, right)
    else:
        return binary_search_any(arr, target, left, mid - 1)
```

**Complexity Analysis**  
- **Time:** O(log n).  
- **Space:** O(log n).

---

## 4. Recursive Binary Search for Leftmost (First) Occurrence

**Explanation**  
Given a sorted list that may contain duplicates, returns the index of the first occurrence of the target. If target is not present, returns -1.

**Approach**  
- Perform binary search but do not stop when a match is found. Instead, continue searching the left half to find any earlier occurrence.  
- When a match is found at index `mid`, record it and continue searching left (`left` to `mid-1`) for an even earlier occurrence.  
- Use a helper to propagate the leftmost index found.

**Algorithm**  
1. `binary_search_leftmost(arr, target, left, right)`:  
   - If `left > right`, return -1.  
   - `mid = left + (right - left) // 2`.  
   - If `arr[mid] == target`:  
        - Recursively search left half for a possible earlier occurrence: `candidate = binary_search_leftmost(arr, target, left, mid-1)`.  
        - If `candidate != -1`, return `candidate`; else return `mid`.  
   - If `target > arr[mid]`, search right half.  
   - Else search left half.

**Pseudocode**
```
function binary_search_leftmost(arr, target, left, right):
    if left > right:
        return -1
    mid = left + (right - left) // 2
    if arr[mid] == target:
        candidate = binary_search_leftmost(arr, target, left, mid-1)
        return candidate if candidate != -1 else mid
    elif target > arr[mid]:
        return binary_search_leftmost(arr, target, mid+1, right)
    else:
        return binary_search_leftmost(arr, target, left, mid-1)
```

**Implementation**
```python
def binary_search_leftmost(arr, target, left, right):
    # Base case: empty interval
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        # Potential match found; check if any earlier occurrence exists
        left_candidate = binary_search_leftmost(arr, target, left, mid - 1)
        # If a left candidate exists, it is earlier; otherwise current mid is the leftmost
        return left_candidate if left_candidate != -1 else mid
    elif target > arr[mid]:
        # Target lies in the right half
        return binary_search_leftmost(arr, target, mid + 1, right)
    else:
        # Target lies in the left half
        return binary_search_leftmost(arr, target, left, mid - 1)
```

**Complexity Analysis**  
- **Time:** O(log n) – each recursive call reduces the search space, though in the worst case of duplicates it may examine both sides, but the total work remains logarithmic because the recursion only continues on one side after a match? Actually, this implementation recursively searches left even after finding a match, but that left search is limited to the left half of the current interval. Overall, each element is examined at most once across the recursion tree, resulting in O(log n) if the recursion explores only one path? Wait, careful: when a match is found, we recursively search the left half for an earlier match. That left half search itself is a binary search, so total calls still logarithmic. But if there are many duplicates, the algorithm might still perform O(log n) calls because each recursion only goes into one subinterval (either left or right). It does not explore both halves after a match; it explores left half recursively, and then that recursion might again find a match and explore further left. So it's still O(log n) depth.  
- **Space:** O(log n) stack depth.

---

## 5. Recursive Binary Search for Rightmost (Last) Occurrence

**Explanation**  
Returns the index of the last occurrence of the target in a sorted list with possible duplicates. Returns -1 if not found.

**Approach**  
- Similar to leftmost search but after finding a match, search the right half for a later occurrence.  
- When a match is found at `mid`, record it and recursively search the right half (`mid+1` to `right`) for a later occurrence. Return the later one if found.

**Algorithm**  
1. `binary_search_rightmost(arr, target, left, right)`:  
   - If `left > right`, return -1.  
   - `mid = left + (right - left) // 2`.  
   - If `arr[mid] == target`:  
        - Recursively search right half: `candidate = binary_search_rightmost(arr, target, mid+1, right)`.  
        - If `candidate != -1`, return `candidate`; else return `mid`.  
   - If `target > arr[mid]`, search right half.  
   - Else search left half.

**Pseudocode**
```
function binary_search_rightmost(arr, target, left, right):
    if left > right:
        return -1
    mid = left + (right - left) // 2
    if arr[mid] == target:
        candidate = binary_search_rightmost(arr, target, mid+1, right)
        return candidate if candidate != -1 else mid
    elif target > arr[mid]:
        return binary_search_rightmost(arr, target, mid+1, right)
    else:
        return binary_search_rightmost(arr, target, left, mid-1)
```

**Implementation**
```python
def binary_search_rightmost(arr, target, left, right):
    # Base case: no elements to search
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        # Found a match; check for later occurrences in the right half
        right_candidate = binary_search_rightmost(arr, target, mid + 1, right)
        # If a later candidate exists, it is rightmost; otherwise current mid is rightmost
        return right_candidate if right_candidate != -1 else mid
    elif target > arr[mid]:
        # Target lies to the right
        return binary_search_rightmost(arr, target, mid + 1, right)
    else:
        # Target lies to the left
        return binary_search_rightmost(arr, target, left, mid - 1)
```

**Complexity Analysis**  
- **Time:** O(log n).  
- **Space:** O(log n).

---

## 6. Recursive Binary Search Using List Slicing

**Explanation**  
Implements binary search without explicit index parameters, using list slices to represent subarrays. This approach is conceptually simple but inefficient due to list copying.

**Approach**  
- Pass the entire list (or a slice) recursively.  
- Base case: empty list → return -1.  
- Compute mid index relative to the current slice.  
- If element at mid matches, return the global index (requires tracking offset).  
- Otherwise, recurse on the left or right slice, adjusting the returned index accordingly.

**Algorithm**  
1. `binary_search_slice(arr, target, offset=0)`:  
   - If `len(arr) == 0`, return -1.  
   - `mid = len(arr) // 2`.  
   - If `arr[mid] == target`, return `offset + mid`.  
   - If `target > arr[mid]`, recurse on `arr[mid+1:]` with `offset + mid + 1`.  
   - Else recurse on `arr[:mid]` with same `offset`.

**Pseudocode**
```
function binary_search_slice(arr, target, offset=0):
    if len(arr) == 0:
        return -1
    mid = len(arr) // 2
    if arr[mid] == target:
        return offset + mid
    elif target > arr[mid]:
        return binary_search_slice(arr[mid+1:], target, offset + mid + 1)
    else:
        return binary_search_slice(arr[:mid], target, offset)
```

**Implementation**
```python
def binary_search_slice(arr, target, offset=0):
    # Base case: empty subarray
    if len(arr) == 0:
        return -1
    
    # Mid index relative to current slice
    mid = len(arr) // 2
    
    # Target found at mid
    if arr[mid] == target:
        # Return global index by adding offset
        return offset + mid
    
    # Decide direction
    if target > arr[mid]:
        # Search right half: elements after mid
        # New offset accounts for elements before the right half
        return binary_search_slice(arr[mid+1:], target, offset + mid + 1)
    else:
        # Search left half: elements before mid
        return binary_search_slice(arr[:mid], target, offset)
```

**Complexity Analysis**  
- **Time:** O(n) – each slice creates a new list, copying O(k) elements per call, leading to O(n log n) or O(n) total depending on implementation; worst-case O(n²) due to repeated copying.  
- **Space:** O(n log n) due to list copies.

---

## 7. Tail-Recursive Binary Search

**Explanation**  
Demonstrates a tail‑recursive formulation where the recursive call is the final operation. Although Python does not optimize tail recursion, the pattern is useful for understanding functional programming concepts.

**Approach**  
- Structure the recursion so that no operation follows the recursive call.  
- In binary search, the recursive call is already the last expression in each branch, so the basic implementation is inherently tail‑recursive.

**Algorithm**  
Identical to variant 2 or 3; the recursive calls are in tail position.

**Pseudocode**
```
function binary_search_tail(arr, target, left, right):
    if left > right:
        return -1
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    if target > arr[mid]:
        return binary_search_tail(arr, target, mid+1, right)
    else:
        return binary_search_tail(arr, target, left, mid-1)
```

**Implementation**
```python
def binary_search_tail(arr, target, left, right):
    # Base case
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        return mid
    
    # Both recursive calls are in tail position
    if target > arr[mid]:
        return binary_search_tail(arr, target, mid + 1, right)
    else:
        return binary_search_tail(arr, target, left, mid - 1)
```

**Complexity Analysis**  
- **Time:** O(log n).  
- **Space:** O(log n) – Python still allocates stack frames.

---

## 8. Recursive Binary Search with Inner Function (Closure)

**Explanation**  
Encapsulates the recursive logic inside an inner function that captures the list and target, avoiding the need to pass them repeatedly. The outer function initiates the recursion.

**Approach**  
- Define an inner function `search(l, r)` that uses `arr` and `target` from the outer scope.  
- Outer function calls `search(0, len(arr)-1)` and returns its result.

**Algorithm**  
1. `binary_search_closure(arr, target)`:  
   - Define `def search(left, right):`  
        - If `left > right`, return -1.  
        - Compute `mid`.  
        - If `arr[mid] == target`, return `mid`.  
        - If `target > arr[mid]`, return `search(mid+1, right)`.  
        - Else return `search(left, mid-1)`.  
   - Return `search(0, len(arr)-1)`.

**Pseudocode**
```
function binary_search_closure(arr, target):
    function search(left, right):
        if left > right:
            return -1
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        if target > arr[mid]:
            return search(mid+1, right)
        else:
            return search(left, mid-1)
    return search(0, len(arr)-1)
```

**Implementation**
```python
def binary_search_closure(arr, target):
    # Inner recursive function that uses arr and target from closure
    def search(left, right):
        # Base case: empty interval
        if left > right:
            return -1
        
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            return mid
        
        if target > arr[mid]:
            return search(mid + 1, right)
        else:
            return search(left, mid - 1)
    
    # Start recursion with full array bounds
    return search(0, len(arr) - 1)
```

**Complexity Analysis**  
- **Time:** O(log n).  
- **Space:** O(log n) – recursion stack.

---

## 9. Recursive Binary Search on Rotated Sorted Array

**Explanation**  
Searches for a target in a rotated sorted array (e.g., [4,5,6,7,0,1,2]) where the pivot point is unknown. Returns the index if found, else -1.

**Approach**  
- Identify which half is normally sorted by comparing `arr[left]` with `arr[mid]`.  
- Check if the target lies in the sorted half; if so, search that half recursively.  
- Otherwise, search the other half.

**Algorithm**  
1. `binary_search_rotated(arr, target, left, right)`:  
   - If `left > right`, return -1.  
   - `mid = left + (right - left) // 2`.  
   - If `arr[mid] == target`, return `mid`.  
   - If `arr[left] <= arr[mid]` (left half is sorted):  
        - If `arr[left] <= target < arr[mid]`, search left half.  
        - Else search right half.  
   - Else (right half is sorted):  
        - If `arr[mid] < target <= arr[right]`, search right half.  
        - Else search left half.

**Pseudocode**
```
function binary_search_rotated(arr, target, left, right):
    if left > right:
        return -1
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    # Determine which half is sorted
    if arr[left] <= arr[mid]:  # left half is sorted
        if arr[left] <= target < arr[mid]:
            return binary_search_rotated(arr, target, left, mid-1)
        else:
            return binary_search_rotated(arr, target, mid+1, right)
    else:  # right half is sorted
        if arr[mid] < target <= arr[right]:
            return binary_search_rotated(arr, target, mid+1, right)
        else:
            return binary_search_rotated(arr, target, left, mid-1)
```

**Implementation**
```python
def binary_search_rotated(arr, target, left, right):
    # Base case
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    # Target found
    if arr[mid] == target:
        return mid
    
    # Check if left half is sorted
    if arr[left] <= arr[mid]:
        # Left half is sorted
        if arr[left] <= target < arr[mid]:
            # Target lies in the sorted left half
            return binary_search_rotated(arr, target, left, mid - 1)
        else:
            # Target may be in the right half
            return binary_search_rotated(arr, target, mid + 1, right)
    else:
        # Right half is sorted
        if arr[mid] < target <= arr[right]:
            # Target lies in the sorted right half
            return binary_search_rotated(arr, target, mid + 1, right)
        else:
            # Target may be in the left half
            return binary_search_rotated(arr, target, left, mid - 1)
```

**Complexity Analysis**  
- **Time:** O(log n) – each call eliminates roughly half the array.  
- **Space:** O(log n).

---

## 10. Recursive Binary Search for Insertion Point (Lower Bound)

**Explanation**  
Returns the index where the target should be inserted to maintain sorted order (i.e., the first position where the element is >= target). This is the lower bound. If the target is present, returns the index of its first occurrence.

**Approach**  
- Standard binary search but modified to always narrow down to the leftmost insertion point.  
- When `arr[mid] >= target`, continue searching the left half (including `mid` as a candidate).  
- When `arr[mid] < target`, search the right half.  
- Base case: when interval reduces to a single element, return that index (the insertion point).

**Algorithm**  
1. `binary_search_lower_bound(arr, target, left, right)`:  
   - If `left > right`, return `left` (insertion point).  
   - `mid = left + (right - left) // 2`.  
   - If `arr[mid] < target`, search right half (`mid+1, right`).  
   - Else (arr[mid] >= target), search left half (`left, mid-1`).  
   - Return the result of recursion.

**Pseudocode**
```
function binary_search_lower_bound(arr, target, left, right):
    if left > right:
        return left
    mid = left + (right - left) // 2
    if arr[mid] < target:
        return binary_search_lower_bound(arr, target, mid+1, right)
    else:
        return binary_search_lower_bound(arr, target, left, mid-1)
```

**Implementation**
```python
def binary_search_lower_bound(arr, target, left, right):
    # Base case: interval exhausted, insertion point is left
    if left > right:
        return left
    
    mid = left + (right - left) // 2
    
    if arr[mid] < target:
        # Target is greater, so insertion point must be to the right
        return binary_search_lower_bound(arr, target, mid + 1, right)
    else:
        # arr[mid] >= target, insertion point could be mid or to the left
        return binary_search_lower_bound(arr, target, left, mid - 1)
```

**Complexity Analysis**  
- **Time:** O(log n).  
- **Space:** O(log n).