# Linear Search

## Definition

**Linear Search** is a sequential searching algorithm that scans every element of a collection one by one until the target element is found or the entire collection has been traversed.

The algorithm does not require the dataset to be sorted and works with any sequential data structure such as arrays, lists, or strings.

---

# Conceptual Operation

Linear search evaluates each element starting from the first position and continues moving forward until a match is encountered.

### Logical Flow

1. Start at index `0`.
2. Compare the current element with the target value.
3. If the values match, return the current index.
4. If the values do not match, move to the next element.
5. Continue until the element is found or the end of the collection is reached.
6. If the end is reached without finding the element, return a value indicating absence.

---

# Algorithm Representation

### Stepwise Algorithm

```
1. Accept the list and the target element.
2. Traverse the list from index 0 to n-1.
3. For each element:
       If element == target
            return index
4. If loop completes without match
       return -1
```

---

# Python Implementation

```python
def linear_search(input_list, target_value):
    """
    Performs linear search over a list.

    Parameters:
    input_list (list): List of elements to search within
    target_value (any): Value that needs to be located

    Returns:
    int: Index of target if found
         -1 if the element does not exist
    """

    # iterate through every element sequentially
    for current_index in range(len(input_list)):

        # check if current element equals the target
        if input_list[current_index] == target_value:

            # return the index where element is located
            return current_index

    # if loop ends without finding element
    return -1


# Example usage
numbers_list = [12, 45, 7, 23, 89, 34]
target_number = 23

result_index = linear_search(numbers_list, target_number)

print(result_index)
```

### Expected Output

```
3
```

The algorithm returned index `3` because the element `23` exists at that position in the list.

---

# Time Complexity Analysis

| Case         | Time Complexity | Explanation                                     |
| ------------ | --------------- | ----------------------------------------------- |
| Best Case    | O(1)            | Target element located at first index           |
| Average Case | O(n)            | Element found somewhere in the middle           |
| Worst Case   | O(n)            | Element located at last position or not present |

---

# Space Complexity

| Type            | Complexity | Explanation                                 |
| --------------- | ---------- | ------------------------------------------- |
| Auxiliary Space | O(1)       | No additional memory required during search |

Linear search operates in constant extra space because it only uses a few variables regardless of input size.

---

# Characteristics

| Property         | Description                            |
| ---------------- | -------------------------------------- |
| Data Requirement | Works on both sorted and unsorted data |
| Implementation   | Extremely simple algorithm             |
| Performance      | Inefficient for very large datasets    |
| Stability        | Does not modify the original data      |

---

# Advantages

| Advantage              | Explanation                                  |
| ---------------------- | -------------------------------------------- |
| Simplicity             | Very easy to implement and understand        |
| Works on unsorted data | No preprocessing or sorting required         |
| Minimal overhead       | No complex logic or memory structures needed |

---

# Disadvantages

| Disadvantage                   | Explanation                                   |
| ------------------------------ | --------------------------------------------- |
| Inefficient for large datasets | Requires checking many elements sequentially  |
| Not scalable                   | Performance degrades linearly with input size |
| Poor for repeated searches     | Re-scans entire dataset every time            |

---

# Example Walkthrough

### Dataset

```
Array = [10, 25, 33, 47, 59]
Target = 47
```

### Iteration Process

| Step | Index | Value Compared | Result      |
| ---- | ----- | -------------- | ----------- |
| 1    | 0     | 10             | Not equal   |
| 2    | 1     | 25             | Not equal   |
| 3    | 2     | 33             | Not equal   |
| 4    | 3     | 47             | Match found |

### Result

```
Index returned = 3
```

---

# Recursive Implementation

```python
def linear_search_recursive(input_list, target_value, current_index=0):
    """
    Recursive version of linear search.
    """

    # base case: reached end of list
    if current_index >= len(input_list):
        return -1

    # check if element matches
    if input_list[current_index] == target_value:
        return current_index

    # recursive call for next index
    return linear_search_recursive(input_list, target_value, current_index + 1)


numbers_list = [5, 9, 13, 21, 42]
target_number = 21

result_index = linear_search_recursive(numbers_list, target_number)

print(result_index)
```

### Expected Output

```
3
```

---

# Real World Applications

| Application              | Description                            |
| ------------------------ | -------------------------------------- |
| Small datasets           | Searching within small arrays or lists |
| Unsorted collections     | When sorting overhead is unnecessary   |
| Sequential file scanning | Checking records one-by-one            |
| Simple lookup tasks      | Basic element presence detection       |

---

# Comparison with Binary Search

| Feature                   | Linear Search          | Binary Search         |
| ------------------------- | ---------------------- | --------------------- |
| Data requirement          | Works on unsorted data | Requires sorted data  |
| Time complexity           | O(n)                   | O(log n)              |
| Implementation complexity | Very simple            | More complex          |
| Dataset suitability       | Small datasets         | Large sorted datasets |

---

# Key Insight

Linear search is conceptually the simplest search algorithm but becomes inefficient as dataset size grows. Its practical importance lies primarily in educational contexts and small-scale datasets where algorithmic overhead must remain minimal.
