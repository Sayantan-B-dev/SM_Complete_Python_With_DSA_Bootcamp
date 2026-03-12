## 1. Classic Recursive Merge Sort (Top-Down)

### Explanation
Classic merge sort is a divide-and-conquer algorithm that recursively splits the array into halves until each subarray contains a single element, then merges the sorted halves back together. This implementation uses a helper merge function that creates a temporary list for merging.

### Approach
- If the array has one element, it's already sorted.
- Recursively sort the left half and the right half.
- Merge the two sorted halves by comparing elements and building a new temporary list, then copying back to the original array.

### Algorithm
1. Divide: find middle index `m = (s+e)//2`.
2. Conquer: recursively sort `[s..m]` and `[m+1..e]`.
3. Combine: merge the two sorted subarrays using a temporary array.

### Pseudocode
```
function merge(arr, s, m, e):
    left = arr[s..m], right = arr[m+1..e]
    i = j = 0, k = s
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            arr[k] = left[i]; i++
        else:
            arr[k] = right[j]; j++
        k++
    while i < len(left):
        arr[k] = left[i]; i++; k++
    while j < len(right):
        arr[k] = right[j]; j++; k++

function mergeSort(arr, s, e):
    if s >= e: return
    m = (s+e)//2
    mergeSort(arr, s, m)
    mergeSort(arr, m+1, e)
    merge(arr, s, m, e)
```

### Code
```python
def merge(arr, s, m, e):
    # Create temporary lists for left and right halves
    left = arr[s:m+1]   # slice from s to m (inclusive)
    right = arr[m+1:e+1] # slice from m+1 to e

    i = j = 0
    k = s

    # Merge two sorted lists back into arr
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            arr[k] = left[i]
            i += 1
        else:
            arr[k] = right[j]
            j += 1
        k += 1

    # Copy remaining elements from left (if any)
    while i < len(left):
        arr[k] = left[i]
        i += 1
        k += 1

    # Copy remaining elements from right (if any)
    while j < len(right):
        arr[k] = right[j]
        j += 1
        k += 1

def merge_sort(arr, s, e):
    # Base case: single element or empty
    if s >= e:
        return

    # Find middle index
    m = (s + e) // 2

    # Recursively sort both halves
    merge_sort(arr, s, m)
    merge_sort(arr, m+1, e)

    # Merge sorted halves
    merge(arr, s, m, e)

def sort(arr):
    merge_sort(arr, 0, len(arr)-1)

# Example usage
arr = [6, 5, 12, 10, 9, 1]
sort(arr)
print("Sorted:", arr)  # Output: [1, 5, 6, 9, 10, 12]
```

---

## 2. Iterative Bottom-Up Merge Sort

### Explanation
Bottom-up merge sort avoids recursion by starting with subarrays of size 1 and repeatedly merging adjacent subarrays, doubling the size each pass. It uses a temporary array for merging.

### Approach
- Iterate over subarray sizes: 1, 2, 4, ... until size >= length.
- For each size, merge adjacent pairs of subarrays of that size.
- Use a temporary array to hold merged result, then copy back.

### Algorithm
1. Let `size = 1`. While `size < n`:
   - For `left = 0` to `n-1` step `2*size`:
     - `mid = left + size - 1`
     - `right = min(left + 2*size - 1, n-1)`
     - Merge `arr[left..mid]` and `arr[mid+1..right]` using temp array.
   - Double `size`.

### Pseudocode
```
function merge(arr, l, m, r, temp):
    i = l, j = m+1, k = l
    while i <= m and j <= r:
        if arr[i] <= arr[j]:
            temp[k] = arr[i]; i++
        else:
            temp[k] = arr[j]; j++
        k++
    while i <= m:
        temp[k] = arr[i]; i++; k++
    while j <= r:
        temp[k] = arr[j]; j++; k++
    for k = l to r:
        arr[k] = temp[k]

function mergeSort(arr):
    n = len(arr)
    temp = [0]*n
    size = 1
    while size < n:
        for l in range(0, n-1, 2*size):
            m = min(l+size-1, n-1)
            r = min(l+2*size-1, n-1)
            if m < r:
                merge(arr, l, m, r, temp)
        size *= 2
```

