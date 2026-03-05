## Using `in` Operator
```python
def is_substring(s, t):
    # Main logic: Python's 'in' operator checks if substring exists
    return t in s  # Return boolean result

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Using `str.find()`
```python
def is_substring(s, t):
    # Main logic: find() returns index of first occurrence or -1 if not found
    return s.find(t) != -1  # Return True if found

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Using `str.index()` with Exception Handling
```python
def is_substring(s, t):
    # Main logic: index() raises ValueError if not found
    try:
        s.index(t)  # Attempt to find substring
        return True  # Found, no exception
    except ValueError:
        return False  # Not found

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Using `str.startswith()` in a Loop
```python
def is_substring(s, t):
    length_t = len(t)
    # Main logic: iterate through possible start positions
    for i in range(len(s) - length_t + 1):
        if s.startswith(t, i):  # Check if substring starts at i
            return True
    return False

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Manual Two-Pointer Sliding Window (No Slicing)
```python
def is_substring(s, t):
    len_s, len_t = len(s), len(t)
    if len_t > len_s:
        return False
    # Main logic: compare character by character at each position
    for i in range(len_s - len_t + 1):
        match = True
        for j in range(len_t):
            if s[i + j] != t[j]:
                match = False
                break
        if match:
            return True
    return False

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Using `all()` with Generator and Slicing
```python
def is_substring(s, t):
    len_t = len(t)
    # Main logic: use all() to check if any slice matches
    # any() stops at first True
    return any(s[i:i+len_t] == t for i in range(len(s) - len_t + 1))

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Using Regular Expression (`re.search()`)
```python
import re

def is_substring(s, t):
    # Main logic: compile pattern (escape special chars) and search
    pattern = re.escape(t)  # Handle special characters in substring
    return bool(re.search(pattern, s))

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Using `collections.deque` as Sliding Window
```python
from collections import deque

def is_substring(s, t):
    len_t = len(t)
    if len_t > len(s):
        return False
    # Create a deque from first len_t characters of s
    window = deque(s[:len_t], maxlen=len_t)
    # Main logic: slide window and compare with t
    if ''.join(window) == t:
        return True
    for i in range(len_t, len(s)):
        window.append(s[i])  # Slide window: remove left, add right
        if ''.join(window) == t:
            return True
    return False

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Simplified KMP (Knuth-Morris-Pratt) Algorithm
```python
def is_substring(s, t):
    # KMP algorithm for pattern matching
    if not t:
        return True
    len_s, len_t = len(s), len(t)
    if len_t > len_s:
        return False
    
    # Build LPS (Longest Prefix Suffix) array for t
    lps = [0] * len_t
    length = 0
    i = 1
    while i < len_t:
        if t[i] == t[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    
    # Main logic: search using LPS
    i = j = 0
    while i < len_s:
        if t[j] == s[i]:
            i += 1
            j += 1
        if j == len_t:
            return True  # Found match
        elif i < len_s and t[j] != s[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    return False

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```

## Using `str.count()` (Limited Use Case)
```python
def is_substring(s, t):
    # Main logic: count occurrences, but only works if no overlaps matter
    # This returns True if count > 0, but fails with overlapping patterns? Actually count counts non-overlapping.
    # Not reliable for all cases, but shown as alternative.
    return s.count(t) > 0

# Call the function and print output
result = is_substring("hello world", "world")
print(result)  # Output: True
```