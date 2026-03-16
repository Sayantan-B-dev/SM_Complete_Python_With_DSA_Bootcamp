### Next Greater Element

```python
def next_greater_element(a, n):
    result = [-1] * n
    stack = []
    for i in range(n - 1, -1, -1):
        while stack and stack[-1] <= a[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(a[i])
    return result
```




### Function: Next Greater Element Using Monotonic Stack

The **Next Greater Element (NGE)** problem determines, for every element in an array, the first element to its right that is strictly greater. If no such element exists, the result for that position is `-1`.
The optimal solution uses a **monotonic decreasing stack** to ensure linear time complexity.

#### Algorithmic Strategy

1. Traverse the array **from right to left**.
2. Maintain a stack containing potential candidates for the next greater element.
3. While the stack top is **less than or equal to** the current element, remove it because it cannot be the next greater element.
4. If the stack becomes empty, the next greater element does not exist and `-1` is stored.
5. Otherwise, the stack top represents the nearest greater value.
6. Push the current element onto the stack for future comparisons.

**Time Complexity:** `O(n)` because each element is pushed and popped at most once.
**Space Complexity:** `O(n)` for the stack and result storage.

---

### Implementation

```python
def next_greater_element(a, n):
    """
    Computes the Next Greater Element for every element in the array.

    Parameters
    ----------
    a : list[int]
        Input list containing integer values.
    n : int
        Number of elements present in the list.

    Returns
    -------
    list[int]
        A list where each index contains the next greater element
        appearing to the right of the original array position.
        If no greater element exists, the value will be -1.
    """

    # Result array initialized with -1 for all positions
    # -1 represents that no greater element exists
    result = [-1] * n

    # Stack used to store elements that may act
    # as the next greater candidate for elements
    stack = []

    # Traverse the array from right to left
    # because the next greater element must lie to the right
    for index in range(n - 1, -1, -1):

        current_value = a[index]

        # Remove elements from the stack that are
        # smaller than or equal to the current value
        # because they cannot serve as next greater elements
        while stack and stack[-1] <= current_value:
            stack.pop()

        # If stack still contains elements,
        # the top element becomes the next greater element
        if stack:
            result[index] = stack[-1]

        # Push the current element onto the stack
        # so it can serve as a candidate for elements on the left
        stack.append(current_value)

    return result
```

---

### Expected Output Example

```python
array_values = [4, 5, 2, 25]
length = len(array_values)

print(next_greater_element(array_values, length))
```

```
[5, 25, 25, -1]
```

**Explanation of Result**

| Index | Element | Next Greater Element |
| ----- | ------- | -------------------- |
| 0     | 4       | 5                    |
| 1     | 5       | 25                   |
| 2     | 2       | 25                   |
| 3     | 25      | -1                   |

Each element maps to the **first strictly larger element appearing to its right**. If no such element exists, the value remains `-1`.


---