### Code
```python
def merge(arr, l, m, r, temp):
    i = l       # start of left subarray
    j = m + 1   # start of right subarray
    k = l       # index in temp

    # Merge two sorted halves into temp
    while i <= m and j <= r:
        if arr[i] <= arr[j]:
            temp[k] = arr[i]
            i += 1
        else:
            temp[k] = arr[j]
            j += 1
        k += 1

    # Copy remaining left elements
    while i <= m:
        temp[k] = arr[i]
        i += 1
        k += 1

    # Copy remaining right elements
    while j <= r:
        temp[k] = arr[j]
        j += 1
        k += 1

    # Copy back from temp to original array
    for idx in range(l, r+1):
        arr[idx] = temp[idx]

def iterative_merge_sort(arr):
    n = len(arr)
    temp = [0] * n          # Preallocate temp array
    size = 1

    # Double subarray size each iteration
    while size < n:
        # Merge adjacent subarrays of current size
        for left in range(0, n-1, 2*size):
            mid = left + size - 1
            right = min(left + 2*size - 1, n-1)
            if mid < right:          # Only merge if there are two subarrays
                merge(arr, left, mid, right, temp)
        size *= 2

# Example usage
arr = [6, 5, 12, 10, 9, 1]
iterative_merge_sort(arr)
print("Sorted:", arr)  # Output: [1, 5, 6, 9, 10, 12]
```

---

## 3. Merge Sort with Sentinel Values

### Explanation
This variant simplifies the merge logic by adding a sentinel (a very large value) at the end of each temporary subarray. This avoids checking for bounds during the merge loop because the sentinel ensures one subarray never runs out before the other.

### Approach
- When creating left and right slices, append `float('inf')` to each.
- Merge by comparing until both sentinels are reached (implicitly handled by loop limits).
- No need for post-copy loops because sentinel guarantees all elements are merged.

### Algorithm
1. Split array into left and right slices plus sentinel.
2. Merge by comparing elements; sentinel ensures no index out of bounds.
3. Loop ends when both indices reach the sentinel (but we know exactly `len(left)-1` iterations).

### Pseudocode
```
function merge(arr, s, m, e):
    left = arr[s..m] + [∞]
    right = arr[m+1..e] + [∞]
    i = j = 0
    for k = s to e:
        if left[i] <= right[j]:
            arr[k] = left[i]; i++
        else:
            arr[k] = right[j]; j++
```

### Code
```python
def merge_sentinel(arr, s, m, e):
    # Create left and right subarrays with a sentinel at the end
    left = arr[s:m+1] + [float('inf')]   # left half plus sentinel
    right = arr[m+1:e+1] + [float('inf')] # right half plus sentinel

    i = j = 0
    # Merge: exactly (e-s+1) iterations, sentinels prevent overflow
    for k in range(s, e+1):
        # Main logic: compare and pick smaller element
        if left[i] <= right[j]:
            arr[k] = left[i]
            i += 1
        else:
            arr[k] = right[j]
            j += 1
    # No need for extra while loops because sentinel handles leftovers

def merge_sort_sentinel(arr, s, e):
    if s >= e:
        return
    m = (s + e) // 2
    merge_sort_sentinel(arr, s, m)
    merge_sort_sentinel(arr, m+1, e)
    merge_sentinel(arr, s, m, e)

def sort(arr):
    merge_sort_sentinel(arr, 0, len(arr)-1)

# Example usage
arr = [6, 5, 12, 10, 9, 1]
sort(arr)
print("Sorted:", arr)  # Output: [1, 5, 6, 9, 10, 12]
```

