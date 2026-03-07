# Binary Search

## Definition

**Binary Search** is a divide-and-conquer searching algorithm used to locate a target element inside a **sorted collection**. The algorithm repeatedly divides the search interval in half and eliminates the portion of the dataset where the target cannot exist.

Binary search operates by comparing the target value with the middle element of the array. Based on the comparison result, the algorithm decides whether to continue searching in the left half or the right half of the collection.

Binary search is significantly more efficient than linear search for large datasets because it reduces the search space by half during every iteration.

---

# Preconditions for Binary Search

Binary search works only under specific conditions.

| Requirement         | Explanation                                               |
| ------------------- | --------------------------------------------------------- |
| Sorted data         | The array must be sorted in ascending or descending order |
| Random access       | The structure must allow direct indexing like arrays      |
| Comparable elements | Elements must support ordering comparison operations      |

If the dataset is not sorted, binary search cannot guarantee correct results.

---

# Core Concept

Binary search uses three variables to maintain the search range.

| Variable | Meaning                              |
| -------- | ------------------------------------ |
| `low`    | Starting index of search space       |
| `high`   | Ending index of search space         |
| `mid`    | Middle index of current search range |

The algorithm calculates the middle index and compares the value with the target.

---

# Logical Process

### Stepwise Procedure

1. Initialize `low = 0`.
2. Initialize `high = length of array - 1`.
3. Compute the middle index.
4. Compare the middle element with the target.
5. If equal, return the index.
6. If target is smaller, discard the right half.
7. If target is larger, discard the left half.
8. Repeat until the element is found or search space becomes empty.

---

# Mathematical Mid Calculation

The middle index must be calculated carefully to avoid overflow in some languages.

### Standard Calculation

```
mid = (low + high) // 2
```

### Safer Calculation

```
mid = low + (high - low) // 2
```

The second approach prevents integer overflow in languages with fixed integer sizes.

---

# Binary Search Algorithm

```
1. Start with two pointers:
       low = 0
       high = n - 1

2. While low <= high:
       mid = (low + high) // 2

3. If array[mid] == target:
       return mid

4. If array[mid] < target:
       low = mid + 1

5. Else:
       high = mid - 1

6. If loop finishes:
       return -1
```

---

# Python Iterative Implementation

```python
def binary_search(sorted_list, target_value):
    """
    Performs binary search on a sorted list.

    Parameters:
    sorted_list (list): A sorted list of elements
    target_value (any): Element that needs to be searched

    Returns:
    int: Index of the target element
         -1 if element is not present
    """

    # initialize boundaries of the search interval
    left_index = 0
    right_index = len(sorted_list) - 1

    # continue searching while valid interval exists
    while left_index <= right_index:

        # compute the middle index safely
        middle_index = left_index + (right_index - left_index) // 2

        # retrieve middle element
        middle_element = sorted_list[middle_index]

        # check if middle element matches target
        if middle_element == target_value:
            return middle_index

        # if target is greater, discard left half
        elif middle_element < target_value:
            left_index = middle_index + 1

        # if target is smaller, discard right half
        else:
            right_index = middle_index - 1

    # element not found
    return -1


# Example dataset
numbers = [3, 7, 12, 19, 25, 31, 44]

target = 25

index_found = binary_search(numbers, target)

print(index_found)
```

### Expected Output

```
4
```

The algorithm returns index `4` because the value `25` exists at that position.

---

# Recursive Implementation

```python
def binary_search_recursive(sorted_list, target_value, left_index, right_index):
    """
    Recursive binary search implementation.
    """

    # base condition: interval becomes invalid
    if left_index > right_index:
        return -1

    # compute middle index
    middle_index = left_index + (right_index - left_index) // 2

    # check if element matches target
    if sorted_list[middle_index] == target_value:
        return middle_index

    # search right half
    if sorted_list[middle_index] < target_value:
        return binary_search_recursive(sorted_list, target_value, middle_index + 1, right_index)

    # search left half
    return binary_search_recursive(sorted_list, target_value, left_index, middle_index - 1)


numbers = [5, 11, 18, 24, 33, 47]

target = 24

index_found = binary_search_recursive(numbers, target, 0, len(numbers) - 1)

print(index_found)
```

