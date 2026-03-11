## 1. Basic Boolean Search
### Explanation
This variant returns a boolean indicating whether the target element exists in the list. It's the simplest form of linear search using recursion.

### Approach
- Start from the first element (index 0).
- If the current index equals the length of the list, the element is not found → return False.
- If the element at current index matches the target → return True.
- Otherwise, recursively call the function with the next index.

### Algorithm
1. Define function `linear_search_bool(arr, target, index=0)`.
2. Base case: if `index == len(arr)`, return False.
3. If `arr[index] == target`, return True.
4. Recursive case: return `linear_search_bool(arr, target, index+1)`.

### Pseudocode
```
function linear_search_bool(arr, target, index=0):
    if index == len(arr):
        return False
    if arr[index] == target:
        return True
    return linear_search_bool(arr, target, index+1)
```

### Code
```python
def linear_search_bool(arr, target, index=0):
    # Base case: reached end without finding
    if index == len(arr):
        return False
    # Check current element
    if arr[index] == target:
        return True
    # Recursive call to next index
    return linear_search_bool(arr, target, index + 1)

# Example usage
arr = [3, 5, 1, 9, 2]
target = 9
print(linear_search_bool(arr, target))  # Output: True
```

---

## 2. Return First Occurrence Index
### Explanation
This variant returns the index of the first occurrence of the target, or -1 if not found. It's a common requirement for search functions.

### Approach
- Similar to boolean search but return the index when found.
- If element found at current index, return that index.
- If not found after all elements, return -1.

### Algorithm
1. Define function `linear_search_first(arr, target, index=0)`.
2. Base case: if `index == len(arr)`, return -1.
3. If `arr[index] == target`, return `index`.
4. Recursive case: return `linear_search_first(arr, target, index+1)`.

### Pseudocode
```
function linear_search_first(arr, target, index=0):
    if index == len(arr):
        return -1
    if arr[index] == target:
        return index
    return linear_search_first(arr, target, index+1)
```

### Code
```python
def linear_search_first(arr, target, index=0):
    # Base case: reached end, target not found
    if index == len(arr):
        return -1
    # If found at current index, return index
    if arr[index] == target:
        return index
    # Recursive call to search further
    return linear_search_first(arr, target, index + 1)

# Example
arr = [4, 2, 7, 2, 9]
target = 2
print(linear_search_first(arr, target))  # Output: 1
```

---

## 3. Return Last Occurrence Index
### Explanation
This variant returns the index of the last occurrence of the target. It searches from the end of the list towards the beginning.

### Approach
- Start from the last index (len(arr)-1) and move backwards.
- Base case: if index becomes -1, return -1 (not found).
- If element matches target, return index.
- Otherwise, recursively call with index-1.

### Algorithm
1. Define function `linear_search_last(arr, target, index=None)`. If index is None, set it to `len(arr)-1`.
2. Base case: if `index < 0`, return -1.
3. If `arr[index] == target`, return `index`.
4. Recursive case: return `linear_search_last(arr, target, index-1)`.

### Pseudocode
```
function linear_search_last(arr, target, index=len(arr)-1):
    if index < 0:
        return -1
    if arr[index] == target:
        return index
    return linear_search_last(arr, target, index-1)
```

### Code
```python
def linear_search_last(arr, target, index=None):
    # Initialize index to last element on first call
    if index is None:
        index = len(arr) - 1
    # Base case: checked all elements
    if index < 0:
        return -1
    # Check current element from the end
    if arr[index] == target:
        return index
    # Recursive call moving leftwards
    return linear_search_last(arr, target, index - 1)

# Example
arr = [4, 2, 7, 2, 9]
target = 2
print(linear_search_last(arr, target))  # Output: 3
```

---

## 4. Return All Indices
### Explanation
This variant returns a list of all indices where the target occurs. It collects indices during recursion.

### Approach
- Use recursion to traverse the list, accumulating indices in a list.
- At each step, if current element matches target, add current index to the result list.
- Combine results from recursive calls.

### Algorithm
1. Define function `linear_search_all(arr, target, index=0)`.
2. Base case: if `index == len(arr)`, return empty list.
3. Recursively get result from the rest: `rest_indices = linear_search_all(arr, target, index+1)`.
4. If `arr[index] == target`, prepend `index` to `rest_indices`.
5. Return the combined list.

