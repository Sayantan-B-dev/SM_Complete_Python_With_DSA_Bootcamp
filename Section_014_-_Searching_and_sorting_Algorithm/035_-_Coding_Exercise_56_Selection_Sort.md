## Selection Sort: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Selection sort is a comparison-based sorting algorithm that divides the list into two parts: a sorted sublist built from left to right and an unsorted sublist containing the remaining elements. It repeatedly selects the smallest (or largest) element from the unsorted portion and swaps it with the first element of the unsorted portion, growing the sorted sublist.

**Approach:**  
- Start with the first position as the current minimum.  
- Scan the rest of the list to find the actual minimum.  
- Swap the found minimum with the element at the current position.  
- Move the boundary between sorted and unsorted one step to the right.  
- Repeat until the entire list is sorted.

**Algorithm:**  
1. Set `n = length(array)`.  
2. For `i = 0` to `n-1`:  
   - Set `min_index = i`.  
   - For `j = i+1` to `n-1`:  
     - If `array[j] < array[min_index]`, update `min_index = j`.  
   - If `min_index != i`, swap `array[i]` and `array[min_index]`.  
3. Return sorted array.

**Pseudocode:**  
```
procedure selectionSort(arr):
    n = length(arr)
    for i = 0 to n-1:
        minIndex = i
        for j = i+1 to n-1:
            if arr[j] < arr[minIndex]:
                minIndex = j
        if minIndex != i:
            swap(arr[i], arr[minIndex])
    return arr
```

---

## Basic Selection Sort (As Given)

```python
def selection_sort_basic(lst):
    n = len(lst)
    for i in range(n):
        # Assume the current position holds the minimum
        min_index = i
        # Find the actual minimum in the unsorted part
        for j in range(i + 1, n):
            if lst[j] < lst[min_index]:
                min_index = j          # update min_index with new minimum
        # Swap the found minimum with the element at i
        temp = lst[i]
        lst[i] = lst[min_index]
        lst[min_index] = temp
    return lst

# Example call
arr = [64, 25, 12, 22, 11]
sorted_arr = selection_sort_basic(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [11, 12, 22, 25, 64]
```

---

## Optimized Selection Sort (Skip Swap if Same Index)

```python
def selection_sort_optimized(lst):
    n = len(lst)
    for i in range(n):
        min_index = i
        for j in range(i + 1, n):
            if lst[j] < lst[min_index]:
                min_index = j
        # Special: only swap if min_index changed
        if min_index != i:
            lst[i], lst[min_index] = lst[min_index], lst[i]   # Pythonic swap
    return lst

# Example call
arr = [64, 25, 12, 22, 11]
sorted_arr = selection_sort_optimized(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [11, 12, 22, 25, 64]
```

---

## Recursive Selection Sort

```python
def selection_sort_recursive(lst, start=0):
    n = len(lst)
    # Base case: if start reaches end, list is sorted
    if start >= n - 1:
        return lst
    # Find minimum index from start to end
    min_index = start
    for j in range(start + 1, n):
        if lst[j] < lst[min_index]:
            min_index = j
    # Swap the found minimum with element at start
    if min_index != start:
        lst[start], lst[min_index] = lst[min_index], lst[start]
    # Recursive call on the remaining unsorted part
    return selection_sort_recursive(lst, start + 1)

# Example call
arr = [64, 25, 12, 22, 11]
sorted_arr = selection_sort_recursive(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [11, 12, 22, 25, 64]
```

---

## Selection Sort with Custom Comparator

```python
def selection_sort_custom(lst, compare):
    n = len(lst)
    for i in range(n):
        min_index = i
        for j in range(i + 1, n):
            # Main logic: use custom comparator to determine order
            if compare(lst[j], lst[min_index]) < 0:
                min_index = j
        if min_index != i:
            lst[i], lst[min_index] = lst[min_index], lst[i]
    return lst

# Custom comparator: sort by absolute value
def compare_abs(a, b):
    return abs(a) - abs(b)

# Example call
arr = [ -5, 3, -1, 2, -8 ]
sorted_arr = selection_sort_custom(arr, compare_abs)
print("Sorted by absolute value:", sorted_arr)   # Output: Sorted by absolute value: [-1, 2, 3, -5, -8]
```