---

## 4. Merge Sort with Preallocated Temporary Array

### Explanation
Classic merge sort creates a new temporary list for every merge call, leading to many allocations. This variant allocates a single temporary array at the beginning and reuses it for all merges, reducing overhead. The merge function writes into this temp array and copies back.

### Approach
- Pass a temporary list `temp` to the recursive function.
- In merge, use `temp` to store merged elements, then copy back.
- The temp array must be at least as large as the original array.

### Algorithm
1. Allocate `temp` of size `n`.
2. Recursively sort halves, using `temp` in merge.
3. Merge: write merged result into `temp`, then copy to original array.

### Pseudocode
```
function merge(arr, s, m, e, temp):
    i = s, j = m+1, k = s
    while i <= m and j <= e:
        if arr[i] <= arr[j]:
            temp[k] = arr[i]; i++
        else:
            temp[k] = arr[j]; j++
        k++
    while i <= m:
        temp[k] = arr[i]; i++; k++
    while j <= e:
        temp[k] = arr[j]; j++; k++
    for k = s to e:
        arr[k] = temp[k]

function mergeSort(arr, s, e, temp):
    if s >= e: return
    m = (s+e)//2
    mergeSort(arr, s, m, temp)
    mergeSort(arr, m+1, e, temp)
    merge(arr, s, m, e, temp)
```

### Code
```python
def merge(arr, s, m, e, temp):
    i = s
    j = m + 1
    k = s

    # Merge into temp array
    while i <= m and j <= e:
        if arr[i] <= arr[j]:
            temp[k] = arr[i]
            i += 1
        else:
            temp[k] = arr[j]
            j += 1
        k += 1

    # Copy remaining left elements
    while i <= m:
        temp[k] = arr[i]
        i += 1
        k += 1

    # Copy remaining right elements
    while j <= e:
        temp[k] = arr[j]
        j += 1
        k += 1

    # Copy back from temp to original array
    for idx in range(s, e+1):
        arr[idx] = temp[idx]

def merge_sort_helper(arr, s, e, temp):
    if s >= e:
        return
    m = (s + e) // 2
    merge_sort_helper(arr, s, m, temp)
    merge_sort_helper(arr, m+1, e, temp)
    merge(arr, s, m, e, temp)

def sort(arr):
    temp = [0] * len(arr)   # Preallocate temp array
    merge_sort_helper(arr, 0, len(arr)-1, temp)

# Example usage
arr = [6, 5, 12, 10, 9, 1]
sort(arr)
print("Sorted:", arr)  # Output: [1, 5, 6, 9, 10, 12]
```

---

## 5. In-Place Merge Sort (Using Rotations)

### Explanation
True in-place merge sort with O(1) extra space is complex. This variant implements a simple in-place merge using array rotations, but it has O(n²) worst-case merge time. It's educational to show that merging without extra space can be done by shifting elements.

### Approach
- To merge two sorted subarrays [s..m] and [m+1..e] in-place:
  - While both subarrays have elements:
    - If the first element of the left subarray is <= first of right, advance left.
    - Else, rotate the entire right subarray left by one position and move the first right element into the correct place. This shifts elements.
- This is inefficient but demonstrates in-place merging.

### Algorithm
1. `i = s`, `j = m+1`.
2. While `i <= m` and `j <= e`:
   - If `arr[i] <= arr[j]`, increment `i`.
   - Else:
     - Store `arr[j]` in a temporary variable.
     - Shift `arr[i..j-1]` right by one.
     - Place stored value at `arr[i]`.
     - Increment `i`, `m`, `j`.
3. Recursively sort halves before merging.

