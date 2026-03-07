Bubble Sort is a simple comparison-based sorting algorithm. It works by repeatedly stepping through a list, comparing adjacent elements, and swapping them if they are in the wrong order. The process is repeated until the entire list is sorted. The name "bubble sort" comes from the way larger elements "bubble" to the end of the list with each pass.

## How Bubble Sort Works

- Start at the beginning of the list.
- Compare the first two elements. If the first is greater than the second, swap them.
- Move to the next pair (second and third), compare and swap if necessary.
- Continue this process for the entire list. After the first pass, the largest element will have "bubbled up" to the last position.
- Repeat the process for the remaining unsorted portion of the list (excluding the last sorted element). After each pass, the next largest element settles into its correct position.
- The algorithm terminates when a complete pass makes no swaps, indicating the list is sorted.

## Visualization Example

Consider the array: [5, 1, 4, 2, 8]

**Pass 1:**
- Compare 5 and 1 → swap → [1, 5, 4, 2, 8]
- Compare 5 and 4 → swap → [1, 4, 5, 2, 8]
- Compare 5 and 2 → swap → [1, 4, 2, 5, 8]
- Compare 5 and 8 → no swap → [1, 4, 2, 5, 8]
End of pass 1: largest element 8 is in place.

**Pass 2:**
- Compare 1 and 4 → no swap → [1, 4, 2, 5, 8]
- Compare 4 and 2 → swap → [1, 2, 4, 5, 8]
- Compare 4 and 5 → no swap → [1, 2, 4, 5, 8]
- Compare 5 and 8 → no swap (but 8 is already sorted, so we could stop earlier)
End of pass 2: 5 is now in place.

**Pass 3:**
- Compare 1 and 2 → no swap → [1, 2, 4, 5, 8]
- Compare 2 and 4 → no swap → [1, 2, 4, 5, 8]
- Compare 4 and 5 → no swap (5 already sorted)
No swaps were made in this pass, so the array is sorted.

Final sorted array: [1, 2, 4, 5, 8]

## Pseudocode

Below is the pseudocode for the optimized version of bubble sort, which includes a flag to detect whether any swaps occurred in a pass.

```
procedure bubbleSort(A : list of sortable items)
    n = length(A)
    repeat
        swapped = false
        for i = 0 to n-2 inclusive do
            if A[i] > A[i+1] then
                swap(A[i], A[i+1])
                swapped = true
            end if
        end for
        n = n - 1      // each pass places the next largest element
    until not swapped
end procedure
```

Alternatively, a more traditional version without early termination:

```
procedure bubbleSort(A : list of sortable items)
    n = length(A)
    for i = 0 to n-1 inclusive do
        for j = 0 to n-i-2 inclusive do
            if A[j] > A[j+1] then
                swap(A[j], A[j+1])
            end if
        end for
    end for
end procedure
```

## Complexity Analysis

- **Worst-case time complexity**: O(n²) – occurs when the array is in reverse order. Each element must be compared and swapped many times.
- **Best-case time complexity**: O(n) – occurs when the array is already sorted. With the optimized version, only one pass is needed to verify no swaps.
- **Average-case time complexity**: O(n²)
- **Space complexity**: O(1) – only a constant amount of extra memory is used for temporary variables (in-place sorting).

## Stability

Bubble sort is a stable sorting algorithm. It does not change the relative order of elements with equal keys because swapping only occurs when an element is strictly greater than the next.

## Optimizations

The optimized pseudocode includes a flag (`swapped`) to break out of the loop early if the array becomes sorted before all passes are completed. This improves best-case performance to linear time. Another minor optimization is to reduce the range of the inner loop after each pass since the last elements are already in place.

## Conclusion

Bubble sort is an intuitive sorting algorithm often used for educational purposes due to its simplicity. However, its O(n²) average and worst-case time complexity makes it inefficient for large datasets. For practical applications, more efficient algorithms like quicksort, mergesort, or heapsort are preferred.