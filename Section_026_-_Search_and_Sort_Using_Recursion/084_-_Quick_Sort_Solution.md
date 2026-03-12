# Quick Sort (Python)

## 1. Definition and Concept Overview

QuickSort is a divide‑and‑conquer sorting algorithm. It selects an element as a *pivot* and partitions the array around the pivot such that elements smaller than the pivot come before it and elements greater come after it. The two subarrays (left of pivot, right of pivot) are then recursively sorted. The algorithm works in‑place and is widely used due to its good average‑case performance.

## 2. Core Principles and Internal Mechanics

- **Pivot selection**: Any element can be chosen as pivot (first, last, median, random). Choice affects performance.  
- **Partitioning**: Rearranges the array so that all elements less than the pivot are placed before it, and all greater are placed after it. After partitioning, the pivot is in its final sorted position.  
- **Recursion**: The left and right subarrays (excluding the pivot) are sorted independently using the same logic.  
- **In‑place operation**: QuickSort typically sorts the array without requiring significant extra memory (only recursion stack).  

Two common partition schemes are:

- **Lomuto partition**: Uses a single scan; simpler but less efficient when many duplicates exist.  
- **Hoare partition**: Uses two pointers moving toward each other; generally faster and does fewer swaps.

## 3. Step‑by‑Step Logical Breakdown

### 3.1 Algorithm in Plain English

1. If the array segment has 0 or 1 element, it is already sorted – return.  
2. Choose a pivot element (e.g., the last element of the segment).  
3. Partition the segment: reorder elements so that all items less than the pivot come before it, and all greater come after it. After partitioning, the pivot is at its correct final index.  
4. Recursively apply QuickSort to the subarray left of the pivot (indices less than pivot index) and to the subarray right of the pivot (indices greater than pivot index).  

### 3.2 Partitioning Mechanics (Lomuto Scheme)

Given an array `arr` and segment `[low, high]` with pivot chosen as `arr[high]`:

- Initialize a pointer `i = low` (tracks the boundary of elements less than pivot).  
- Iterate `j` from `low` to `high-1`:  
  - If `arr[j] < pivot`, swap `arr[i]` and `arr[j]`, then increment `i`.  
- After the loop, swap `arr[i]` and `arr[high]` (place pivot in correct position).  
- Return `i` (the final pivot index).

This ensures that after the loop, all elements before index `i` are `< pivot`, and elements from `i` to `high-1` are `>= pivot`. Swapping pivot with `arr[i]` places it between the two groups.

### 3.3 Diagram View (Example)

Array: `[3, 6, 7, 2, 1, 4, 5]`, pivot = last element `5`.

**Before partition**:  
`[3, 6, 7, 2, 1, 4, 5]`  
`low=0, high=6, pivot=5, i=0`

**Scan (j from 0 to 5):**  
- j=0, arr[0]=3 <5 → swap arr[0] with arr[0] (no change), i=1  
- j=1, arr[1]=6 not <5 → no swap  
- j=2, arr[2]=7 not <5 → no swap  
- j=3, arr[3]=2 <5 → swap arr[1] with arr[3] → `[3,2,7,6,1,4,5]`, i=2  
- j=4, arr[4]=1 <5 → swap arr[2] with arr[4] → `[3,2,1,6,7,4,5]`, i=3  
- j=5, arr[5]=4 <5 → swap arr[3] with arr[5] → `[3,2,1,4,7,6,5]`, i=4  

After loop, swap arr[i] (arr[4]=7) with pivot (arr[6]=5) → `[3,2,1,4,5,6,7]`.  
Pivot index = 4. Left subarray `[3,2,1,4]` and right subarray `[6,7]` are then sorted recursively.

## 4. Implementation (Python)

Below is an implementation using the Lomuto partition scheme. Inline comments explain the reasoning.

```python
def partition_lomuto(arr, low, high):
    """
    Lomuto partition scheme.
    Pivot is chosen as the last element arr[high].
    Returns the final index of the pivot.
    """
    pivot = arr[high]
    i = low  # i will track the boundary of elements < pivot

    for j in range(low, high):
        # If current element is smaller than pivot, swap it to the left side
        if arr[j] < pivot:
            arr[i], arr[j] = arr[j], arr[i]
            i += 1

    # Place pivot in its correct position (between smaller and larger elements)
    arr[i], arr[high] = arr[high], arr[i]
    return i


def quicksort_helper(arr, low, high):
    """
    Recursive helper for QuickSort.
    """
    if low >= high:
        # Base case: segment with 0 or 1 element is already sorted
        return

    # Partition the array and get the pivot index
    p = partition_lomuto(arr, low, high)

    # Recursively sort elements before and after pivot
    quicksort_helper(arr, low, p - 1)
    quicksort_helper(arr, p + 1, high)


def quicksort(arr):
    """
    Public interface for QuickSort.
    """
    quicksort_helper(arr, 0, len(arr) - 1)
```