### Pseudocode
```
function inPlaceMerge(arr, s, m, e):
    i = s, j = m+1
    while i <= m and j <= e:
        if arr[i] <= arr[j]:
            i++
        else:
            temp = arr[j]
            for k = j down to i+1:
                arr[k] = arr[k-1]
            arr[i] = temp
            i++; m++; j++

function mergeSort(arr, s, e):
    if s >= e: return
    m = (s+e)//2
    mergeSort(arr, s, m)
    mergeSort(arr, m+1, e)
    inPlaceMerge(arr, s, m, e)
```

### Code
```python
def in_place_merge(arr, s, m, e):
    i = s
    j = m + 1

    while i <= m and j <= e:
        # If left element is already in place, move on
        if arr[i] <= arr[j]:
            i += 1
        else:
            # Right element is smaller; need to insert it before left
            temp = arr[j]
            # Shift all elements from i to j-1 one position right
            for k in range(j, i, -1):
                arr[k] = arr[k-1]
            arr[i] = temp
            # Update indices after rotation
            i += 1
            m += 1
            j += 1

def merge_sort_inplace(arr, s, e):
    if s >= e:
        return
    m = (s + e) // 2
    merge_sort_inplace(arr, s, m)
    merge_sort_inplace(arr, m+1, e)
    in_place_merge(arr, s, m, e)

def sort(arr):
    merge_sort_inplace(arr, 0, len(arr)-1)

# Example usage
arr = [6, 5, 12, 10, 9, 1]
sort(arr)
print("Sorted:", arr)  # Output: [1, 5, 6, 9, 10, 12]
```

---

## 6. Merge Sort for Linked Lists

### Explanation
Merge sort works naturally on linked lists because it doesn't require random access. This variant sorts a singly linked list by recursively splitting at the middle (using slow/fast pointers) and merging sorted lists.

### Approach
- Base case: if list is empty or has one node, return it.
- Find middle node using slow and fast pointers.
- Split list into two halves at the middle.
- Recursively sort both halves.
- Merge the two sorted lists by comparing node values.

### Algorithm
1. If head is None or head.next is None: return head.
2. Find middle and split: `prev` stops before middle, set second half = `prev.next`, then `prev.next = None`.
3. Recursively sort left and right.
4. Merge: create dummy node, compare and link nodes.

### Pseudocode
```
function merge(l1, l2):
    dummy = ListNode(0)
    tail = dummy
    while l1 and l2:
        if l1.val <= l2.val:
            tail.next = l1; l1 = l1.next
        else:
            tail.next = l2; l2 = l2.next
        tail = tail.next
    tail.next = l1 if l1 else l2
    return dummy.next

function mergeSort(head):
    if not head or not head.next: return head
    slow = head, fast = head.next
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    mid = slow.next
    slow.next = None
    left = mergeSort(head)
    right = mergeSort(mid)
    return merge(left, right)
```

### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def merge_lists(l1, l2):
    dummy = ListNode()   # Dummy node to simplify head handling
    tail = dummy

    # Merge while both lists have nodes
    while l1 and l2:
        if l1.val <= l2.val:
            tail.next = l1
            l1 = l1.next
        else:
            tail.next = l2
            l2 = l2.next
        tail = tail.next

    # Attach remaining nodes
    tail.next = l1 if l1 else l2
    return dummy.next

def merge_sort_list(head):
    # Base case: empty or single node
    if not head or not head.next:
        return head

    # Find middle using slow/fast pointers
    slow = head
    fast = head.next
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    mid = slow.next   # Start of second half
    slow.next = None  # Split the list

    # Recursively sort both halves
    left = merge_sort_list(head)
    right = merge_sort_list(mid)

    # Merge sorted halves
    return merge_lists(left, right)

# Helper to create list from array and print
def array_to_list(arr):
    dummy = ListNode()
    current = dummy
    for val in arr:
        current.next = ListNode(val)
        current = current.next
    return dummy.next

def print_list(head):
    vals = []
    while head:
        vals.append(head.val)
        head = head.next
    print(vals)

