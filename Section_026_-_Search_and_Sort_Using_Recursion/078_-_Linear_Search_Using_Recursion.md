## 1. Basic Recursive Linear Search (Boolean Return)

**Explanation**  
Checks whether a target element exists in a list using recursion. The function returns `True` if found, otherwise `False`.

**Approach**  
- Use an index parameter to track the current position.  
- Base case: index reaches the list length → element not found, return `False`.  
- If element at current index equals target, return `True`.  
- Otherwise, recursively call with incremented index.

**Algorithm**  
1. If `index == len(arr)`, return `False`.  
2. If `arr[index] == target`, return `True`.  
3. Return `recursive_linear_search(arr, target, index + 1)`.

**Pseudocode**
```
function linear_search_bool(arr, target, index=0):
    if index == len(arr):
        return False
    if arr[index] == target:
        return True
    return linear_search_bool(arr, target, index + 1)
```

**Implementation**
```python
def linear_search_bool(arr, target, index=0):
    # Base case: reached end without finding target
    if index == len(arr):
        return False
    # Target found at current index
    if arr[index] == target:
        return True
    # Recursive call to check remaining elements
    return linear_search_bool(arr, target, index + 1)
```

**Complexity Analysis**  
- **Time:** O(n) – each element is compared once.  
- **Space:** O(n) – recursion stack depth equals number of elements in worst case.

---

## 2. Recursive Linear Search Returning Index (First Occurrence)

**Explanation**  
Returns the index of the first occurrence of target, or -1 if not present.

**Approach**  
- Same recursive traversal.  
- Base case: index out of bounds → return -1.  
- If match found → return current index.  
- Otherwise, return result of recursive call.

**Algorithm**  
1. If `index == len(arr)`, return -1.  
2. If `arr[index] == target`, return `index`.  
3. Return `linear_search_index(arr, target, index + 1)`.

**Pseudocode**
```
function linear_search_index(arr, target, index=0):
    if index == len(arr):
        return -1
    if arr[index] == target:
        return index
    return linear_search_index(arr, target, index + 1)
```

**Implementation**
```python
def linear_search_index(arr, target, index=0):
    # End of list reached, target not found
    if index == len(arr):
        return -1
    # Target found, return current index
    if arr[index] == target:
        return index
    # Continue searching in the rest of the list
    return linear_search_index(arr, target, index + 1)
```

**Complexity Analysis**  
- **Time:** O(n) – worst case scans entire list.  
- **Space:** O(n) – recursion stack.

---

## 3. Recursive Linear Search Using List Slicing

**Explanation**  
Implements linear search without an explicit index parameter. Instead, passes progressively smaller slices of the list.

**Approach**  
- Base case: empty list → return -1.  
- Check first element of current slice. If match, return 0 (relative to current slice).  
- Recurse on slice `arr[1:]` and adjust returned index.

**Algorithm**  
1. If `arr` is empty, return -1.  
2. If `arr[0] == target`, return 0.  
3. `result = linear_search_slice(arr[1:], target)`.  
4. If `result == -1`, return -1; else return `result + 1`.

**Pseudocode**
```
function linear_search_slice(arr, target):
    if len(arr) == 0:
        return -1
    if arr[0] == target:
        return 0
    result = linear_search_slice(arr[1:], target)
    return -1 if result == -1 else result + 1
```

**Implementation**
```python
def linear_search_slice(arr, target):
    # Base case: empty sublist, target not found
    if len(arr) == 0:
        return -1
    # Check first element of current slice
    if arr[0] == target:
        return 0  # found at relative index 0
    # Recursive call on the rest of the list
    result = linear_search_slice(arr[1:], target)
    # If found in the rest, adjust index by adding 1
    if result == -1:
        return -1
    else:
        return result + 1
```

**Complexity Analysis**  
- **Time:** O(n²) – each slice creates a new list of size n-1, causing quadratic copying overhead.  
- **Space:** O(n²) – due to list copies on each call; recursion stack also O(n).

---

## 4. Tail-Recursive Linear Search

