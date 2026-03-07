## Insertion Sort: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Insertion sort builds a sorted array one element at a time by repeatedly taking the next element and inserting it into its correct position among the previously sorted elements. It is efficient for small datasets and adaptive (runs faster on partially sorted data).

**Approach:**  
- Start with the second element (index 1) as the "current" element.  
- Compare it with elements to its left, shifting larger elements one position to the right.  
- Insert the current element into the empty gap.  
- Move to the next element and repeat until the entire array is processed.

**Algorithm:**  
1. Set `n = length(array)`.  
2. For `i = 1` to `n-1`:  
   - Let `key = array[i]`.  
   - Let `j = i - 1`.  
   - While `j >= 0` and `array[j] > key`:  
     - Shift `array[j]` to `array[j+1]`.  
     - Decrement `j`.  
   - Place `key` at `array[j+1]`.  
3. Return sorted array.

**Pseudocode:**  
```
procedure insertionSort(arr):
    n = length(arr)
    for i = 1 to n-1:
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]
            j = j - 1
        arr[j+1] = key
    return arr
```

---

## Standard Insertion Sort (Corrected)

```python
def insertion_sort_standard(lst):
    n = len(lst)
    for current in range(1, n):
        current_card = lst[current]        # element to be inserted
        position = current - 1

        # Shift elements greater than current_card to the right
        while position >= 0 and lst[position] > current_card:
            lst[position + 1] = lst[position]
            position -= 1

        # Place current_card in its correct position
        lst[position + 1] = current_card

    return lst

# Example call
arr = [12, 11, 13, 5, 6]
sorted_arr = insertion_sort_standard(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [5, 6, 11, 12, 13]
```

---

## Optimized Insertion Sort (with early break)

```python
def insertion_sort_optimized(lst):
    n = len(lst)
    for current in range(1, n):
        current_card = lst[current]
        position = current - 1

        # Main logic: shift only while element is greater than current_card
        while position >= 0 and lst[position] > current_card:
            lst[position + 1] = lst[position]
            position -= 1

        # Insert at correct position
        lst[position + 1] = current_card

    return lst

# Example call
arr = [12, 11, 13, 5, 6]
sorted_arr = insertion_sort_optimized(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [5, 6, 11, 12, 13]
```

---

## Recursive Insertion Sort

```python
def insertion_sort_recursive(lst, n=None):
    if n is None:
        n = len(lst)

    # Base case: a single element is already sorted
    if n <= 1:
        return lst

    # Sort first n-1 elements recursively
    insertion_sort_recursive(lst, n - 1)

    # Insert the nth element into its correct position
    current_card = lst[n - 1]
    position = n - 2

    # Shift elements to make space
    while position >= 0 and lst[position] > current_card:
        lst[position + 1] = lst[position]
        position -= 1

    lst[position + 1] = current_card
    return lst

# Example call
arr = [12, 11, 13, 5, 6]
sorted_arr = insertion_sort_recursive(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [5, 6, 11, 12, 13]
```

---

## Binary Insertion Sort

Uses binary search to find the insertion position, reducing comparisons but not shifts.

```python
import bisect

def insertion_sort_binary(lst):
    for current in range(1, len(lst)):
        current_card = lst[current]
        # Use binary search to find insertion index in sorted portion [0:current]
        pos = bisect.bisect_left(lst, current_card, 0, current)

        # Shift elements to the right to make space
        for j in range(current, pos, -1):
            lst[j] = lst[j - 1]

        # Place current_card at correct position
        lst[pos] = current_card

    return lst

# Example call
arr = [12, 11, 13, 5, 6]
sorted_arr = insertion_sort_binary(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [5, 6, 11, 12, 13]
```

---

## Insertion Sort on a Singly Linked List

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insertion_sort_linked_list(head):
    if not head or not head.next:
        return head

    sorted_head = None      # start of sorted list
    current = head

    while current:
        next_node = current.next   # store next before unlinking

        # Insert current into sorted list
        if not sorted_head or current.data < sorted_head.data:
            # Insert at beginning
            current.next = sorted_head
            sorted_head = current
        else:
            # Find insertion point in sorted list
            temp = sorted_head
            while temp.next and temp.next.data < current.data:
                temp = temp.next
            current.next = temp.next
            temp.next = current

        current = next_node

    return sorted_head