### Pseudocode
```
function linear_search_all(arr, target, index=0):
    if index == len(arr):
        return []
    rest = linear_search_all(arr, target, index+1)
    if arr[index] == target:
        return [index] + rest
    else:
        return rest
```

### Code
```python
def linear_search_all(arr, target, index=0):
    # Base case: reached end, return empty list
    if index == len(arr):
        return []
    # Recursive call for remaining elements
    rest_indices = linear_search_all(arr, target, index + 1)
    # If current element matches, include current index
    if arr[index] == target:
        return [index] + rest_indices
    else:
        return rest_indices

# Example
arr = [1, 2, 3, 2, 4, 2]
target = 2
print(linear_search_all(arr, target))  # Output: [1, 3, 5]
```

---

## 5. Count Occurrences
### Explanation
This variant counts how many times the target appears in the list using recursion.

### Approach
- Traverse the list, incrementing count when match is found.
- Return total count after recursion.

### Algorithm
1. Define function `linear_search_count(arr, target, index=0)`.
2. Base case: if `index == len(arr)`, return 0.
3. Recursive count from rest: `count_rest = linear_search_count(arr, target, index+1)`.
4. If `arr[index] == target`, return `1 + count_rest` else `count_rest`.

### Pseudocode
```
function linear_search_count(arr, target, index=0):
    if index == len(arr):
        return 0
    count = linear_search_count(arr, target, index+1)
    if arr[index] == target:
        return 1 + count
    else:
        return count
```

### Code
```python
def linear_search_count(arr, target, index=0):
    # Base case: end of list
    if index == len(arr):
        return 0
    # Recursive count from next index
    count_from_rest = linear_search_count(arr, target, index + 1)
    # Add 1 if current element matches
    if arr[index] == target:
        return 1 + count_from_rest
    else:
        return count_from_rest

# Example
arr = [2, 4, 2, 6, 2, 8]
target = 2
print(linear_search_count(arr, target))  # Output: 3
```

---

## 6. Search in a Sublist (with start and end indices)
### Explanation
This variant searches for the target within a specified sublist range `[start, end)`. Useful for searching in portions of a list.

### Approach
- Pass `start` and `end` parameters to define the range.
- Base case: if `start >= end`, return -1 (not found).
- If `arr[start] == target`, return `start`.
- Recursively call with `start+1` and same `end`.

### Algorithm
1. Define function `linear_search_range(arr, target, start, end)`.
2. Base case: if `start >= end`, return -1.
3. If `arr[start] == target`, return `start`.
4. Recursive case: return `linear_search_range(arr, target, start+1, end)`.

### Pseudocode
```
function linear_search_range(arr, target, start, end):
    if start >= end:
        return -1
    if arr[start] == target:
        return start
    return linear_search_range(arr, target, start+1, end)
```

### Code
```python
def linear_search_range(arr, target, start, end):
    # Base case: empty range or start beyond end
    if start >= end:
        return -1
    # Check current start index
    if arr[start] == target:
        return start
    # Recursively search from next index
    return linear_search_range(arr, target, start + 1, end)

# Example: search in indices 2 to 5 (inclusive start, exclusive end)
arr = [10, 20, 30, 40, 50, 60]
target = 40
print(linear_search_range(arr, target, 2, 5))  # Output: 3
```

---

## 7. Tail Recursive Search
### Explanation
Tail recursion is when the recursive call is the last operation. Here, we pass an accumulator (current index) and return the result directly. This can be optimized by some compilers.

### Approach
- Use an additional parameter to carry the current index.
- The recursive call returns the result without further computation.
- Base case returns -1 or the found index.

### Algorithm
1. Define function `linear_search_tail(arr, target, index=0)`.
2. Base case: if `index == len(arr)`, return -1.
3. If `arr[index] == target`, return `index`.
4. Recursive case: return `linear_search_tail(arr, target, index+1)` — this is the last operation.

### Pseudocode
```
function linear_search_tail(arr, target, index=0):
    if index == len(arr):
        return -1
    if arr[index] == target:
        return index
    return linear_search_tail(arr, target, index+1)   # tail call
```

### Code
```python
def linear_search_tail(arr, target, index=0):
    # Base case: not found
    if index == len(arr):
        return -1
    # Found at current index
    if arr[index] == target:
        return index
    # Tail recursive call: no further computation after return
    return linear_search_tail(arr, target, index + 1)

# Example
arr = [5, 8, 3, 9, 2]
target = 3
print(linear_search_tail(arr, target))  # Output: 2
```

