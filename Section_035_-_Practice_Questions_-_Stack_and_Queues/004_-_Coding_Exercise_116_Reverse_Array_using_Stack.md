
### Reverse Array using Stack

```python
def reverse_array_using_stack(arr, n):
    stack = []
    for i in range(n):
        stack.append(arr[i])
    for i in range(n):
        arr[i] = stack.pop()
    return arr
```


## Reverse Array Using Stack

### Conceptual Principle

A **stack** follows the **Last-In-First-Out (LIFO)** discipline.
When elements of an array are pushed sequentially onto a stack, the element inserted last becomes the first one removed. This property naturally reverses the order of elements.

Reversal procedure:

1. Push every array element onto the stack.
2. Pop elements from the stack sequentially.
3. Insert the popped values back into the array.

Because popping occurs in reverse insertion order, the array becomes reversed.

### Algorithm Steps

| Step | Operation                                                       |
| ---- | --------------------------------------------------------------- |
| 1    | Initialize an empty stack                                       |
| 2    | Traverse the array and push each element into the stack         |
| 3    | Traverse array indices again                                    |
| 4    | Pop elements from stack and assign them back to array positions |

### Complexity Analysis

| Metric           | Value                                                 |
| ---------------- | ----------------------------------------------------- |
| Time Complexity  | `O(n)` because each element is pushed and popped once |
| Space Complexity | `O(n)` because stack stores all array elements        |

---

## Implementation

```python
def reverse_array_using_stack(arr, n):
    """
    Reverses the given array using a stack data structure.

    Parameters
    ----------
    arr : list
        Input array that must be reversed.
    n : int
        Number of elements present in the array.

    Returns
    -------
    list
        The same array but with elements in reversed order.
    """

    # Stack used to temporarily store elements
    # This stack enables reversal due to LIFO behavior
    stack = []

    # Step 1: Push all elements of the array onto the stack
    for index in range(n):
        stack.append(arr[index])

    # Step 2: Pop elements from stack and place them back into array
    for index in range(n):
        arr[index] = stack.pop()

    # Return the reversed array
    return arr
```

---

## Expected Output Demonstration

```python
array_values = [1, 2, 3, 4, 5]
length = len(array_values)

print(reverse_array_using_stack(array_values, length))
```

### Expected Output

```
[5, 4, 3, 2, 1]
```

### Structural Explanation

| Original Array | Stack After Push | Final Array After Pop |
| -------------- | ---------------- | --------------------- |
| `[1,2,3,4,5]`  | `[1,2,3,4,5]`    | `[5,4,3,2,1]`         |

The stack ensures that the **last inserted element (`5`) becomes the first extracted**, thereby reversing the sequence.
