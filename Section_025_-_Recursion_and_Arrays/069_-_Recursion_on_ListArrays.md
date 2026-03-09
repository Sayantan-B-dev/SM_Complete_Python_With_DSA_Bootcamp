### Additional Recursive Array Examples

The following examples illustrate further recursive patterns on arrays, contrasting index‑bound and copying approaches.

---

#### Recursive Binary Search (Index Bounds)

A standard binary search that returns the index of the target or -1.

```python
def binary_search(arr: list, target: int, left: int, right: int) -> int:
    """
    Returns index of target in sorted arr[left:right+1], or -1 if not found.
    """
    if left > right:
        return -1

    mid = (left + right) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search(arr, target, mid + 1, right)
    else:
        return binary_search(arr, target, left, mid - 1)
```

**Wrapper for ease of use:**

```python
def search(arr: list, target: int) -> int:
    return binary_search(arr, target, 0, len(arr) - 1)
```

---

#### Two‑Pointer Pair Sum – Recursive with Index Bounds

Determines whether any two distinct elements in a sorted array sum to a given target.

```python
def has_pair_sum(arr: list, target: int, left: int, right: int) -> bool:
    """
    Returns True if arr[left] + arr[right] == target for some left <= right.
    Assumes arr is sorted.
    """
    if left >= right:
        return False                     # no pair possible

    current_sum = arr[left] + arr[right]
    if current_sum == target:
        return True
    elif current_sum < target:
        # Need a larger sum, move left pointer forward
        return has_pair_sum(arr, target, left + 1, right)
    else:  # current_sum > target
        # Need a smaller sum, move right pointer backward
        return has_pair_sum(arr, target, left, right - 1)
```

**Wrapper:**

```python
def pair_sum_exists(arr: list, target: int) -> bool:
    return has_pair_sum(arr, target, 0, len(arr) - 1)
```

---

#### Two‑Pointer Pair Sum – Inefficient Copying Version

Demonstrates the overhead of creating new sublists for each recursive call. This version is shown only for conceptual comparison and should not be used in practice.

```python
def pair_sum_copy(arr: list, target: int) -> bool:
    """
    Returns True if any two distinct elements in arr sum to target.
    Inefficient due to list slicing.
    """
    if len(arr) < 2:
        return False

    left = 0
    right = len(arr) - 1
    current_sum = arr[left] + arr[right]

    if current_sum == target:
        return True
    elif current_sum < target:
        # Recurse on subarray excluding the leftmost element
        return pair_sum_copy(arr[1:], target)
    else:  # current_sum > target
        # Recurse on subarray excluding the rightmost element
        return pair_sum_copy(arr[:-1], target)
```

**Analysis**: Each recursive call creates a new list slice, copying O(k) elements where k is the current length. Total copying cost becomes O(n²) in the worst case. Index‑bound version avoids copying entirely.

---

#### Recursive Array Reversal (Two‑Pointer)

Reverses an array in place using recursion with two pointers.

```python
def reverse_range(arr: list, left: int, right: int) -> None:
    """
    Reverses the subarray arr[left:right+1] in place.
    """
    if left >= right:
        return

    # Swap elements at left and right
    arr[left], arr[right] = arr[right], arr[left]
    # Recurse on the inner subarray
    reverse_range(arr, left + 1, right - 1)

def reverse_array(arr: list) -> None:
    reverse_range(arr, 0, len(arr) - 1)
```

**Example:**

```python
a = [1, 2, 3, 4, 5]
reverse_array(a)
print(a)  # Output: [5, 4, 3, 2, 1]
```

---

#### Recursive Check for Sorted Array

Verifies if an array is sorted in non‑decreasing order using recursion.

```python
def is_sorted(arr: list, index: int = 0) -> bool:
    """
    Returns True if arr[index:] is sorted in non‑decreasing order.
    """
    if index >= len(arr) - 1:
        return True                     # single element or empty is sorted

    if arr[index] > arr[index + 1]:
        return False                    # out of order
    return is_sorted(arr, index + 1)
```

**Index‑bound variant (if only a subarray needs checking):**

