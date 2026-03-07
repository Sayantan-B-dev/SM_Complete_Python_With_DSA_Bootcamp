## Reverse List: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Reversing a list means rearranging its elements so that the first becomes last, the second becomes second-last, and so on. This can be done by creating a new reversed list or modifying the original list in place.

**Approach:**  
- The simplest method is using Python's slicing feature to create a reversed copy.  
- For in-place reversal, swap elements symmetrically from both ends moving toward the center.  
- Other approaches include using built-in functions like `reversed()` or `reverse()`, recursion, or data structures like stacks.

**Algorithm (In-Place Two-Pointer):**  
1. Set `left = 0`, `right = len(lst) - 1`.  
2. While `left < right`:  
   - Swap `lst[left]` and `lst[right]`.  
   - Increment `left`, decrement `right`.  
3. Return the list (or nothing if in-place).

**Pseudocode:**  
```
function reverseList(lst):
    left = 0
    right = len(lst) - 1
    while left < right:
        swap(lst[left], lst[right])
        left = left + 1
        right = right - 1
    return lst
```

---

## Original Slicing Method

```python
def reverse_list_slice(lst):
    # Main logic: use slicing with step -1 to create a reversed copy
    return lst[::-1]                    # returns new reversed list

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_slice(original)
print("Reversed:", reversed_lst)        # Output: Reversed: [5, 4, 3, 2, 1]
print("Original unchanged:", original)  # Original unchanged: [1, 2, 3, 4, 5]
```

---

## Two-Pointer In-Place Reversal

```python
def reverse_list_two_pointer(lst):
    left = 0
    right = len(lst) - 1
    # Main logic: swap elements from both ends moving inward
    while left < right:
        lst[left], lst[right] = lst[right], lst[left]   # swap
        left += 1
        right -= 1
    return lst                                           # original list now reversed

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_two_pointer(original)
print("Reversed in-place:", reversed_lst)   # Output: Reversed in-place: [5, 4, 3, 2, 1]
print("Original modified:", original)       # Original modified: [5, 4, 3, 2, 1]
```

---

## Using List `reverse()` Method

```python
def reverse_list_method(lst):
    # Built-in list method that reverses in-place
    lst.reverse()                          # modifies original list
    return lst                              # return same list (optional)

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_method(original)
print("Reversed in-place:", reversed_lst)   # Output: Reversed in-place: [5, 4, 3, 2, 1]
print("Original modified:", original)       # Original modified: [5, 4, 3, 2, 1]
```

---

## Using `reversed()` Built-in

```python
def reverse_list_reversed(lst):
    # reversed() returns an iterator, convert to list to get reversed copy
    return list(reversed(lst))               # returns new reversed list

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_reversed(original)
print("Reversed:", reversed_lst)             # Output: Reversed: [5, 4, 3, 2, 1]
print("Original unchanged:", original)       # Original unchanged: [1, 2, 3, 4, 5]
```

---

## Recursive Reversal

```python
def reverse_list_recursive(lst):
    # Base case: empty or single-element list is already reversed
    if len(lst) <= 1:
        return lst
    # Recursive case: reverse the rest and append first element at the end
    return reverse_list_recursive(lst[1:]) + [lst[0]]

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_recursive(original)
print("Reversed:", reversed_lst)             # Output: Reversed: [5, 4, 3, 2, 1]
print("Original unchanged:", original)       # Original unchanged: [1, 2, 3, 4, 5]
```

---

## Using a Stack (List as Stack)

```python
def reverse_list_stack(lst):
    stack = []
    # Push all elements onto stack
    for item in lst:
        stack.append(item)                   # push
    # Pop elements to build reversed list
    reversed_lst = []
    while stack:
        reversed_lst.append(stack.pop())      # pop (LIFO)
    return reversed_lst

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_stack(original)
print("Reversed:", reversed_lst)             # Output: Reversed: [5, 4, 3, 2, 1]
print("Original unchanged:", original)       # Original unchanged: [1, 2, 3, 4, 5]
```

---

## Using `collections.deque`

```python
from collections import deque

def reverse_list_deque(lst):
    dq = deque(lst)
    reversed_lst = []
    # Main logic: pop from right and append to new list
    while dq:
        reversed_lst.append(dq.pop())         # pop from right end
    return reversed_lst

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_deque(original)
print("Reversed:", reversed_lst)             # Output: Reversed: [5, 4, 3, 2, 1]
print("Original unchanged:", original)       # Original unchanged: [1, 2, 3, 4, 5]
```

---

## Using a For Loop to Build New List

```python
def reverse_list_for_loop(lst):
    reversed_lst = []
    # Iterate from last index to first
    for i in range(len(lst) - 1, -1, -1):
        reversed_lst.append(lst[i])           # append each element in reverse order
    return reversed_lst

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_for_loop(original)
print("Reversed:", reversed_lst)             # Output: Reversed: [5, 4, 3, 2, 1]
print("Original unchanged:", original)       # Original unchanged: [1, 2, 3, 4, 5]
```

---

## Using While Loop to Build New List

```python
def reverse_list_while(lst):
    reversed_lst = []
    i = len(lst) - 1
    while i >= 0:
        reversed_lst.append(lst[i])           # append from end to start
        i -= 1
    return reversed_lst

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_while(original)
print("Reversed:", reversed_lst)             # Output: Reversed: [5, 4, 3, 2, 1]
print("Original unchanged:", original)       # Original unchanged: [1, 2, 3, 4, 5]
```

---

## Using List Comprehension with Negative Step

```python
def reverse_list_comprehension(lst):
    # List comprehension with index in reverse order
    return [lst[i] for i in range(len(lst) - 1, -1, -1)]

# Example call
original = [1, 2, 3, 4, 5]
reversed_lst = reverse_list_comprehension(original)
print("Reversed:", reversed_lst)             # Output: Reversed: [5, 4, 3, 2, 1]
print("Original unchanged:", original)       # Original unchanged: [1, 2, 3, 4, 5]
```