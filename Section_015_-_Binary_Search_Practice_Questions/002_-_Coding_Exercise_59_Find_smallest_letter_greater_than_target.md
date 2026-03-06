# Find Smallest Letter Greater Than Target

**Explanation:**  
Given a sorted array of characters in non‑decreasing order and a target character, the goal is to find the smallest element in the array that is lexicographically greater than the target. If no such element exists, the first element of the array is returned (wrapping around). The function uses binary search to achieve O(log n) time complexity.

**Approach:**  
Perform binary search on the sorted list. Maintain a variable to store the smallest greater letter found so far. If the middle element is greater than the target, it is a candidate; move to the left half to look for an even smaller greater letter. If the middle element is less than or equal to the target, move to the right half. At the end, return the candidate (or the first element if none found).

**Algorithm:**  
1. Set `left = 0`, `right = len(letters) - 1`.  
2. Initialize `result = letters[0]` (default for wrap‑around).  
3. While `left <= right`:  
   - Compute `mid = (left + right) // 2`.  
   - If `letters[mid] > target`:  
        - Update `result = letters[mid]`.  
        - Move left: `right = mid - 1` (search for a smaller greater letter).  
   - Else (`letters[mid] <= target`):  
        - Move right: `left = mid + 1`.  
4. Return `result`.

**Pseudocode:**  
```
function next_greatest_letter(letters, target):
    left = 0
    right = length(letters) - 1
    result = letters[0]

    while left <= right:
        mid = (left + right) // 2
        if letters[mid] > target:
            result = letters[mid]
            right = mid - 1
        else:
            left = mid + 1

    return result
```

## Linear Scan
```python
def next_greatest_letter(letters, target):
    # Main logic: iterate through sorted list and return first letter greater than target
    for letter in letters:
        if letter > target:
            return letter  # Return the first greater letter found
    # If no greater letter found, return the first letter (wrap around)
    return letters[0]

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'a')
print(result)  # Output: c
```

## Using bisect Module
```python
import bisect

def next_greatest_letter(letters, target):
    # Main logic: use bisect_right to find insertion point of target
    # bisect_right returns index where target would be inserted to maintain order
    pos = bisect.bisect_right(letters, target)
    # If position is at the end, wrap to first element
    return letters[pos % len(letters)]

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'c')
print(result)  # Output: f
```

## Using next() with Generator
```python
def next_greatest_letter(letters, target):
    # Main logic: generator yields letters greater than target, next() returns first
    # If none, StopIteration occurs, then return first letter
    return next((c for c in letters if c > target), letters[0])

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'f')
print(result)  # Output: j
```

## Using filter and min
```python
def next_greatest_letter(letters, target):
    # Main logic: filter letters greater than target, then take the smallest
    greater = list(filter(lambda x: x > target, letters))
    # If any greater exist, return the smallest (since sorted, first is smallest)
    # Otherwise return first letter
    return greater[0] if greater else letters[0]

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'd')
print(result)  # Output: f
```

## Recursive Binary Search
```python
def next_greatest_letter(letters, target):
    # Helper recursive function
    def search(left, right, result):
        if left > right:
            return result
        mid = (left + right) // 2
        if letters[mid] > target:
            # Potential candidate, search left half for smaller greater letter
            return search(left, mid - 1, letters[mid])
        else:
            # Need to go right
            return search(mid + 1, right, result)
    # Initial result is first letter (wrap-around candidate)
    return search(0, len(letters) - 1, letters[0])

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'g')
print(result)  # Output: j
```

## Two-Pointer Scan from Start
```python
def next_greatest_letter(letters, target):
    i = 0
    # Main logic: scan until we find a letter greater than target
    while i < len(letters) and letters[i] <= target:
        i += 1
    # If we reached the end, return first; otherwise return letters[i]
    return letters[i % len(letters)]

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'j')
print(result)  # Output: c
```

## List Comprehension and First Element
```python
def next_greatest_letter(letters, target):
    # Main logic: create list of letters greater than target, take first element
    greater = [c for c in letters if c > target]
    return greater[0] if greater else letters[0]

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'b')
print(result)  # Output: c
```

## For Loop with Break
```python
def next_greatest_letter(letters, target):
    result = letters[0]  # Default wrap-around
    # Main logic: loop through, if found greater, set result and break
    for c in letters:
        if c > target:
            result = c
            break
    return result

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'f')
print(result)  # Output: j
```

## While Loop with Index Increment
```python
def next_greatest_letter(letters, target):
    i = 0
    n = len(letters)
    # Main logic: move index while current letter <= target
    while i < n and letters[i] <= target:
        i += 1
    # If i reached n, wrap to 0
    return letters[i % n]

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'c')
print(result)  # Output: f
```

## Binary Search without Result Variable (Direct Return)
```python
def next_greatest_letter(letters, target):
    left, right = 0, len(letters) - 1
    # Main logic: binary search to find the smallest greater element
    while left <= right:
        mid = (left + right) // 2
        if letters[mid] > target:
            # We found a candidate, but there might be smaller greater on left
            right = mid - 1
        else:
            left = mid + 1
    # After loop, left is the index where target would be inserted to keep order
    # If left == len(letters), wrap to 0
    return letters[left % len(letters)]

# Call the function and print output
result = next_greatest_letter(['c','f','j'], 'z')
print(result)  # Output: c
```