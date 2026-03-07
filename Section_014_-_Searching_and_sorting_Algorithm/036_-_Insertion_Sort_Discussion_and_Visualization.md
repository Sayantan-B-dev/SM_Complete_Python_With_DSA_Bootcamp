## Insertion Sort: Explanation and Visualisation

### What is Insertion Sort?
Insertion sort is a simple comparison-based sorting algorithm that builds the final sorted array (or list) one element at a time. It is much less efficient on large lists than more advanced algorithms such as quicksort, heapsort, or merge sort. However, it has several advantages:
- Simple implementation.
- Efficient for small data sets.
- Adaptive: efficient for data sets that are already substantially sorted (O(n) in best case).
- Stable: does not change the relative order of elements with equal keys.
- In-place: only requires a constant amount O(1) of additional memory space.

### How Insertion Sort Works
The idea is analogous to the way people sort a hand of playing cards. We start with an empty left hand (the sorted part) and the cards to be sorted placed on the table (the unsorted part). We then remove one card at a time from the table and insert it into the correct position in the left hand. To find the correct position, we compare the new card with already sorted cards, moving from right to left until we find a card smaller than or equal to the new card.

In terms of an array, insertion sort divides the array into two regions: a sorted region and an unsorted region. Initially, the sorted region consists of the first element (a single element is trivially sorted). The rest of the array is the unsorted region. The algorithm then iterates through the unsorted region, taking each element and inserting it into its correct position within the sorted region, shifting larger elements to the right as necessary.

### Algorithm Steps
Given an array `arr` of `n` elements (0-indexed):
1. Start with the first element (index 0) considered as the sorted portion.
2. For each element from index `i = 1` to `n-1` (the next element to insert):
   a. Store the current element `key = arr[i]`.
   b. Set `j = i - 1` (the last index of the sorted portion).
   c. While `j >= 0` and `arr[j] > key`:
      - Shift `arr[j]` one position to the right: `arr[j+1] = arr[j]`.
      - Decrement `j` by 1.
   d. Place `key` into its correct position: `arr[j+1] = key`.
3. After processing all elements, the array is sorted.

### Pseudocode
```
INSERTION-SORT(A)
  for i = 1 to A.length - 1
    key = A[i]
    // Insert A[i] into the sorted sequence A[0..i-1]
    j = i - 1
    while j >= 0 and A[j] > key
      A[j + 1] = A[j]
      j = j - 1
    A[j + 1] = key
```

### Visualisation (Step-by-Step Example)
Consider sorting the array `[5, 2, 4, 6, 1, 3]` using insertion sort.

**Initial array:** [5, 2, 4, 6, 1, 3]
- Sorted portion: [5]
- Unsorted: [2, 4, 6, 1, 3]

**Step 1 (i=1, key=2):**
- Compare 2 with 5 → 5 > 2, shift 5 right.
- Array becomes: [5, 5, 4, 6, 1, 3] (temporarily)
- Insert 2 at position 0: [2, 5, 4, 6, 1, 3]
- Sorted: [2, 5], unsorted: [4, 6, 1, 3]

**Step 2 (i=2, key=4):**
- Compare 4 with 5 → 5 > 4, shift 5 right.
- Array: [2, 5, 5, 6, 1, 3]
- Compare 4 with 2 → 2 ≤ 4, stop shifting.
- Insert 4 at position 1: [2, 4, 5, 6, 1, 3]
- Sorted: [2, 4, 5], unsorted: [6, 1, 3]

**Step 3 (i=3, key=6):**
- Compare 6 with 5 → 5 ≤ 6, no shift.
- Insert 6 at same position (no change): [2, 4, 5, 6, 1, 3]
- Sorted: [2, 4, 5, 6], unsorted: [1, 3]

**Step 4 (i=4, key=1):**
- Compare 1 with 6 → 6 > 1, shift 6 right.
- Array: [2, 4, 5, 6, 6, 3]
- Compare 1 with 5 → 5 > 1, shift 5 right.
- Array: [2, 4, 5, 5, 6, 3]
- Compare 1 with 4 → 4 > 1, shift 4 right.
- Array: [2, 4, 4, 5, 6, 3]
- Compare 1 with 2 → 2 > 1, shift 2 right.
- Array: [2, 2, 4, 5, 6, 3]
- j becomes -1, exit loop.
- Insert 1 at position 0: [1, 2, 4, 5, 6, 3]
- Sorted: [1, 2, 4, 5, 6], unsorted: [3]

**Step 5 (i=5, key=3):**
- Compare 3 with 6 → 6 > 3, shift 6 right.
- Array: [1, 2, 4, 5, 6, 6]
- Compare 3 with 5 → 5 > 3, shift 5 right.
- Array: [1, 2, 4, 5, 5, 6]
- Compare 3 with 4 → 4 > 3, shift 4 right.
- Array: [1, 2, 4, 4, 5, 6]
- Compare 3 with 2 → 2 ≤ 3, stop.
- Insert 3 at position 2: [1, 2, 3, 4, 5, 6]

**Final sorted array:** [1, 2, 3, 4, 5, 6]

### Complexity Analysis
- **Time Complexity:**
  - **Best case:** O(n) – occurs when the array is already sorted. In each iteration, the inner while loop condition fails immediately.
  - **Average case:** O(n²) – on average, about half of the elements in the sorted portion are shifted.
  - **Worst case:** O(n²) – occurs when the array is sorted in reverse order. For each i, the inner loop runs i times.
- **Space Complexity:** O(1) – only a constant amount of extra space (for key and loop variables) is used.
- **Stability:** Stable – equal elements retain their original order because we stop shifting when `arr[j] <= key`.
- **Adaptive:** Efficient for partially sorted arrays.

### When to Use Insertion Sort
- Small arrays (typically fewer than 50 elements).
- Arrays that are already mostly sorted.
- As part of hybrid sorting algorithms (e.g., Timsort uses insertion sort for small runs).
- When memory is limited (in-place sorting).

Insertion sort is a fundamental algorithm that provides a clear introduction to sorting concepts and is often taught as a building block for more advanced techniques.