```python
def is_sorted_range(arr: list, left: int, right: int) -> bool:
    if left >= right:
        return True
    if arr[left] > arr[left + 1]:
        return False
    return is_sorted_range(arr, left + 1, right)
```

---

## 5. Time and Space Complexity Analysis

| Problem                          | Approach        | Time Complexity | Space Complexity (stack) | Additional Memory |
|----------------------------------|-----------------|-----------------|---------------------------|-------------------|
| First/last occurrence            | Index bounds    | O(log n)        | O(log n)                  | O(1)              |
| First/last occurrence            | Copying slices  | O(n log n)*     | O(log n)                  | O(n log n)*       |
| Two‑pointer pair sum             | Index bounds    | O(n)            | O(n)                      | O(1)              |
| Two‑pointer pair sum             | Copying slices  | O(n²)           | O(n)                      | O(n²)             |
| Binary search                    | Index bounds    | O(log n)        | O(log n)                  | O(1)              |
| Array reversal                   | Index bounds    | O(n)            | O(n)                      | O(1)              |
| Sorted check                     | Index bounds    | O(n)            | O(n)                      | O(1)              |

*The copying version for first occurrence has O(n log n) total copying due to repeated slicing; each level of recursion copies a subarray of size roughly n/2, n/4, ... summing to O(n). However, each copy is O(k) and performed at multiple levels, leading to O(n log n) total work.*

**Key observation**: Index‑bound recursion uses only constant extra memory per call (the indices), whereas copying approaches allocate new lists, increasing both time and memory overhead substantially.

---

## 6. Edge Cases and Failure Scenarios

- **Empty array**: All functions must handle `len(arr) == 0` gracefully (base cases return appropriate values).  
- **Single element**: Binary search and pair sum functions should not treat a single element as a valid pair; two‑pointer base case `left >= right` correctly returns `False`.  
- **Target not present**: Search functions return -1; pair sum returns False.  
- **Duplicate elements**: First/last occurrence functions correctly locate boundaries due to continued search in the relevant half.  
- **Very large arrays**: Recursion depth may become a problem for linear‑recursion algorithms (e.g., pair sum, reversal, sorted check) when n > ~1000. For these, iteration is preferred.  
- **Unsorted input**: Binary search and pair sum assume sorted input; behavior is undefined otherwise.  

---

## 7. Practical Use Cases

- **Binary search** is fundamental in database indexing, search engines, and any scenario requiring fast lookup in sorted data.  
- **First/last occurrence** is used in counting occurrences (e.g., count of a value in a sorted range), interval queries, and implementing `lower_bound` / `upper_bound`.  
- **Two‑pointer pair sum** appears in problems like finding two numbers with a given sum (e.g., in financial transactions, cryptography).  
- **Array reversal** is a building block in algorithms (e.g., rotating arrays, palindrome checking).  
- **Sorted check** is often used in assertions or as a helper in divide‑and‑conquer algorithms (e.g., validating invariants).

In production Python, iterative solutions are generally preferred for linear‑time problems due to recursion depth limits and function call overhead. Recursive versions serve educational purposes and are applicable when the recursion depth is logarithmic (e.g., binary search) or when the data structure itself is recursive (e.g., trees).

---

## 8. Limitations and Trade-offs

- **Recursion depth**: Python’s default recursion limit (~1000) restricts the use of linear‑recursion algorithms for large inputs. Converting to iteration is necessary for scalability.  
- **Performance**: Function calls in Python are relatively expensive; iterative loops are faster and more memory‑efficient.  
- **Readability**: For simple array traversals, loops are often clearer than recursion. Recursion shines when the problem naturally decomposes into similar subproblems (e.g., divide‑and‑conquer).  
- **Copying overhead**: Passing sliced copies to recursive calls is extremely inefficient and should be avoided in any performance‑sensitive code. The index‑bound pattern is the correct way to handle subarrays recursively.  
- **Tail call optimization**: Python does not optimize tail recursion, so even conceptually tail‑recursive algorithms (like the two‑pointer pair sum) still consume O(n) stack space.

The examples provided illustrate the trade‑offs between clarity, efficiency, and stack usage. For any real‑world application, the index‑bound iterative equivalents should be used unless the recursive formulation offers significant conceptual advantage and the input size is guaranteed to be small.