**Explanation**  
Demonstrates a tail‑recursive formulation where the recursive call is the last operation. Python does not optimize tail recursion, but the pattern is conceptually useful.

**Approach**  
- Use an accumulator (or direct return) so that no operation follows the recursive call.  
- In Python, the return statement simply passes the result upward.

**Algorithm**  
Identical to basic boolean or index search – the recursive call is already the last expression in those versions. This variant highlights the pattern explicitly.

**Pseudocode**
```
function linear_search_tail(arr, target, index=0):
    if index == len(arr):
        return False
    if arr[index] == target:
        return True
    return linear_search_tail(arr, target, index + 1)
```

**Implementation**
```python
def linear_search_tail(arr, target, index=0):
    # Base cases
    if index == len(arr):
        return False
    if arr[index] == target:
        return True
    # Recursive call is the final action – tail position
    return linear_search_tail(arr, target, index + 1)
```

**Complexity Analysis**  
- **Time:** O(n).  
- **Space:** O(n) – Python still builds a stack frame for each call.

---

## 5. Recursive Linear Search Returning All Indices

**Explanation**  
Returns a list of all indices where the target appears.

**Approach**  
- Traverse the list, collecting indices where match occurs.  
- Use an accumulator list passed through recursive calls, or build the list on return.

**Algorithm**  
1. If `index == len(arr)`, return empty list.  
2. Recurse to get list of indices from the rest of the array.  
3. If current element matches, prepend current index to the result.

**Pseudocode**
```
function linear_search_all(arr, target, index=0):
    if index == len(arr):
        return []
    indices = linear_search_all(arr, target, index + 1)
    if arr[index] == target:
        return [index] + indices
    else:
        return indices
```

**Implementation**
```python
def linear_search_all(arr, target, index=0):
    # Base case: end of list, no more indices
    if index == len(arr):
        return []
    # Recursively find all indices in the remaining part
    indices_from_rest = linear_search_all(arr, target, index + 1)
    # If current element matches, add current index to the front
    if arr[index] == target:
        return [index] + indices_from_rest
    else:
        return indices_from_rest
```

**Complexity Analysis**  
- **Time:** O(n) – each element visited once.  
- **Space:** O(n) for recursion stack plus O(k) for result list, where k is number of matches.

---

## 6. Recursive Linear Search with Custom Key Function

**Explanation**  
Allows searching based on transformed values using a key function (e.g., case‑insensitive string comparison).

**Approach**  
- Accept a `key` callable that maps each element to a comparable value.  
- Compare `key(arr[index])` with `target`.  
- Base case and recursion as in basic search.

**Algorithm**  
1. If `index == len(arr)`, return `False`.  
2. If `key(arr[index]) == target`, return `True`.  
3. Return recursive call with incremented index.

**Pseudocode**
```
function linear_search_key(arr, target, key=lambda x: x, index=0):
    if index == len(arr):
        return False
    if key(arr[index]) == target:
        return True
    return linear_search_key(arr, target, key, index + 1)
```

**Implementation**
```python
def linear_search_key(arr, target, key=lambda x: x, index=0):
    # Default key is identity (no transformation)
    if index == len(arr):
        return False
    # Compare transformed element with target
    if key(arr[index]) == target:
        return True
    return linear_search_key(arr, target, key, index + 1)
```

**Complexity Analysis**  
- **Time:** O(n) – each element transformed once.  
- **Space:** O(n) – recursion stack.

---

## 7. Recursive Linear Search Using Inner Function (Closure)

**Explanation**  
Hides the index parameter by defining a recursive inner function that captures `arr` and `target` from the outer scope.

**Approach**  
- Outer function accepts `arr` and `target`.  
- Inner recursive function uses an index parameter.  
- Outer function initiates recursion with index 0.

**Algorithm**  
1. Define inner function `search(idx)`.  
2. Inside `search`: if `idx == len(arr)`, return `False`; if `arr[idx] == target`, return `True`; else return `search(idx + 1)`.  
3. Outer returns `search(0)`.

**Pseudocode**
```
function linear_search_closure(arr, target):
    function search(idx):
        if idx == len(arr):
            return False
        if arr[idx] == target:
            return True
        return search(idx + 1)
    return search(0)
```

