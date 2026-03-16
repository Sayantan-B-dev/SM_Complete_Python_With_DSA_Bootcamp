### Next Smaller Elements

```python
def next_smaller_elements(arr):
    n = len(arr)
    result = [-1] * n
    stack = []
    for i in range(n - 1, -1, -1):
        while stack and stack[-1] >= arr[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(arr[i])
    return result
```


## Next Smaller Element Using Monotonic Stack

### Problem Definition

For every element in an array, determine the **first element to its right that is strictly smaller**.
If no such element exists, the result for that position is `-1`.

Example:

| Input Array        | Output              |
| ------------------ | ------------------- |
| `[4, 8, 5, 2, 25]` | `[2, 5, 2, -1, -1]` |

Interpretation:

* For `4`, the next smaller element is `2`.
* For `8`, the next smaller element is `5`.
* For `5`, the next smaller element is `2`.
* For `2`, no smaller element exists to the right.

### Algorithmic Strategy

A **monotonic increasing stack** efficiently solves the problem.

Core principle:

* Traverse the array **from right to left**.
* Maintain a stack containing elements that may serve as the next smaller candidates.
* Remove elements from the stack that are **greater than or equal to the current value**, since they cannot be the next smaller element.
* The stack top, if present, becomes the next smaller element.

### Algorithm Steps

| Step | Description                                                    |
| ---- | -------------------------------------------------------------- |
| 1    | Initialize result list filled with `-1`                        |
| 2    | Create an empty stack                                          |
| 3    | Traverse array from right to left                              |
| 4    | Remove stack elements greater than or equal to current element |
| 5    | If stack is not empty, stack top becomes next smaller          |
| 6    | Push current element into stack                                |

### Complexity Analysis

| Metric           | Value                                                         |
| ---------------- | ------------------------------------------------------------- |
| Time Complexity  | `O(n)` because each element is pushed and popped at most once |
| Space Complexity | `O(n)` for stack and result storage                           |

---

## Implementation

```python
def next_smaller_elements(arr):
    """
    Function to compute the next smaller element for every element
    in the given array using a monotonic stack approach.

    Parameters
    ----------
    arr : List[int]
        Input list containing integer values.

    Returns
    -------
    List[int]
        List where each index contains the next smaller element
        appearing to the right. If none exists, the value is -1.
    """

    # Length of the array used for iteration control
    n = len(arr)

    # Result array initialized with -1
    # Default value indicates absence of smaller element
    result = [-1] * n

    # Stack used to maintain potential smaller candidates
    stack = []

    # Traverse array from right to left
    for index in range(n - 1, -1, -1):

        current_value = arr[index]

        # Remove elements from stack that are greater than or equal
        # because they cannot serve as next smaller elements
        while stack and stack[-1] >= current_value:
            stack.pop()

        # If stack still contains elements,
        # the top element becomes the next smaller value
        if stack:
            result[index] = stack[-1]

        # Push current value into stack for future comparisons
        stack.append(current_value)

    return result
```

---

## Expected Output Demonstration

```python
array_values = [4, 8, 5, 2, 25]

print(next_smaller_elements(array_values))
```

### Expected Output

```
[2, 5, 2, -1, -1]
```

### Structural Explanation

| Index | Element | Next Smaller Element |
| ----- | ------- | -------------------- |
| 0     | 4       | 2                    |
| 1     | 8       | 5                    |
| 2     | 5       | 2                    |
| 3     | 2       | -1                   |
| 4     | 25      | -1                   |

Each value maps to the **closest smaller value located to its right**. If no such value exists, the result remains `-1`.
