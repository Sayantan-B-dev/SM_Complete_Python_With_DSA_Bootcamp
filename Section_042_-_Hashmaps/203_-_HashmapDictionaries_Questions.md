### Hashmap Applications: Three Core Problems

The following sections analyze three fundamental problems solvable with hashmap-based approaches. Each includes an algorithmic breakdown, implementation with inline commentary, and complexity analysis.

---

## Problem 1: Intersection of Two Arrays (with Duplicate Handling)

Given two integer arrays, return an array containing the intersection of the two arrays, where each element appears as many times as it appears in both arrays.

### Algorithmic Approach

A hashmap is used to count frequencies of elements from the first array. Then, iterating through the second array, elements that have a remaining positive count are added to the result, and the count is decremented. This ensures that duplicates are handled correctly.

### Implementation

```python
def intersection_of_arrays(arr1: list, arr2: list) -> list:
    """
    Returns the intersection of two arrays, preserving duplicate counts.
    
    Steps:
    1. Build a frequency map of arr1.
    2. Traverse arr2; for each element, if it exists in the map with count > 0,
       add to result and decrement count.
    3. Return the result list.
    """
    freq = {}
    # Count occurrences in first array
    for num in arr1:
        freq[num] = freq.get(num, 0) + 1

    result = []
    for num in arr2:
        if freq.get(num, 0) > 0:
            result.append(num)
            freq[num] -= 1   # Consume one occurrence
    return result
```

### Complexity Analysis

- **Time**: O(m + n), where m = len(arr1), n = len(arr2). Each element is processed once.
- **Space**: O(m) for the frequency map.

### Edge Cases

- One or both arrays empty → returns [].
- Duplicate elements: handled via count decrement.
- Elements not present in arr1 → ignored.
- Unhashable types (e.g., nested lists) – not applicable as inputs are integers.

---

## Problem 2: Contains Duplicate

Determine whether an array contains any duplicate elements.

### Algorithmic Approach

A hashmap (or set) records each element as it is encountered. If an element is already present in the map, a duplicate exists.

### Implementation

```python
def contains_duplicates(nums: list) -> bool:
    """
    Returns True if any value appears at least twice, else False.
    """
    seen = {}
    for num in nums:
        if num in seen:
            return True
        seen[num] = True   # Mark as seen
    return False
```

A set can be used instead of a dict for clarity; the dict version is shown for consistency with the previous pattern.

### Complexity Analysis

- **Time**: O(n) worst case (no duplicates, traverses entire array).
- **Space**: O(n) for the hash table.

### Edge Cases

- Empty list → False.
- Single element → False.
- Large inputs with high cardinality → memory usage proportional to unique elements.

---

## Problem 3: First Non‑Repeating Character in a String

Given a string, find the first character that does not repeat (i.e., appears exactly once).

### Algorithmic Approach

Two passes:
1. First pass: count occurrences of each character using a hashmap.
2. Second pass: iterate through the string in order; return the first character whose count is 1.

### Implementation

```python
def first_non_repeating_char(s: str) -> str or None:
    """
    Returns the first character that appears exactly once, or None if none.
    """
    count = {}
    # Count frequency
    for ch in s:
        count[ch] = count.get(ch, 0) + 1

    # Find first with count == 1
    for ch in s:
        if count[ch] == 1:
            return ch
    return None
```

### Complexity Analysis

- **Time**: O(n), where n is the string length.
- **Space**: O(k), where k is the number of distinct characters (bounded by alphabet size or Unicode range).

### Edge Cases

- Empty string → None.
- All characters repeat → None.
- Single character → returns that character.
- Unicode characters: the algorithm works because Python strings are sequences of Unicode code points.

---

### Summary of Hashmap Usage

| Problem                 | Key           | Value             | Operation Pattern                          |
|-------------------------|---------------|-------------------|---------------------------------------------|
| Intersection            | Array element | Frequency count   | Build map, then consume counts              |
| Contains Duplicate      | Array element | Presence boolean  | Early termination on collision              |
| First Non‑Repeating Char| Character     | Frequency count   | Two passes: count then scan                 |

All three problems leverage the average O(1) lookup and insertion of hashmaps to achieve linear time complexity, avoiding the O(n²) overhead of nested loops.