# Example usage
arr = [6, 5, 12, 10, 9, 1]
head = array_to_list(arr)
sorted_head = merge_sort_list(head)
print("Sorted linked list:", end=" ")
print_list(sorted_head)  # Output: [1, 5, 6, 9, 10, 12]
```

---

## 7. Parallel Merge Sort (using multiprocessing)

### Explanation
This variant uses Python's `multiprocessing` pool to sort subarrays in parallel. It splits the array into chunks, sorts each chunk concurrently, and then merges the results. This demonstrates how divide-and-conquer can be parallelized.

### Approach
- Determine number of processes (e.g., CPU count).
- Split array into roughly equal chunks.
- Use a pool to sort each chunk (using built-in `sorted` or merge sort recursively).
- Merge sorted chunks using a multi-way merge (here we use a simple pairwise merge).

### Algorithm
1. If array size is small, sort directly.
2. Otherwise, split into `num_processes` chunks.
3. Use `pool.map` to sort each chunk in parallel.
4. Merge sorted chunks sequentially.

### Pseudocode
```
function parallelMergeSort(arr, num_processes):
    if len(arr) <= 1: return arr
    chunk_size = ceil(len(arr)/num_processes)
    chunks = [arr[i:i+chunk_size] for i in range(0, len(arr), chunk_size)]
    with Pool(num_processes) as pool:
        sorted_chunks = pool.map(sorted, chunks)
    result = sorted_chunks[0]
    for chunk in sorted_chunks[1:]:
        result = merge(result, chunk)
    return result
```

### Code
```python
import multiprocessing as mp
import math

def merge(a, b):
    # Merge two sorted lists
    result = []
    i = j = 0
    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            result.append(a[i])
            i += 1
        else:
            result.append(b[j])
            j += 1
    result.extend(a[i:])
    result.extend(b[j:])
    return result

def sort_chunk(chunk):
    # Sort a chunk (can use any sorting algorithm)
    return sorted(chunk)

def parallel_merge_sort(arr, num_processes=None):
    if num_processes is None:
        num_processes = mp.cpu_count()

    # If array is small, sort directly
    if len(arr) <= 1:
        return arr

    # Split array into chunks
    chunk_size = math.ceil(len(arr) / num_processes)
    chunks = [arr[i:i+chunk_size] for i in range(0, len(arr), chunk_size)]

    # Sort chunks in parallel
    with mp.Pool(processes=num_processes) as pool:
        sorted_chunks = pool.map(sort_chunk, chunks)

    # Merge sorted chunks sequentially
    result = sorted_chunks[0]
    for chunk in sorted_chunks[1:]:
        result = merge(result, chunk)
    return result

# Example usage
if __name__ == "__main__":  # Required for multiprocessing on Windows
    arr = [6, 5, 12, 10, 9, 1, 15, 3, 8, 7]
    sorted_arr = parallel_merge_sort(arr)
    print("Sorted:", sorted_arr)  # Output: [1, 3, 5, 6, 7, 8, 9, 10, 12, 15]
```

---

## 8. Merge Sort using Generators (Streaming Merge)

### Explanation
This variant demonstrates merging two sorted sequences lazily using generators. It's useful when dealing with large data streams that don't fit in memory. The merge function yields elements one by one from two sorted iterators.

### Approach
- Implement a generator `merge_gen` that takes two sorted iterators and yields the merged sequence.
- For sorting, we recursively split and merge, but the merge step is lazy. The recursion still builds lists, but we can combine with lazy evaluation.

### Algorithm
1. Base case: if list length <= 1, return iter(list).
2. Split list into two halves.
3. Recursively get generators for each half.
4. Return `merge_gen(left_gen, right_gen)` which yields merged elements lazily.

### Pseudocode
```
function merge_gen(a_iter, b_iter):
    a = next(a_iter), b = next(b_iter)
    while True:
        if a <= b:
            yield a
            a = next(a_iter)
        else:
            yield b
            b = next(b_iter)
    # handle exhaustion...

