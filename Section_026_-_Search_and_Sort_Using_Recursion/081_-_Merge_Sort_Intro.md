# Merge Sort in Python

## 1. Conceptual Overview

**Merge Sort** is a comparison-based sorting algorithm based on the **divide and conquer paradigm**. The algorithm repeatedly divides the input array into two halves until each sub-array contains a single element. Single-element arrays are inherently sorted. The algorithm then merges these smaller sorted arrays back together while maintaining sorted order.

### Core Phases

| Phase   | Description                                                         |
| ------- | ------------------------------------------------------------------- |
| Divide  | Recursively split the array into two halves until size becomes one. |
| Conquer | Individual elements are considered sorted subarrays.                |
| Merge   | Combine two sorted subarrays into one sorted array.                 |

### Important Properties

| Property         | Value                                          |
| ---------------- | ---------------------------------------------- |
| Time Complexity  | `O(n log n)` in best, average, and worst cases |
| Space Complexity | `O(n)` auxiliary space                         |
| Stability        | Stable sorting algorithm                       |
| Sorting Method   | Comparison-based                               |

---

# 2. Recursive Merge Sort (Classical Implementation)

## Algorithm Flow

1. Divide the array into two halves.
2. Recursively sort each half.
3. Merge the two sorted halves into a single sorted list.

---

## Python Implementation (Recursive)

```python
def merge_sort(input_list):
    """
    Recursive Merge Sort implementation.

    Parameters
    ----------
    input_list : list
        List of integers that needs to be sorted.

    Returns
    -------
    list
        Sorted version of the input list.
    """

    # Base condition
    # If list length becomes 1 or 0, the list is already sorted
    if len(input_list) <= 1:
        return input_list

    # Step 1: Divide the list into two halves
    middle_index = len(input_list) // 2

    left_half = input_list[:middle_index]     # left subarray
    right_half = input_list[middle_index:]    # right subarray

    # Step 2: Recursively sort both halves
    sorted_left = merge_sort(left_half)
    sorted_right = merge_sort(right_half)

    # Step 3: Merge the sorted halves
    return merge(sorted_left, sorted_right)


def merge(left_array, right_array):
    """
    Merge two sorted arrays into one sorted array.

    This function performs the merging step of merge sort.

    Parameters
    ----------
    left_array : list
    right_array : list

    Returns
    -------
    list
        Merged sorted list.
    """

    merged_result = []

    left_pointer = 0
    right_pointer = 0

    # Compare elements from both arrays
    while left_pointer < len(left_array) and right_pointer < len(right_array):

        if left_array[left_pointer] <= right_array[right_pointer]:
            merged_result.append(left_array[left_pointer])
            left_pointer += 1
        else:
            merged_result.append(right_array[right_pointer])
            right_pointer += 1

    # Add remaining elements from left array
    while left_pointer < len(left_array):
        merged_result.append(left_array[left_pointer])
        left_pointer += 1

    # Add remaining elements from right array
    while right_pointer < len(right_array):
        merged_result.append(right_array[right_pointer])
        right_pointer += 1

    return merged_result


# Example usage
numbers = [38, 27, 43, 3, 9, 82, 10]

sorted_numbers = merge_sort(numbers)

print(sorted_numbers)
```

### Expected Output

```
[3, 9, 10, 27, 38, 43, 82]
```

---

# 3. Iterative (Non-Recursive / Bottom-Up) Merge Sort

The iterative implementation avoids recursion by merging subarrays of increasing sizes.

### Working Principle

Instead of splitting recursively, the algorithm starts by merging:

1. subarrays of size `1`
2. then size `2`
3. then size `4`
4. then size `8`

This continues until the entire array is merged.

---

## Python Implementation (Iterative Merge Sort)

```python
def merge_sort_iterative(input_list):
    """
    Iterative Bottom-Up Merge Sort implementation.

    This version avoids recursion and progressively merges
    subarrays of increasing sizes.

    Parameters
    ----------
    input_list : list
        List of integers to be sorted.

    Returns
    -------
    list
        Sorted list.
    """

    array_length = len(input_list)

    # Current size of subarrays to merge
    current_size = 1

    # Temporary array used during merging
    temp_array = input_list.copy()

    # Continue merging until subarray size exceeds array length
    while current_size < array_length:

        # Start merging pairs of subarrays
        for start_index in range(0, array_length, 2 * current_size):

            mid_index = min(start_index + current_size, array_length)
            end_index = min(start_index + 2 * current_size, array_length)

            left_pointer = start_index
            right_pointer = mid_index
            merged_pointer = start_index

            # Merge two subarrays
            while left_pointer < mid_index and right_pointer < end_index:

                if input_list[left_pointer] <= input_list[right_pointer]:
                    temp_array[merged_pointer] = input_list[left_pointer]
                    left_pointer += 1
                else:
                    temp_array[merged_pointer] = input_list[right_pointer]
                    right_pointer += 1

                merged_pointer += 1

            # Copy remaining elements from left subarray
            while left_pointer < mid_index:
                temp_array[merged_pointer] = input_list[left_pointer]
                left_pointer += 1
                merged_pointer += 1

            # Copy remaining elements from right subarray
            while right_pointer < end_index:
                temp_array[merged_pointer] = input_list[right_pointer]
                right_pointer += 1
                merged_pointer += 1

        # Copy temporary array back to original array
        input_list[:] = temp_array[:]

        # Double the subarray size for the next iteration
        current_size *= 2

    return input_list


# Example usage
numbers = [38, 27, 43, 3, 9, 82, 10]

sorted_numbers = merge_sort_iterative(numbers)

print(sorted_numbers)
```

### Expected Output

```
[3, 9, 10, 27, 38, 43, 82]
```

---

# 4. Step-by-Step Example

Sorting:

```
[38, 27, 43, 3, 9, 82, 10]
```

### Division Phase

```
[38,27,43,3,9,82,10]
        ↓
[38,27,43]   [3,9,82,10]
      ↓           ↓
[38] [27,43]   [3,9] [82,10]
```

### Merge Phase

```
[27,43] → merge with [38] → [27,38,43]

[3,9] → already sorted
[82,10] → [10,82]

[3,9] + [10,82] → [3,9,10,82]

Final merge:
[27,38,43] + [3,9,10,82]

Result:
[3,9,10,27,38,43,82]
```

---

# 5. Why Merge Sort Is Important in DSA

| Advantage                 | Explanation                                          |
| ------------------------- | ---------------------------------------------------- |
| Predictable complexity    | Always `O(n log n)` regardless of input distribution |
| Stable sort               | Maintains relative order of equal elements           |
| Suitable for linked lists | No random access required                            |
| External sorting          | Efficient for very large datasets                    |

---

# 6. Comparison With Other Sorting Algorithms

| Algorithm      | Best         | Average      | Worst        | Space      | Stable |
| -------------- | ------------ | ------------ | ------------ | ---------- | ------ |
| Bubble Sort    | `O(n)`       | `O(n²)`      | `O(n²)`      | `O(1)`     | Yes    |
| Selection Sort | `O(n²)`      | `O(n²)`      | `O(n²)`      | `O(1)`     | No     |
| Quick Sort     | `O(n log n)` | `O(n log n)` | `O(n²)`      | `O(log n)` | No     |
| **Merge Sort** | `O(n log n)` | `O(n log n)` | `O(n log n)` | `O(n)`     | Yes    |