### 4.1 Alternative Partition (User‑Provided Code)

The following code implements a two‑pass partition: first it counts elements smaller than pivot to determine the pivot's correct position, then it rearranges elements around that position using two pointers. This approach is valid but less common; it requires two scans of the segment.

```python
def partition_two_pass(arr, low, high):
    pivot = arr[high]
    # First pass: count how many elements are smaller than pivot
    smaller_count = 0
    for i in range(low, high):
        if arr[i] < pivot:
            smaller_count += 1
    # Place pivot at its correct position
    pivot_index = low + smaller_count
    arr[pivot_index], arr[high] = arr[high], arr[pivot_index]

    # Second pass: ensure all elements left of pivot are < pivot, all right are >= pivot
    i, j = low, high
    while i < pivot_index and j > pivot_index:
        if arr[i] < pivot:
            i += 1
        elif arr[j] >= pivot:
            j -= 1
        else:
            arr[i], arr[j] = arr[j], arr[i]
            i += 1
            j -= 1
    return pivot_index
```

## 5. Time and Space Complexity Analysis

- **Best‑case time**: `O(n log n)` – occurs when the pivot consistently divides the array into two nearly equal halves.  
- **Average‑case time**: `O(n log n)` – for random pivot selection, the expected depth is logarithmic.  
- **Worst‑case time**: `O(n²)` – occurs when the pivot is always the smallest or largest element (e.g., already sorted array with first/last pivot). This degenerates to `n + (n-1) + ... + 1` comparisons.  
- **Space complexity**:  
  - In‑place sorting uses `O(log n)` auxiliary space for the recursion stack (worst case `O(n)` if unbalanced).  
  - No significant additional memory for arrays (ignoring recursion).

## 6. Edge Cases and Failure Scenarios

- **Empty array**: `quicksort([])` – base case triggers, no changes.  
- **Single element**: `quicksort([x])` – already sorted, returns unchanged.  
- **Already sorted array**: With naive pivot (first or last), worst‑case `O(n²)` occurs. Mitigation: use random pivot or median‑of‑three.  
- **All equal elements**: Lomuto partition may produce highly unbalanced splits (all elements equal → pivot index always at one end). Some implementations handle this by using a three‑way partition (Dutch national flag).  
- **Duplicate values**: The algorithm is not stable; relative order of equal elements may change.  
- **Very large arrays**: Recursion depth may exceed Python's recursion limit (default ~1000). For large datasets, an iterative version or increasing recursion limit may be needed.  
- **Pivot selection risks**: Deterministic pivot choices can lead to worst‑case performance on specific inputs. Randomization reduces probability of worst case.

## 7. Practical Use Cases

- **General‑purpose sorting**: QuickSort is often the default sorting algorithm in many language libraries (e.g., C's `qsort`, though it may be implemented as introsort which switches to heapsort to avoid worst case).  
- **In‑memory sorting**: When data fits in RAM, QuickSort's low constant factors and in‑place nature make it efficient.  
- **Sorting with custom comparators**: Easy to adapt.  
- **Selection algorithms**: The partition logic is also used to find the k‑th smallest element (quickselect).  
- **Parallel sorting**: The divide step can be parallelized across multiple processors.

## 8. Limitations and Trade‑offs

- **Worst‑case time**: `O(n²)` can occur with poor pivot choices. Mitigations (random pivot, median‑of‑three, introsort) add overhead but guarantee `O(n log n)` performance.  
- **Not stable**: If stability is required (e.g., sorting by multiple keys), merge sort is preferable.  
- **Recursive depth**: Deep recursion can cause stack overflow; iterative implementations or tail recursion optimisations may be necessary for constrained environments.  
- **Performance on small arrays**: For very small subarrays, recursion overhead may dominate. Many optimized implementations switch to insertion sort for sizes below a threshold (e.g., 10–20).  
- **Hardware considerations**: QuickSort exhibits good cache locality during partitioning, but worst‑case can cause thrashing.