---

## Stable Selection Sort

Stable selection sort preserves the relative order of equal elements by inserting the minimum instead of swapping (which can destabilize).

```python
def selection_sort_stable(lst):
    n = len(lst)
    for i in range(n):
        min_index = i
        for j in range(i + 1, n):
            if lst[j] < lst[min_index]:
                min_index = j
        # Special: if min_index is not i, perform a rotation to maintain stability
        if min_index != i:
            # Save the minimum value
            min_val = lst[min_index]
            # Shift elements from i to min_index-1 right by one
            for k in range(min_index, i, -1):
                lst[k] = lst[k-1]
            # Place the minimum at position i
            lst[i] = min_val
    return lst

# Example call
arr = [4, 2, 3, 4, 1]
sorted_arr = selection_sort_stable(arr)
print("Stably sorted:", sorted_arr)   # Output: Stably sorted: [1, 2, 3, 4, 4] (original order of 4s preserved)
```

---

## Double Selection Sort (Min-Max)

In each pass, find both the minimum and maximum and place them at the beginning and end simultaneously, reducing the number of passes by half.

```python
def selection_sort_min_max(lst):
    n = len(lst)
    for i in range(n // 2):
        min_index = i
        max_index = i
        # Scan the unsorted part for both min and max
        for j in range(i, n - i):
            if lst[j] < lst[min_index]:
                min_index = j
            if lst[j] > lst[max_index]:
                max_index = j
        # Swap min with position i
        if min_index != i:
            lst[i], lst[min_index] = lst[min_index], lst[i]
            # If max was at i, it got moved to min_index
            if max_index == i:
                max_index = min_index
        # Swap max with position n-i-1
        if max_index != n - i - 1:
            lst[n - i - 1], lst[max_index] = lst[max_index], lst[n - i - 1]
    return lst

# Example call
arr = [64, 25, 12, 22, 11]
sorted_arr = selection_sort_min_max(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [11, 12, 22, 25, 64]
```

---

## Selection Sort Using While Loop

```python
def selection_sort_while(lst):
    n = len(lst)
    i = 0
    while i < n:
        min_index = i
        j = i + 1
        while j < n:
            if lst[j] < lst[min_index]:
                min_index = j
            j += 1
        if min_index != i:
            lst[i], lst[min_index] = lst[min_index], lst[i]
        i += 1
    return lst

# Example call
arr = [64, 25, 12, 22, 11]
sorted_arr = selection_sort_while(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [11, 12, 22, 25, 64]
```

---

## Selection Sort in Descending Order

```python
def selection_sort_descending(lst):
    n = len(lst)
    for i in range(n):
        # Find maximum instead of minimum
        max_index = i
        for j in range(i + 1, n):
            if lst[j] > lst[max_index]:
                max_index = j
        if max_index != i:
            lst[i], lst[max_index] = lst[max_index], lst[i]
    return lst

# Example call
arr = [64, 25, 12, 22, 11]
sorted_arr = selection_sort_descending(arr)
print("Descending order:", sorted_arr)   # Output: Descending order: [64, 25, 22, 12, 11]
```

---

## Selection Sort for Strings (Lexicographic Order)

```python
def selection_sort_strings(lst):
    n = len(lst)
    for i in range(n):
        min_index = i
        for j in range(i + 1, n):
            # Main logic: compare strings lexicographically
            if lst[j] < lst[min_index]:
                min_index = j
        if min_index != i:
            lst[i], lst[min_index] = lst[min_index], lst[i]
    return lst

# Example call
arr = ["banana", "apple", "cherry", "date"]
sorted_arr = selection_sort_strings(arr)
print("Sorted strings:", sorted_arr)   # Output: Sorted strings: ['apple', 'banana', 'cherry', 'date']
```