function mergeSortGen(arr):
    if len(arr) <= 1: return iter(arr)
    mid = len(arr)//2
    left = mergeSortGen(arr[:mid])
    right = mergeSortGen(arr[mid:])
    return merge_gen(left, right)
```

### Code
```python
def merge_gen(gen_a, gen_b):
    # Get first elements from both generators
    try:
        a = next(gen_a)
    except StopIteration:
        # If one is empty, yield all from the other
        yield from gen_b
        return
    try:
        b = next(gen_b)
    except StopIteration:
        yield a
        yield from gen_a
        return

    # Merge loop
    while True:
        if a <= b:
            yield a
            try:
                a = next(gen_a)
            except StopIteration:
                yield b
                yield from gen_b
                break
        else:
            yield b
            try:
                b = next(gen_b)
            except StopIteration:
                yield a
                yield from gen_a
                break

def merge_sort_gen(arr):
    if len(arr) <= 1:
        # Return generator that yields the single element
        yield from arr
        return

    mid = len(arr) // 2
    left_gen = merge_sort_gen(arr[:mid])
    right_gen = merge_sort_gen(arr[mid:])

    # Merge the two generators lazily
    yield from merge_gen(left_gen, right_gen)

# Example usage
arr = [6, 5, 12, 10, 9, 1]
sorted_gen = merge_sort_gen(arr)
sorted_list = list(sorted_gen)
print("Sorted:", sorted_list)  # Output: [1, 5, 6, 9, 10, 12]
```

---

## 9. Three-Way Merge Sort

### Explanation
Instead of splitting into two halves, three-way merge sort divides the array into three parts, recursively sorts each, and then merges three sorted lists. This reduces the recursion depth but increases the merge complexity.

### Approach
- Split into three roughly equal parts: indices `s..m1`, `m1+1..m2`, `m2+1..e`.
- Recursively sort each part.
- Merge three sorted lists using a priority queue or manual comparisons.

### Algorithm
1. If `s >= e`, return.
2. Compute two midpoints: `mid1 = s + (e-s)//3`, `mid2 = s + 2*(e-s)//3`.
3. Recursively sort three subarrays.
4. Merge three sorted lists: compare the smallest among the three, advance.

### Pseudocode
```
function merge3(arr, s, m1, m2, e):
    temp = []
    i = s, j = m1+1, k = m2+1
    while i <= m1 and j <= m2 and k <= e:
        smallest = min(arr[i], arr[j], arr[k])
        if smallest == arr[i]: i++
        elif smallest == arr[j]: j++
        else: k++
        temp.append(smallest)
    // handle remaining pairs...
    // copy back

function mergeSort3(arr, s, e):
    if s >= e: return
    m1 = s + (e-s)//3
    m2 = s + 2*(e-s)//3
    mergeSort3(arr, s, m1)
    mergeSort3(arr, m1+1, m2)
    mergeSort3(arr, m2+1, e)
    merge3(arr, s, m1, m2, e)
```