**Implementation**
```python
def linear_search_closure(arr, target):
    def search(idx):
        # Base case: index out of bounds
        if idx == len(arr):
            return False
        # Match found
        if arr[idx] == target:
            return True
        # Recurse with next index
        return search(idx + 1)
    # Start recursion from index 0
    return search(0)
```

**Complexity Analysis**  
- **Time:** O(n).  
- **Space:** O(n) – recursion stack.

---

## 8. Recursive Linear Search Counting Occurrences

**Explanation**  
Returns the total count of how many times the target appears in the list.

**Approach**  
- Increment count when a match is found.  
- Add counts from recursive calls.

**Algorithm**  
1. If `index == len(arr)`, return 0.  
2. `count = 1 if arr[index] == target else 0`.  
3. Return `count + linear_search_count(arr, target, index + 1)`.

**Pseudocode**
```
function linear_search_count(arr, target, index=0):
    if index == len(arr):
        return 0
    match = 1 if arr[index] == target else 0
    return match + linear_search_count(arr, target, index + 1)
```

**Implementation**
```python
def linear_search_count(arr, target, index=0):
    # Base case: no more elements to check
    if index == len(arr):
        return 0
    # Determine if current element matches
    match = 1 if arr[index] == target else 0
    # Add matches from the rest of the list
    return match + linear_search_count(arr, target, index + 1)
```

**Complexity Analysis**  
- **Time:** O(n).  
- **Space:** O(n).

---

## 9. Recursive Linear Search for Last Occurrence

**Explanation**  
Finds the index of the last occurrence of the target, or -1 if not found.

**Approach**  
- Traverse the list recursively, but propagate the highest index found.  
- On returning from recursion, update result if current index is a match and greater than previously found.

**Algorithm**  
1. If `index == len(arr)`, return -1.  
2. `result = linear_search_last(arr, target, index + 1)`.  
3. If `result != -1`, return `result`.  
4. Else if `arr[index] == target`, return `index`.  
5. Else return -1.

**Pseudocode**
```
function linear_search_last(arr, target, index=0):
    if index == len(arr):
        return -1
    result = linear_search_last(arr, target, index + 1)
    if result != -1:
        return result
    if arr[index] == target:
        return index
    return -1
```

**Implementation**
```python
def linear_search_last(arr, target, index=0):
    # Base case: end of list, no match found yet
    if index == len(arr):
        return -1
    # First, search the rest of the list
    result_from_rest = linear_search_last(arr, target, index + 1)
    # If a later occurrence exists, return that index
    if result_from_rest != -1:
        return result_from_rest
    # Otherwise, check current element
    if arr[index] == target:
        return index
    # No match in this or subsequent elements
    return -1
```

**Complexity Analysis**  
- **Time:** O(n).  
- **Space:** O(n).

---

## 10. Recursive Linear Search Using `any()` and Generator (Hybrid)

**Explanation**  
Combines recursion with Python’s `any()` and a generator to express linear search declaratively.

**Approach**  
- Recursively yield boolean values indicating matches.  
- `any()` short‑circuits upon first `True`.

**Algorithm**  
1. If `index == len(arr)`, return (generator stops).  
2. Yield `arr[index] == target`.  
3. Recurse with `index + 1`.  
4. Wrap with `any()` to consume generator.

**Pseudocode**
```
function linear_search_any(arr, target, index=0):
    if index == len(arr):
        return
    yield arr[index] == target
    yield from linear_search_any(arr, target, index + 1)
```

**Implementation**
```python
def linear_search_any(arr, target, index=0):
    # Base case: stop generation
    if index == len(arr):
        return
    # Yield whether current element matches
    yield arr[index] == target
    # Continue generating from the rest
    yield from linear_search_any(arr, target, index + 1)

# Usage:
found = any(linear_search_any([1, 2, 3, 4], 3))
```

**Complexity Analysis**  
- **Time:** O(n) in worst case; short‑circuits if early match.  
- **Space:** O(n) due to recursion stack and generator frames.