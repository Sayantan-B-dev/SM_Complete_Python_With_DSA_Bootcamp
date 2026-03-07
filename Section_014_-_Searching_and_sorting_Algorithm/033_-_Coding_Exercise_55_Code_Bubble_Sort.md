## Bubble Sort: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Bubble Sort is a simple comparison-based sorting algorithm. It repeatedly steps through a list, compares adjacent elements, and swaps them if they are in the wrong order. The pass through the list is repeated until no swaps are needed, indicating the list is sorted. Larger elements "bubble" to the end with each pass.

**Approach:**  
- Start at the beginning of the array.  
- Compare each pair of adjacent elements.  
- If the first is greater than the second, swap them.  
- After each full pass, the largest unsorted element is placed at its correct position at the end.  
- Repeat passes, each time ignoring the last sorted elements, until no swaps occur.

**Algorithm:**  
1. Set `n = length(array)`.  
2. Repeat for `i = 0` to `n-1`:  
   - Set `swapped = false`.  
   - Repeat for `j = 0` to `n-i-2`:  
     - If `array[j] > array[j+1]`, swap them and set `swapped = true`.  
   - If `swapped` is false, break.  
3. Array is sorted.

**Pseudocode:**  
```
procedure bubbleSort(arr):
    n = length(arr)
    for i = 0 to n-1:
        swapped = false
        for j = 0 to n-i-2:
            if arr[j] > arr[j+1]:
                swap(arr[j], arr[j+1])
                swapped = true
        if not swapped: break
```

---

## Basic Bubble Sort (Unoptimized)

```python
def bubble_sort_basic(arr):
    n = len(arr)
    # Outer loop for number of passes
    for i in range(n):
        # Inner loop for comparisons in each pass
        for j in range(0, n-i-1):
            # Main logic: compare adjacent elements
            if arr[j] > arr[j+1]:
                # Swap if out of order
                arr[j], arr[j+1] = arr[j+1], arr[j]
    return arr

# Example call
arr = [64, 34, 25, 12, 22, 11, 90]
sorted_arr = bubble_sort_basic(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [11, 12, 22, 25, 34, 64, 90]
```

---

## Optimized Bubble Sort with Early Exit

```python
def bubble_sort_optimized(arr):
    n = len(arr)
    for i in range(n):
        swapped = False                      # Flag to detect any swap in this pass
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True                # Swap occurred
        # Special: if no swaps, array is already sorted
        if not swapped:
            break                               # Early exit
    return arr

# Example call
arr = [1, 2, 3, 4, 5, 6]
sorted_arr = bubble_sort_optimized(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [1, 2, 3, 4, 5, 6] (only one pass)
```

---

## Recursive Bubble Sort

```python
def bubble_sort_recursive(arr, n=None):
    if n is None:
        n = len(arr)
    # Base case: if size is 1, it's sorted
    if n == 1:
        return arr
    # One pass of bubble sort to place largest element at end
    for i in range(n-1):
        if arr[i] > arr[i+1]:
            arr[i], arr[i+1] = arr[i+1], arr[i]
    # Recursive call on the remaining (n-1) elements
    return bubble_sort_recursive(arr, n-1)

# Example call
arr = [64, 34, 25, 12, 22, 11, 90]
sorted_arr = bubble_sort_recursive(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [11, 12, 22, 25, 34, 64, 90]
```

---

## Bubble Sort on a Singly Linked List

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def bubble_sort_linked_list(head):
    if not head or not head.next:
        return head
    swapped = True
    while swapped:
        swapped = False
        current = head
        while current and current.next:
            # Compare adjacent nodes
            if current.data > current.next.data:
                # Swap data values (or swap nodes themselves)
                current.data, current.next.data = current.next.data, current.data
                swapped = True
            current = current.next
    return head

# Helper to create list and print
def create_list(arr):
    if not arr: return None
    head = Node(arr[0])
    current = head
    for val in arr[1:]:
        current.next = Node(val)
        current = current.next
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> ")
        head = head.next
    print("None")

