## String Reversal (Original)
```python
def is_palindrome(s):
    # Normalize: lowercase and remove spaces
    s = "".join(s.lower().split())
    # Main logic: compare string with its reverse
    return s == s[::-1]  # Return result of equality check

# Call the function and print output
result = is_palindrome("A man a plan a canal Panama")
print(result)  # Output: True
```

## Two-Pointer Technique
```python
def is_palindrome(s):
    # Normalize: lowercase and remove spaces
    s = "".join(s.lower().split())
    left, right = 0, len(s) - 1  # Initialize pointers
    # Main logic: move pointers towards center, comparing characters
    while left < right:
        if s[left] != s[right]:
            return False  # Mismatch found, not a palindrome
        left += 1
        right -= 1
    return True  # All characters matched

# Call the function and print output
result = is_palindrome("A man a plan a canal Panama")
print(result)  # Output: True
```

## Recursive Comparison
```python
def is_palindrome(s):
    # Normalize: lowercase and remove spaces
    s = "".join(s.lower().split())
    
    def check_palindrome(sub):
        # Base case: empty or single character is palindrome
        if len(sub) <= 1:
            return True
        # Main logic: compare first and last, recurse on middle
        return sub[0] == sub[-1] and check_palindrome(sub[1:-1])
    
    return check_palindrome(s)

# Call the function and print output
result = is_palindrome("A man a plan a canal Panama")
print(result)  # Output: True
```

## Using Deque from Collections
```python
from collections import deque

def is_palindrome(s):
    # Normalize: lowercase and remove spaces
    s = "".join(s.lower().split())
    dq = deque(s)  # Create deque from normalized string
    # Main logic: pop from both ends and compare
    while len(dq) > 1:
        if dq.popleft() != dq.pop():
            return False  # Mismatch, not a palindrome
    return True  # All pairs matched

# Call the function and print output
result = is_palindrome("A man a plan a canal Panama")
print(result)  # Output: True
```

## For Loop with Index Comparison
```python
def is_palindrome(s):
    # Normalize: lowercase and remove spaces
    s = "".join(s.lower().split())
    n = len(s)
    # Main logic: compare first half with second half in reverse
    for i in range(n // 2):
        if s[i] != s[n - 1 - i]:
            return False  # Mismatch found
    return True  # All matched

# Call the function and print output
result = is_palindrome("A man a plan a canal Panama")
print(result)  # Output: True
```

## Using all() and zip() with Reversed
```python
def is_palindrome(s):
    # Normalize: lowercase and remove spaces
    s = "".join(s.lower().split())
    # Main logic: zip string with its reverse and check all pairs
    return all(a == b for a, b in zip(s, reversed(s)))

# Call the function and print output
result = is_palindrome("A man a plan a canal Panama")
print(result)  # Output: True
```

## Normalize Using Filter and Compare Half
```python
def is_palindrome(s):
    # Normalize: lowercase and keep only alphanumeric (ignoring spaces and punctuation)
    s = ''.join(filter(str.isalnum, s.lower()))
    # Main logic: compare first half with reversed second half using slicing
    return s == s[::-1]

# Call the function and print output
result = is_palindrome("A man, a plan, a canal: Panama")
print(result)  # Output: True
```

## Stack Approach (Push First Half, Pop and Compare)
```python
def is_palindrome(s):
    # Normalize: lowercase and remove spaces
    s = "".join(s.lower().split())
    n = len(s)
    stack = list(s[:n//2])  # Push first half onto stack
    # Main logic: compare second half with popped characters from stack
    for i in range((n+1)//2, n):
        if s[i] != stack.pop():
            return False  # Mismatch
    return True  # All matched

# Call the function and print output
result = is_palindrome("A man a plan a canal Panama")
print(result)  # Output: True
```

## Using Regular Expression to Normalize
```python
import re

def is_palindrome(s):
    # Normalize: lowercase and remove non-alphanumeric characters using regex
    s = re.sub(r'[^a-z0-9]', '', s.lower())
    # Main logic: compare with reverse
    return s == s[::-1]

# Call the function and print output
result = is_palindrome("A man, a plan, a canal: Panama")
print(result)  # Output: True
```

## Functional Approach with reduce
```python
from functools import reduce

def is_palindrome(s):
    # Normalize: lowercase and remove spaces
    s = "".join(s.lower().split())
    # Main logic: reduce to check if all pairs match
    # reduce accumulates a boolean (True so far) and updates by comparing each pair
    return reduce(lambda acc, i: acc and (s[i] == s[len(s)-1-i]), range(len(s)//2), True)

# Call the function and print output
result = is_palindrome("A man a plan a canal Panama")
print(result)  # Output: True
```