### Code
```python
def merge3(arr, s, m1, m2, e):
    # Create temporary list for merged result
    temp = []
    i, j, k = s, m1+1, m2+1

    # Merge three lists
    while i <= m1 and j <= m2 and k <= e:
        if arr[i] <= arr[j] and arr[i] <= arr[k]:
            temp.append(arr[i])
            i += 1
        elif arr[j] <= arr[i] and arr[j] <= arr[k]:
            temp.append(arr[j])
            j += 1
        else:
            temp.append(arr[k])
            k += 1

    # Merge remaining two lists
    while i <= m1 and j <= m2:
        if arr[i] <= arr[j]:
            temp.append(arr[i])
            i += 1
        else:
            temp.append(arr[j])
            j += 1

    while i <= m1 and k <= e:
        if arr[i] <= arr[k]:
            temp.append(arr[i])
            i += 1
        else:
            temp.append(arr[k])
            k += 1

    while j <= m2 and k <= e:
        if arr[j] <= arr[k]:
            temp.append(arr[j])
            j += 1
        else:
            temp.append(arr[k])
            k += 1

    # Copy remaining single list
    while i <= m1:
        temp.append(arr[i])
        i += 1
    while j <= m2:
        temp.append(arr[j])
        j += 1
    while k <= e:
        temp.append(arr[k])
        k += 1

    # Copy back to original array
    for idx, val in enumerate(temp):
        arr[s + idx] = val

def merge_sort_3way(arr, s, e):
    if s >= e:
        return
    # Calculate two midpoints for three-way split
    m1 = s + (e - s) // 3
    m2 = s + 2 * (e - s) // 3
    merge_sort_3way(arr, s, m1)
    merge_sort_3way(arr, m1+1, m2)
    merge_sort_3way(arr, m2+1, e)
    merge3(arr, s, m1, m2, e)

def sort(arr):
    merge_sort_3way(arr, 0, len(arr)-1)

# Example usage
arr = [6, 5, 12, 10, 9, 1]
sort(arr)
print("Sorted:", arr)  # Output: [1, 5, 6, 9, 10, 12]
```

---

## 10. Hybrid Merge Sort with Insertion Sort for Small Subarrays

### Explanation
For small arrays, insertion sort has lower overhead than recursive merge sort. This hybrid approach uses insertion sort when the subarray size falls below a threshold (e.g., 15). This improves performance in practice.

### Approach
- In the recursive function, if `e - s + 1 <= THRESHOLD`, use insertion sort on that subarray.
- Otherwise, split and merge as usual.

### Algorithm
1. If subarray size <= THRESHOLD: sort with insertion sort.
2. Else:
   - Find middle.
   - Recursively sort left and right.
   - Merge.

### Pseudocode
```
function insertionSort(arr, s, e):
    for i = s+1 to e:
        key = arr[i]
        j = i-1
        while j >= s and arr[j] > key:
            arr[j+1] = arr[j]
            j--
        arr[j+1] = key

function mergeSortHybrid(arr, s, e):
    if s >= e: return
    if e - s + 1 <= THRESHOLD:
        insertionSort(arr, s, e)
        return
    m = (s+e)//2
    mergeSortHybrid(arr, s, m)
    mergeSortHybrid(arr, m+1, e)
    merge(arr, s, m, e)
```

### Code
```python
THRESHOLD = 5  # Small value for demonstration

def insertion_sort(arr, s, e):
    # Sort subarray arr[s..e] using insertion sort
    for i in range(s+1, e+1):
        key = arr[i]
        j = i - 1
        while j >= s and arr[j] > key:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = key

def merge(arr, s, m, e):
    # Standard merge using temporary slices (as in variant 1)
    left = arr[s:m+1]
    right = arr[m+1:e+1]
    i = j = 0
    k = s
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            arr[k] = left[i]
            i += 1
        else:
            arr[k] = right[j]
            j += 1
        k += 1
    while i < len(left):
        arr[k] = left[i]
        i += 1
        k += 1
    while j < len(right):
        arr[k] = right[j]
        j += 1
        k += 1

def merge_sort_hybrid(arr, s, e):
    if s >= e:
        return
    # If subarray is small, use insertion sort
    if e - s + 1 <= THRESHOLD:
        insertion_sort(arr, s, e)
        return
    m = (s + e) // 2
    merge_sort_hybrid(arr, s, m)
    merge_sort_hybrid(arr, m+1, e)
    merge(arr, s, m, e)

def sort(arr):
    merge_sort_hybrid(arr, 0, len(arr)-1)

# Example usage
arr = [6, 5, 12, 10, 9, 1, 15, 3, 8, 7]
sort(arr)
print("Sorted:", arr)  # Output: [1, 3, 5, 6, 7, 8, 9, 10, 12, 15]
```