# Example call
arr = [64, 34, 25, 12, 22, 11, 90]
head = create_list(arr)
sorted_head = bubble_sort_linked_list(head)
print("Sorted linked list: ", end="")
print_list(sorted_head)   # Output: 11 -> 12 -> 22 -> 25 -> 34 -> 64 -> 90 -> None
```

---

## Bidirectional Bubble Sort (Cocktail Shaker)

```python
def cocktail_shaker_sort(arr):
    n = len(arr)
    swapped = True
    start = 0
    end = n - 1
    while swapped:
        swapped = False
        # Forward pass: bubble largest to the end
        for i in range(start, end):
            if arr[i] > arr[i+1]:
                arr[i], arr[i+1] = arr[i+1], arr[i]
                swapped = True
        if not swapped:
            break
        swapped = False
        end -= 1
        # Backward pass: bubble smallest to the beginning
        for i in range(end-1, start-1, -1):
            if arr[i] > arr[i+1]:
                arr[i], arr[i+1] = arr[i+1], arr[i]
                swapped = True
        start += 1
    return arr

# Example call
arr = [5, 1, 4, 2, 8, 0, 2]
sorted_arr = cocktail_shaker_sort(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [0, 1, 2, 2, 4, 5, 8]
```

---

## Bubble Sort with Custom Comparator (Sorting by String Length)

```python
def bubble_sort_custom(arr, compare):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n-i-1):
            # Main logic: use custom comparator function
            if compare(arr[j], arr[j+1]) > 0:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped:
            break
    return arr

# Custom comparator: compare by string length
def compare_by_length(a, b):
    return len(a) - len(b)

# Example call
arr = ["apple", "pie", "banana", "kiwi", "strawberry"]
sorted_arr = bubble_sort_custom(arr, compare_by_length)
print("Sorted by length:", sorted_arr)   # Output: Sorted by length: ['pie', 'kiwi', 'apple', 'banana', 'strawberry']
```

---

## Bubble Sort in Descending Order

```python
def bubble_sort_descending(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n-i-1):
            # Main logic: swap if current is less than next (for descending)
            if arr[j] < arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped:
            break
    return arr

# Example call
arr = [64, 34, 25, 12, 22, 11, 90]
sorted_arr = bubble_sort_descending(arr)
print("Descending order:", sorted_arr)   # Output: Descending order: [90, 64, 34, 25, 22, 12, 11]
```

---

## Bubble Sort Using While Loop (No For)

```python
def bubble_sort_while(arr):
    n = len(arr)
    i = 0
    while i < n:
        swapped = False
        j = 0
        while j < n - i - 1:
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
            j += 1
        if not swapped:
            break
        i += 1
    return arr

# Example call
arr = [64, 34, 25, 12, 22, 11, 90]
sorted_arr = bubble_sort_while(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [11, 12, 22, 25, 34, 64, 90]
```

---

## Bubble Sort with Step-by-Step Visualization

```python
def bubble_sort_verbose(arr):
    n = len(arr)
    print("Initial array:", arr)
    for i in range(n):
        swapped = False
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        # Special: print after each pass
        print(f"After pass {i+1}: {arr}")
        if not swapped:
            print("No swaps, array is sorted.")
            break
    return arr

# Example call
arr = [5, 1, 4, 2, 8]
sorted_arr = bubble_sort_verbose(arr)
# Output shows each pass
```

---

## Bubble Sort for Strings (Lexicographic Order)

```python
def bubble_sort_strings(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n-i-1):
            # Main logic: compare strings lexicographically
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped:
            break
    return arr

# Example call
arr = ["banana", "apple", "cherry", "date"]
sorted_arr = bubble_sort_strings(arr)
print("Sorted strings:", sorted_arr)   # Output: Sorted strings: ['apple', 'banana', 'cherry', 'date']
```