### Expected Output

```
3
```

---

# Time Complexity Analysis

| Case         | Time Complexity | Explanation                                |
| ------------ | --------------- | ------------------------------------------ |
| Best Case    | O(1)            | Target element found at middle immediately |
| Average Case | O(log n)        | Search space halves repeatedly             |
| Worst Case   | O(log n)        | Maximum number of halvings required        |

Binary search reduces the problem size exponentially.

Example search steps for `n = 1,000,000`

```
1,000,000 → 500,000 → 250,000 → 125,000 → ...
```

Only about **20 comparisons** are required.

---

# Space Complexity

| Implementation | Space Complexity | Explanation                           |
| -------------- | ---------------- | ------------------------------------- |
| Iterative      | O(1)             | Only pointer variables used           |
| Recursive      | O(log n)         | Stack frames created during recursion |

---

# Binary Search Walkthrough

### Dataset

```
Array = [5, 10, 15, 20, 25, 30, 35]
Target = 25
```

### Iteration Table

| Step | Low | High | Mid | Value | Decision     |
| ---- | --- | ---- | --- | ----- | ------------ |
| 1    | 0   | 6    | 3   | 20    | Search right |
| 2    | 4   | 6    | 5   | 30    | Search left  |
| 3    | 4   | 4    | 4   | 25    | Found        |

### Result

```
Index = 4
```

---

# Advantages

| Advantage           | Explanation                              |
| ------------------- | ---------------------------------------- |
| Extremely efficient | Logarithmic complexity makes it scalable |
| Minimal comparisons | Large datasets handled quickly           |
| Widely used         | Foundation of many advanced algorithms   |

---

# Disadvantages

| Disadvantage           | Explanation                      |
| ---------------------- | -------------------------------- |
| Requires sorted data   | Sorting overhead may be required |
| Random access required | Inefficient on linked lists      |
| Slightly complex logic | Compared with linear search      |

---

# Variants of Binary Search

Binary search has several specialized forms used in competitive programming and interviews.

| Variant                 | Purpose                        |
| ----------------------- | ------------------------------ |
| Lower Bound             | Finds first element ≥ target   |
| Upper Bound             | Finds first element > target   |
| First Occurrence        | Used when duplicates exist     |
| Last Occurrence         | Locates final instance         |
| Binary Search on Answer | Used for optimization problems |
| Rotated Binary Search   | Works on rotated sorted arrays |

---

# Example: First Occurrence Binary Search

```python
def first_occurrence(sorted_list, target_value):

    left_index = 0
    right_index = len(sorted_list) - 1
    result_index = -1

    while left_index <= right_index:

        middle_index = left_index + (right_index - left_index) // 2

        if sorted_list[middle_index] == target_value:
            result_index = middle_index
            right_index = middle_index - 1

        elif sorted_list[middle_index] < target_value:
            left_index = middle_index + 1

        else:
            right_index = middle_index - 1

    return result_index


numbers = [2, 4, 4, 4, 6, 9]

print(first_occurrence(numbers, 4))
```

### Expected Output

```
1
```

---

# Binary Search vs Linear Search

| Feature                   | Linear Search    | Binary Search  |
| ------------------------- | ---------------- | -------------- |
| Data requirement          | Unsorted allowed | Must be sorted |
| Time complexity           | O(n)             | O(log n)       |
| Implementation complexity | Simple           | Moderate       |
| Large dataset performance | Poor             | Excellent      |

---

# Key Insight

Binary search is one of the most fundamental algorithms in computer science. Many advanced algorithms such as database indexing, search engines, scheduling systems, and optimization algorithms rely on the divide-and-conquer principle introduced by binary search.