---

## 8. Non-Tail Recursive Search (with explicit result handling)
### Explanation
This variant demonstrates non-tail recursion by storing the result from the recursive call and then possibly modifying it. It's similar to the "Return All Indices" approach but for a single index.

### Approach
- Recursively get result from the rest.
- If current element matches, return current index; otherwise, return the result from recursion.

### Algorithm
1. Define function `linear_search_nontail(arr, target, index=0)`.
2. Base case: if `index == len(arr)`, return -1.
3. `result = linear_search_nontail(arr, target, index+1)`.
4. If `arr[index] == target`, return `index` else return `result`.

### Pseudocode
```
function linear_search_nontail(arr, target, index=0):
    if index == len(arr):
        return -1
    result = linear_search_nontail(arr, target, index+1)
    if arr[index] == target:
        return index
    else:
        return result
```

### Code
```python
def linear_search_nontail(arr, target, index=0):
    # Base case: end of list
    if index == len(arr):
        return -1
    # Recursive call first
    result_from_rest = linear_search_nontail(arr, target, index + 1)
    # Then check current element
    if arr[index] == target:
        return index   # This overrides result_from_rest if found earlier
    else:
        return result_from_rest

# Note: This returns the last occurrence because recursion goes to end first,
# then checks backwards. To get first occurrence, logic would need adjustment.
# Example: returns 3 for target 2 in [1,2,3,2] because it finds the last one.
arr = [1, 2, 3, 2]
target = 2
print(linear_search_nontail(arr, target))  # Output: 3 (last occurrence)
```

---

## 9. Search with Custom Comparator
### Explanation
This variant allows searching using a custom comparison function, useful for lists of objects or complex equality conditions.

### Approach
- Accept a comparator function that takes an element and returns True if it matches the target criteria.
- The comparator replaces the direct equality check.

### Algorithm
1. Define function `linear_search_custom(arr, comparator, index=0)`.
2. Base case: if `index == len(arr)`, return -1.
3. If `comparator(arr[index])` is True, return `index`.
4. Recursive case: return `linear_search_custom(arr, comparator, index+1)`.

### Pseudocode
```
function linear_search_custom(arr, comparator, index=0):
    if index == len(arr):
        return -1
    if comparator(arr[index]):
        return index
    return linear_search_custom(arr, comparator, index+1)
```

### Code
```python
def linear_search_custom(arr, comparator, index=0):
    # Base case: not found
    if index == len(arr):
        return -1
    # Use comparator to check match
    if comparator(arr[index]):
        return index
    # Recursive call
    return linear_search_custom(arr, comparator, index + 1)

# Example: find first even number
arr = [3, 7, 4, 9, 2]
comp = lambda x: x % 2 == 0
print(linear_search_custom(arr, comp))  # Output: 2 (value 4)

# Example: find person with age > 25
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
people = [Person("Alice", 22), Person("Bob", 30), Person("Charlie", 25)]
comp_age = lambda p: p.age > 25
result = linear_search_custom(people, comp_age)
print(people[result].name if result != -1 else "Not found")  # Output: Bob
```

---

## 10. Recursive Linear Search on a String
### Explanation
This variant searches for a character in a string using recursion. Strings are immutable sequences, so similar logic applies.

### Approach
- Treat the string as a sequence of characters.
- Recursively check each character from a given index.

### Algorithm
1. Define function `linear_search_string(s, char, index=0)`.
2. Base case: if `index == len(s)`, return -1.
3. If `s[index] == char`, return `index`.
4. Recursive case: return `linear_search_string(s, char, index+1)`.

### Pseudocode
```
function linear_search_string(s, char, index=0):
    if index == len(s):
        return -1
    if s[index] == char:
        return index
    return linear_search_string(s, char, index+1)
```

### Code
```python
def linear_search_string(s, char, index=0):
    # Base case: end of string
    if index == len(s):
        return -1
    # Check current character
    if s[index] == char:
        return index
    # Recursive call
    return linear_search_string(s, char, index + 1)

# Example
text = "hello world"
character = 'o'
print(linear_search_string(text, character))  # Output: 4 (first 'o')
```