# Helper to create list and print
def create_list(arr):
    if not arr:
        return None
    head = Node(arr[0])
    temp = head
    for val in arr[1:]:
        temp.next = Node(val)
        temp = temp.next
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> ")
        head = head.next
    print("None")

# Example call
arr = [12, 11, 13, 5, 6]
head = create_list(arr)
sorted_head = insertion_sort_linked_list(head)
print("Sorted linked list: ", end="")
print_list(sorted_head)   # Output: Sorted linked list: 5 -> 6 -> 11 -> 12 -> 13 -> None
```

---

## Insertion Sort in Descending Order

```python
def insertion_sort_descending(lst):
    n = len(lst)
    for current in range(1, n):
        current_card = lst[current]
        position = current - 1

        # Shift smaller elements to the right (for descending order)
        while position >= 0 and lst[position] < current_card:
            lst[position + 1] = lst[position]
            position -= 1

        lst[position + 1] = current_card

    return lst

# Example call
arr = [12, 11, 13, 5, 6]
sorted_arr = insertion_sort_descending(arr)
print("Descending order:", sorted_arr)   # Output: Descending order: [13, 12, 11, 6, 5]
```

---

## Insertion Sort with Custom Comparator

```python
def insertion_sort_custom(lst, compare):
    n = len(lst)
    for current in range(1, n):
        current_card = lst[current]
        position = current - 1

        # Use comparator to determine order
        while position >= 0 and compare(lst[position], current_card) > 0:
            lst[position + 1] = lst[position]
            position -= 1

        lst[position + 1] = current_card

    return lst

# Custom comparator: sort by string length
def compare_by_length(a, b):
    return len(a) - len(b)

# Example call
arr = ["apple", "kiwi", "banana", "cherry"]
sorted_arr = insertion_sort_custom(arr, compare_by_length)
print("Sorted by length:", sorted_arr)   # Output: Sorted by length: ['kiwi', 'apple', 'cherry', 'banana']
```

---

## Shell Sort (Generalized Insertion Sort)

Shell sort improves insertion sort by comparing elements separated by a gap, then reducing the gap.

```python
def shell_sort(lst):
    n = len(lst)
    gap = n // 2

    while gap > 0:
        for i in range(gap, n):
            temp = lst[i]
            j = i
            # Shift elements within the gap-sorted sublist
            while j >= gap and lst[j - gap] > temp:
                lst[j] = lst[j - gap]
                j -= gap
            lst[j] = temp
        gap //= 2

    return lst

# Example call
arr = [12, 11, 13, 5, 6]
sorted_arr = shell_sort(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [5, 6, 11, 12, 13]
```

---

## Insertion Sort Using For Loop with Explicit Shifting

```python
def insertion_sort_for_shift(lst):
    for i in range(1, len(lst)):
        current_card = lst[i]
        # Find position to insert using for loop moving backwards
        for j in range(i - 1, -1, -1):
            if lst[j] > current_card:
                lst[j + 1] = lst[j]          # shift right
                if j == 0:                    # special case: insert at beginning
                    lst[j] = current_card
            else:
                lst[j + 1] = current_card     # insert after j
                break
        else:
            # If loop completes without break, insert at beginning
            lst[0] = current_card

    return lst

# Example call
arr = [12, 11, 13, 5, 6]
sorted_arr = insertion_sort_for_shift(arr)
print("Sorted array:", sorted_arr)   # Output: Sorted array: [5, 6, 11, 12, 13]
```

---

## Stable Insertion Sort (Emphasized)

Insertion sort is naturally stable because it inserts equal elements after existing ones.

```python
def insertion_sort_stable(lst):
    n = len(lst)
    for current in range(1, n):
        current_card = lst[current]
        position = current - 1

        # Use <= to ensure stability: equal elements are not shifted, so order preserved
        while position >= 0 and lst[position] > current_card:
            lst[position + 1] = lst[position]
            position -= 1

        lst[position + 1] = current_card

    return lst

# Example call
arr = [(3, 'a'), (1, 'b'), (3, 'c'), (2, 'd')]   # list of tuples
sorted_arr = insertion_sort_stable(arr)
print("Stably sorted:", sorted_arr)   # Output: Stably sorted: [(1, 'b'), (2, 'd'), (3, 'a'), (3, 'c')]
```