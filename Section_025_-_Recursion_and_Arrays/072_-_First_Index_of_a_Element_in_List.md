## Recursive Search in a List

### Finding the **First Index** and **Last Index** of an Element Using Recursion

The following algorithm demonstrates how recursion can be used to search inside a list structure without using iterative constructs such as loops. The algorithm progressively reduces the problem size by slicing the list and delegating the remaining search to the recursive call.

Two complementary functions are implemented:

| Function                | Purpose                                               |
| ----------------------- | ----------------------------------------------------- |
| `firstIndexOfAnElement` | Returns the **first occurrence index** of the element |
| `lastIndexOfAnElement`  | Returns the **last occurrence index** of the element  |

Both functions rely on **problem size reduction** and **index reconstruction** when recursion returns.

---

# 1. First Index of an Element (Recursive Strategy)

### Conceptual Logic

The algorithm checks the **first element** of the list.

If the element matches the target:

```
Return index 0
```

Otherwise the algorithm recursively searches the **remaining sublist**:

```
l1[1:]
```

Because the recursive call searches in a **smaller list**, the returned index must be shifted by **+1** to map it back to the original list.

---

### Implementation

```python
def firstIndexOfAnElement(l1, x):
    # BASE CASE
    # When the list becomes empty, the element does not exist.
    # Returning -1 indicates that the element was not found.
    if(len(l1) == 0):
        return -1

    # DIRECT MATCH CASE
    # If the first element of the current list matches the target,
    # the first occurrence is found at index position 0.
    if(l1[0] == x):
        return 0

    # RECURSIVE STEP
    # Call the function again on the remaining list.
    # The first element is removed using slicing.
    ansFromRecursion = firstIndexOfAnElement(l1[1:], x)

    # RESULT HANDLING
    # If recursion returns -1, the element does not exist.
    # Otherwise the index returned belongs to the smaller list.
    # Therefore we add 1 to adjust the index relative to
    # the original list.
    if(ansFromRecursion == -1):
        return ansFromRecursion
    else:
        return ansFromRecursion + 1
```

---

### Recursive Tree Visualization

Example Input

```
l1 = [4, 7, 2, 7, 9]
x  = 7
```

Recursive call structure:

```
firstIndexOfAnElement([4,7,2,7,9],7)
|
|-- firstIndexOfAnElement([7,2,7,9],7)
|       |
|       |-- match found
|       |-- return 0
|
|-- recursion returned 0
|-- add 1 → return 1
```

Final Result

```
Index = 1
```

---

# 2. Last Index of an Element (Recursive Strategy)

### Conceptual Logic

The algorithm recursively searches the **remaining list first**.

This ensures the recursion explores the **deepest position** of the list before checking the current element.

Steps:

1. Recursively search the remaining list.
2. If recursion finds the element → propagate that index upward.
3. If recursion does not find it → check the current element.

This ensures the **last occurrence** is detected.

---

### Implementation with Extensive Comments

```python
def lastIndexOfAnElement(l1, x):

    # BASE CASE
    # If the list becomes empty, the element cannot exist.
    # Returning -1 indicates absence of the element.
    if(len(l1) == 0):
        return -1

    # RECURSIVE STEP
    # Instead of checking the first element immediately,
    # the algorithm first explores the remaining part of
    # the list. This ensures that deeper positions are
    # examined before the current element.
    ansFromRecursion = lastIndexOfAnElement(l1[1:], x)

    # CASE 1
    # If recursion found the element somewhere in the
    # remaining list, the returned index belongs to the
    # smaller list. Therefore we add 1 to shift the index
    # back to the original list.
    if(ansFromRecursion != -1):
        return ansFromRecursion + 1

    # CASE 2
    # If recursion did not find the element (-1),
    # we now check whether the current element
    # matches the target value.
    if(l1[0] == x):
        return 0

    # CASE 3
    # If the current element also does not match,
    # the element does not exist in the list.
    else:
        return -1
```

---

# Recursive Tree for Last Index Search

Example Input

```
l1 = [4, 7, 2, 7, 9]
x  = 7
```

Recursive expansion:

```
lastIndexOfAnElement([4,7,2,7,9])
|
|-- lastIndexOfAnElement([7,2,7,9])
|      |
|      |-- lastIndexOfAnElement([2,7,9])
|             |
|             |-- lastIndexOfAnElement([7,9])
|                    |
|                    |-- lastIndexOfAnElement([9])
|                           |
|                           |-- lastIndexOfAnElement([])
|                                  |
|                                  |-- return -1
|
| check 9 → not match → return -1
|
| check 7 → match → return 0
|
| propagate index upward:
|
| return 1
|
| return 2
|
| return 3
```

Final Result

```
Last Index = 3
```

---

# Algorithm Complexity Analysis

| Property         | Value                       |
| ---------------- | --------------------------- |
| Time Complexity  | O(n)                        |
| Space Complexity | O(n) due to recursion stack |
| List Traversals  | Exactly one full traversal  |
| Recursion Depth  | Maximum n calls             |

---

# Example Execution

```python
l1 = [4,7,2,7,9]

print(firstIndexOfAnElement(l1,7))
print(lastIndexOfAnElement(l1,7))
```

### Expected Output

```
1
3
```
