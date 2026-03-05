## Slicing
```python
def reverse_string(s):
    # Main logic: use slicing with step -1 to reverse the string
    return s[::-1]  # Return the reversed string

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## For Loop Concatenation
```python
def reverse_string(s):
    reversed_str = ""          # Initialize empty string to build result
    # Main logic: iterate over original string in reverse order
    for char in s:
        reversed_str = char + reversed_str  # Prepend each character
    return reversed_str        # Return the built reversed string

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## While Loop
```python
def reverse_string(s):
    reversed_str = ""          # Initialize empty result
    index = len(s) - 1         # Start from last index
    # Main logic: loop backwards through indices
    while index >= 0:
        reversed_str += s[index]  # Append character at current index
        index -= 1                 # Move to previous index
    return reversed_str        # Return reversed string

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## Using `reversed()` and `join()`
```python
def reverse_string(s):
    # Main logic: reversed(s) returns iterator in reverse order, join into string
    return ''.join(reversed(s))  # Return concatenated reversed characters

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## Recursion
```python
def reverse_string(s):
    # Base case: if string is empty or single char, return itself
    if len(s) <= 1:
        return s
    # Main logic: recursively reverse substring excluding first char, then append first char
    return reverse_string(s[1:]) + s[0]  # Return reversed rest + first character

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## Using Stack (List as Stack)
```python
def reverse_string(s):
    stack = list(s)            # Push all characters onto stack (list)
    reversed_str = ""
    # Main logic: pop from stack (LIFO) to get characters in reverse order
    while stack:
        reversed_str += stack.pop()  # Pop last element and append
    return reversed_str        # Return reversed string

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## Using `reduce()` from `functools`
```python
from functools import reduce

def reverse_string(s):
    # Main logic: reduce applies lambda that prepends each char to accumulated result
    return reduce(lambda acc, char: char + acc, s, "")  # Return accumulated reversed string

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## List Comprehension with `join()`
```python
def reverse_string(s):
    # Main logic: create list of chars in reverse order using comprehension, then join
    return ''.join([s[i] for i in range(len(s)-1, -1, -1)])  # Return joined reversed list

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## Two-Pointer List Swap
```python
def reverse_string(s):
    chars = list(s)            # Convert string to mutable list
    left, right = 0, len(chars) - 1  # Initialize two pointers
    # Main logic: swap characters from ends moving inward
    while left < right:
        chars[left], chars[right] = chars[right], chars[left]  # Swap
        left += 1
        right -= 1
    return ''.join(chars)      # Return joined reversed list

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```

## Using `collections.deque`
```python
from collections import deque

def reverse_string(s):
    # Create deque from string, which supports efficient appends from both ends
    dq = deque(s)
    reversed_str = ""
    # Main logic: pop from right end to get characters in reverse order
    while dq:
        reversed_str += dq.pop()  # Pop from right (LIFO) and append
    return reversed_str        # Return reversed string

# Call the function and print output
result = reverse_string("hello")
print(result)  # Output: olleh
```