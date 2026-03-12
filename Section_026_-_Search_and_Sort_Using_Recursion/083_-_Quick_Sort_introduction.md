# Quick Sort

## Definition

**Quick Sort** is a comparison-based **divide-and-conquer sorting algorithm** that sorts a list by repeatedly selecting a **pivot element**, rearranging elements so that smaller elements move to the left of the pivot and larger elements move to the right, and then recursively applying the same strategy to the resulting subarrays.

Unlike algorithms that repeatedly compare adjacent elements, Quick Sort attempts to **place elements directly near their final sorted position** during each partition step.

---

# Working Philosophy

The fundamental philosophy of Quick Sort is based on three principles.

### Divide

The array is divided around a **pivot element** so that all elements smaller than the pivot appear on the left side and all larger elements appear on the right side.

### Conquer

Each resulting subarray is sorted independently using the same Quick Sort strategy.

### Combine

Unlike Merge Sort, Quick Sort does **not require an explicit merging step** because the partition operation already positions elements correctly relative to the pivot.

This philosophy makes Quick Sort very efficient in practice because many elements move close to their correct position early in the process.

---

# Algorithm in Plain English

The Quick Sort procedure can be described in logical steps independent of programming language.

### Step 1 — Choose a Pivot

Select an element from the array.
Common choices include:

| Pivot Strategy  | Description                    |
| --------------- | ------------------------------ |
| First element   | Simplest approach              |
| Last element    | Common teaching implementation |
| Random element  | Reduces worst-case probability |
| Median-of-three | Improves practical performance |

---

### Step 2 — Partition the Array

Rearrange the array so that:

* Elements **smaller than pivot** move left.
* Elements **greater than pivot** move right.
* Pivot ends up in its **final sorted position**.

---

### Step 3 — Recursively Sort Left Subarray

Apply Quick Sort to the section **before the pivot**.

---

### Step 4 — Recursively Sort Right Subarray

Apply Quick Sort to the section **after the pivot**.

---

### Step 5 — Recursion Stops

The algorithm stops when a subarray contains **zero or one element**, because such arrays are already sorted.

---

# Partitioning Concept

Partitioning is the **core operation of Quick Sort**.
Its purpose is to place the pivot in the correct location while reorganizing other elements around it.

### Partition Rules

| Condition       | Action                          |
| --------------- | ------------------------------- |
| Element < Pivot | Move element to left side       |
| Element > Pivot | Move element to right side      |
| Pivot           | Ends in correct sorted position |

---

# Visual Partition Diagram

Example array:

```
[8, 3, 1, 7, 0, 10, 2]
```

Pivot chosen:

```
Pivot = 2
```

### Initial State

```
8   3   1   7   0   10   2
                        ↑
                      Pivot
```

### During Partition

Elements smaller than pivot gradually move left.

```
1   0   | 2 |   8   3   7   10
```

### After Partition

```
[1, 0]   2   [8, 3, 7, 10]
```

Pivot is now in its **correct sorted position**.

Left side contains smaller elements.
Right side contains larger elements.

---

# Full Quick Sort Diagram

Initial array

```
[8, 3, 1, 7, 0, 10, 2]
```

### First Partition

```
[1,0]  2  [8,3,7,10]
```

### Sort Left Side

```
[1,0]

Pivot = 0

Result
[0,1]
```

### Sort Right Side

```
[8,3,7,10]

Pivot = 10
```

```
[8,3,7] 10
```

Then recursively sort `[8,3,7]`.

---

### Final Sorted Array

```
[0,1,2,3,7,8,10]
```

---

# Python Implementation

## Partition Function

```python
def partition(array, start_index, end_index):
    """
    Partition rearranges the array around a pivot element.

    All elements smaller than the pivot move to the left side.
    All elements greater than the pivot move to the right side.

    The pivot is chosen as the last element.
    """

    # Choose pivot element
    pivot_value = array[end_index]

    # Index that tracks position of smaller elements
    smaller_index = start_index - 1

    # Traverse the array segment
    for current_index in range(start_index, end_index):

        # If element is smaller than pivot
        if array[current_index] <= pivot_value:

            smaller_index += 1

            # Swap elements
            array[smaller_index], array[current_index] = (
                array[current_index],
                array[smaller_index]
            )

    # Place pivot into correct position
    array[smaller_index + 1], array[end_index] = (
        array[end_index],
        array[smaller_index + 1]
    )

    # Return pivot location
    return smaller_index + 1
```

---

## Quick Sort Function

```python
def quick_sort(array, start_index, end_index):
    """
    Recursive Quick Sort implementation.

    Parameters
    ----------
    array : list
        List of elements to be sorted.

    start_index : int
        Starting index of subarray.

    end_index : int
        Ending index of subarray.
    """

    # Base case for recursion
    if start_index >= end_index:
        return

    # Partition array and obtain pivot position
    pivot_index = partition(array, start_index, end_index)

    # Recursively sort left partition
    quick_sort(array, start_index, pivot_index - 1)

    # Recursively sort right partition
    quick_sort(array, pivot_index + 1, end_index)
```

---

## Example Execution

```python
numbers = [8, 3, 1, 7, 0, 10, 2]

quick_sort(numbers, 0, len(numbers) - 1)

print(numbers)
```

### Expected Output

```
[0, 1, 2, 3, 7, 8, 10]
```

---

# Time Complexity

| Case         | Complexity   |
| ------------ | ------------ |
| Best Case    | `O(n log n)` |
| Average Case | `O(n log n)` |
| Worst Case   | `O(n²)`      |

Worst case happens when the pivot always produces extremely unbalanced partitions.

Example:

```
[1,2,3,4,5]
```

Pivot always becomes the smallest or largest element.

---

# Space Complexity

| Type             | Complexity         |
| ---------------- | ------------------ |
| Recursion Stack  | `O(log n)` average |
| Worst Case Stack | `O(n)`             |

Quick Sort is usually considered **in-place** because it does not require an auxiliary array like Merge Sort.

---

# Why Quick Sort Is Popular

| Reason                          | Explanation                                          |
| ------------------------------- | ---------------------------------------------------- |
| Excellent practical performance | Performs fewer operations than many other algorithms |
| Cache friendly                  | Works efficiently with CPU memory hierarchy          |
| In-place sorting                | Requires minimal extra memory                        |
| Widely used                     | Used internally in many standard libraries           |

---

# Comparison With Merge Sort

| Feature          | Quick Sort      | Merge Sort           |
| ---------------- | --------------- | -------------------- |
| Sorting Strategy | Partition based | Merge based          |
| Extra Memory     | Minimal         | Requires extra array |
| Stability        | Not stable      | Stable               |
| Worst Case       | `O(n²)`         | `O(n log n)`         |
| Practical Speed  | Often faster    | Slightly slower      |
