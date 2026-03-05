## Two-Pointer While Loop
```python
def is_subsequence(s, t):
    i = j = 0  # Initialize pointers for s and t
    # Main logic: move both pointers while comparing characters
    while i < len(s) and j < len(t):
        if s[i] == t[j]:
            i += 1  # Found match in s, move s pointer
        j += 1  # Always move t pointer
    # Return True if all characters in s were matched
    return i == len(s)

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Using iter() and all() with next()
```python
def is_subsequence(s, t):
    # Create iterator over t
    t_iter = iter(t)
    # Main logic: for each char in s, find its next occurrence in t
    # all() ensures every char is found; next() raises StopIteration if not found
    return all(c in t_iter for c in s)  # Special: 'in' with iterator consumes until match

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Using str.find() in a loop
```python
def is_subsequence(s, t):
    pos = -1  # Starting position before first character
    # Main logic: for each char in s, find its index in t after current position
    for c in s:
        pos = t.find(c, pos + 1)  # Search from next index
        if pos == -1:
            return False  # Character not found in remaining part
    return True  # All characters found in order

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Using index() with slicing
```python
def is_subsequence(s, t):
    idx = 0  # Starting index in t
    # Main logic: use str.index() to find each char, slice t to advance
    for c in s:
        try:
            idx = t.index(c, idx) + 1  # Find and move past this character
        except ValueError:
            return False  # Character not found
    return True

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Recursive Approach
```python
def is_subsequence(s, t):
    # Base case: if s is empty, it's a subsequence
    if not s:
        return True
    # Base case: if t is empty but s is not, not a subsequence
    if not t:
        return False
    # Main logic: if first chars match, recurse on rest; else, recurse on t without first char
    if s[0] == t[0]:
        return is_subsequence(s[1:], t[1:])
    else:
        return is_subsequence(s, t[1:])

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Using Regular Expression
```python
import re

def is_subsequence(s, t):
    # Build pattern: insert '.*' between characters to allow any characters in between
    pattern = '.*'.join(re.escape(c) for c in s)  # Escape in case of special chars
    # Main logic: search for pattern in t
    return bool(re.search(pattern, t))

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Using deque for efficient popping
```python
from collections import deque

def is_subsequence(s, t):
    # Convert s to a deque for left pops
    s_deque = deque(s)
    # Main logic: iterate through t, if front of s_deque matches, pop it
    for char in t:
        if s_deque and char == s_deque[0]:
            s_deque.popleft()  # Remove matched character
    # If deque is empty, all matched
    return not s_deque

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Using List Comprehension and join (Non-standard but clever)
```python
def is_subsequence(s, t):
    # Create a list of characters from t that are in s, preserving order
    filtered = [c for c in t if c in set(s)]
    # Main logic: check if s is a prefix of the joined filtered string
    return ''.join(filtered).startswith(s)

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Using While Loop with Manual Index (Original style)
```python
def is_subsequence(s, t):
    i = 0  # Index for s
    # Main logic: iterate through t with index j
    for j in range(len(t)):
        if i < len(s) and s[i] == t[j]:
            i += 1  # Move s pointer when match found
    return i == len(s)

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```

## Using functools.reduce
```python
from functools import reduce

def is_subsequence(s, t):
    # Reduce over t, maintaining the index in s that we have matched so far
    # accumulator is the next index to match in s
    def match(acc, char):
        if acc < len(s) and s[acc] == char:
            return acc + 1
        return acc
    final_index = reduce(match, t, 0)
    return final_index == len(s)

# Call the function and print output
result = is_subsequence("abc", "ahbgdc")
print(result)  # Output: True
```