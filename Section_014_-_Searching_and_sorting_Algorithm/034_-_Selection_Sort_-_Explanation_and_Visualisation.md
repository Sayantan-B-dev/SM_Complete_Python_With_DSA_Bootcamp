Selection Sort is a simple comparison-based sorting algorithm. It works by repeatedly finding the minimum element from the unsorted part of the array and moving it to the beginning. The algorithm maintains two subarrays: the sorted subarray and the unsorted subarray. In each iteration, the smallest element from the unsorted part is selected and swapped with the leftmost unsorted element, thereby expanding the sorted subarray by one.

## Algorithm Steps

1. Start with the entire array as unsorted.
2. For each position i from 0 to n-2 (where n is the length of the array):
   - Assume the element at index i is the minimum.
   - Scan the unsorted part (from i+1 to n-1) to find the index of the actual minimum element.
   - If a smaller element is found, update the minimum index.
   - After scanning, swap the element at i with the element at the minimum index (if they are different).
3. After each pass, the element at position i is in its correct sorted position.
4. Repeat until the entire array is sorted.

## Pseudocode

```
procedure selectionSort(A: list of sortable items)
    n = length(A)
    for i = 0 to n-2
        minIndex = i
        for j = i+1 to n-1
            if A[j] < A[minIndex] then
                minIndex = j
            end if
        end for
        if minIndex != i then
            swap A[i] and A[minIndex]
        end if
    end for
end procedure
```

## Example with Visualization

Consider an array: [64, 25, 12, 22, 11]

**Pass 1 (i=0):**
- Find minimum in indices 0..4. Minimum is 11 at index 4.
- Swap 64 and 11.
- Array becomes: [11, 25, 12, 22, 64]

**Pass 2 (i=1):**
- Find minimum in indices 1..4. Minimum is 12 at index 2.
- Swap 25 and 12.
- Array becomes: [11, 12, 25, 22, 64]

**Pass 3 (i=2):**
- Find minimum in indices 2..4. Minimum is 22 at index 3.
- Swap 25 and 22.
- Array becomes: [11, 12, 22, 25, 64]

**Pass 4 (i=3):**
- Find minimum in indices 3..4. Minimum is 25 at index 3 (already in place).
- No swap needed.
- Array remains: [11, 12, 22, 25, 64]

**Pass 5 (i=4) is not needed because the last element is automatically sorted.**

Visual representation of each pass (sorted part shown in bold, unsorted in plain):

Initial: [64, 25, 12, 22, 11]  
Pass 1:  **[11]**, 25, 12, 22, 64  
Pass 2:  **[11, 12]**, 25, 22, 64  
Pass 3:  **[11, 12, 22]**, 25, 64  
Pass 4:  **[11, 12, 22, 25]**, 64  
Final:   **[11, 12, 22, 25, 64]**

## Complexity Analysis

- **Time Complexity:**
  - Best case: O(n²) – even if the array is already sorted, it still performs the comparisons.
  - Average case: O(n²)
  - Worst case: O(n²)
- **Space Complexity:** O(1) – it sorts in place, using only a constant amount of extra memory.
- **Stability:** Selection sort is not stable because swapping may change the relative order of equal elements.

## Advantages and Disadvantages

**Advantages:**
- Simple to implement and understand.
- Performs well on small lists.
- In-place sorting (requires no additional memory).

**Disadvantages:**
- Inefficient on large lists due to O(n²) time complexity.
- Not adaptive – always runs in quadratic time regardless of input order.

Selection sort is often used for educational purposes or when the list is small and simplicity is more important than efficiency.