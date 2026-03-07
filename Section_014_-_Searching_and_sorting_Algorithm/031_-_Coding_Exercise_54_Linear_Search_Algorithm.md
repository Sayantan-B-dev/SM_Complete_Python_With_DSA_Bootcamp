## Linear Search: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Linear search is the simplest searching algorithm. It traverses a collection sequentially, comparing each element with a target value until a match is found or the end is reached.

**Approach:**  
- Start from the first element.  
- Compare each element with the target.  
- If a match is found, return its index (or the element itself).  
- If the entire collection is scanned without a match, return a sentinel value (like `-1` or `None`).

**Algorithm:**  
1. Set `i = 0`.  
2. While `i < length(array)`:  
   - If `array[i] == target`, return `i`.  
   - Increment `i`.  
3. Return `-1`.

**Pseudocode:**  
```
function linear_search(array, target):
    for i from 0 to len(array)-1:
        if array[i] == target:
            return i
    return -1
```

---

## Iterative Linear Search

```python
def linear_search_iterative(arr, target):
    # Iterate over each index in the list
    for i in range(len(arr)):
        # Main logic: compare current element with target
        if arr[i] == target:
            return i          # Return index on match
    return -1                 # Return -1 if target not found

# Example call
arr = [4, 2, 7, 1, 9, 3]
target = 7
result = linear_search_iterative(arr, target)
print(f"Target {target} found at index: {result}")  # Output: Target 7 found at index: 2
```

---

## Recursive Linear Search

```python
def linear_search_recursive(arr, target, index=0):
    # Base case: reached end without finding target
    if index >= len(arr):
        return -1
    # Main logic: compare current element
    if arr[index] == target:
        return index          # Return index on match
    # Recursive call: move to next index
    return linear_search_recursive(arr, target, index + 1)

# Example call
arr = [4, 2, 7, 1, 9, 3]
target = 1
result = linear_search_recursive(arr, target)
print(f"Target {target} found at index: {result}")  # Output: Target 1 found at index: 3
```

---

## Sentinel Linear Search

Optimized version that eliminates the need to check array bounds inside the loop by placing a sentinel value at the end.

```python
def linear_search_sentinel(arr, target):
    # Make a copy to avoid modifying original; append target as sentinel
    temp = arr[:] + [target]
    i = 0
    # Main logic: loop until sentinel is found
    while temp[i] != target:
        i += 1
    # After loop, check if we found real target or sentinel
    if i < len(arr):
        return i          # Return index if target was in original list
    else:
        return -1         # Sentinel reached -> not found

# Example call
arr = [4, 2, 7, 1, 9, 3]
target = 9
result = linear_search_sentinel(arr, target)
print(f"Target {target} found at index: {result}")  # Output: Target 9 found at index: 4
```

---

## Linear Search in Sorted Array (Early Exit)

If the array is sorted, we can stop early when elements become greater than the target.

```python
def linear_search_sorted(arr, target):
    # Assumes arr is sorted in ascending order
    for i in range(len(arr)):
        # Main logic: compare current element
        if arr[i] == target:
            return i          # Found
        # Special early exit: if current > target, further elements will be larger
        if arr[i] > target:
            break
    return -1                 # Not found

# Example call
arr = [1, 3, 5, 7, 9, 11]
target = 6
result = linear_search_sorted(arr, target)
print(f"Target {target} found at index: {result}")  # Output: Target 6 found at index: -1
```

---

## Using Python's Built-in `index()` Method

The most Pythonic way, but raises an exception if not found.

```python
def linear_search_builtin(arr, target):
    try:
        # Built-in method: returns first occurrence index
        return arr.index(target)
    except ValueError:
        # Exception handling for not found
        return -1

# Example call
arr = [4, 2, 7, 1, 9, 3]
target = 2
result = linear_search_builtin(arr, target)
print(f"Target {target} found at index: {result}")  # Output: Target 2 found at index: 1
```

---

## Using `enumerate()` for Index-Value Pairs

Pythonic iteration that simultaneously gives index and value.

```python
def linear_search_enumerate(arr, target):
    # Enumerate yields (index, element) pairs
    for i, val in enumerate(arr):
        # Main logic: compare value with target
        if val == target:
            return i          # Return index directly
    return -1

# Example call
arr = [4, 2, 7, 1, 9, 3]
target = 3
result = linear_search_enumerate(arr, target)
print(f"Target {target} found at index: {result}")  # Output: Target 3 found at index: 5
```

---

## Returning All Occurrences (Multiple Indices)

Instead of stopping at the first match, collect all indices where target appears.

```python
def linear_search_all(arr, target):
    indices = []
    # Main logic: iterate with index
_   for i in range(len(arr)):
        if arr[i] == target:
            indices.append(i)    # Collect each index
    return indices                # Return list of indices (empty if none)

# Example call
arr = [4, 2, 7, 2, 9, 2, 3]
target = 2
result = linear_search_all(arr, target)
print(f"Target {target} found at indices: {result}")  # Output: Target 2 found at indices: [1, 3, 5]
```

---

## Linear Search in a 2D List (Matrix)

Search for a value in a matrix and return its (row, column) coordinates.

```python
def linear_search_2d(matrix, target):
    # Main logic: nested loops
    for i in range(len(matrix)):          # Iterate rows
        for j in range(len(matrix[i])):   # Iterate columns
            if matrix[i][j] == target:
                return (i, j)              # Return tuple (row, col)
    return None                            # Not found

# Example call
matrix = [[1, 4, 7],
          [2, 5, 8],
          [3, 6, 9]]
target = 5
result = linear_search_2d(matrix, target)
print(f"Target {target} found at coordinates: {result}")  # Output: Target 5 found at coordinates: (1, 1)
```

---

## Using `for`-`else` Construct

Python's `for` loop can have an `else` clause that executes only if the loop completes without a `break`.

```python
def linear_search_for_else(arr, target):
    # Main logic: iterate and break on match
    for i in range(len(arr)):
        if arr[i] == target:
            print(f"Found at index {i}")
            break
    else:
        # This else runs only if loop wasn't broken (i.e., not found)
        print("Not found")
        return -1
    return i   # return index after break (i is still defined)

# Example call
arr = [4, 2, 7, 1, 9, 3]
target = 8
result = linear_search_for_else(arr, target)
# Output: Not found
print(f"Return value: {result}")  # Output: Return value: -1
```

---

## Manual Index with `while` Loop

Explicit index control, useful for situations where index manipulation is needed.

```python
def linear_search_while(arr, target):
    i = 0
    # Main logic: while loop with manual index
    while i < len(arr):
        if arr[i] == target:
            return i          # Found
        i += 1                 # Increment index
    return -1                  # Not found

# Example call
arr = [4, 2, 7, 1, 9, 3]
target = 7
result = linear_search_while(arr, target)
print(f"Target {target} found at index: {result}")  # Output: Target 7 found at index: 2
```