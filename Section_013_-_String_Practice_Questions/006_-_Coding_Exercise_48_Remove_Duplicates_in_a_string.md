## Using Set and Loop (Original Variant)
```python
def remove_duplicates(s):
    seen = set()                # Track characters already encountered
    result = ""                  # Build result string
    # Main logic: iterate through characters, add if not seen
    for c in s:
        if c not in seen:        # Check if character is new
            result += c           # Append to result
            seen.add(c)           # Mark as seen
    return result                 # Return string without duplicates

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using OrderedDict from collections
```python
from collections import OrderedDict

def remove_duplicates(s):
    # Main logic: OrderedDict remembers insertion order, keys are unique
    # Special thing: fromkeys creates dictionary with keys from iterable, values None
    return ''.join(OrderedDict.fromkeys(s).keys())  # Join keys to form result

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using dict (Python 3.7+ preserves insertion order)
```python
def remove_duplicates(s):
    # Main logic: dict.fromkeys(s) creates dict with characters as keys (order preserved)
    # Special thing: dict keys maintain insertion order (Python 3.7+)
    return ''.join(dict.fromkeys(s).keys())  # Join keys to get unique chars in order

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using List and Conditional Append
```python
def remove_duplicates(s):
    seen = []                    # Use list to track seen characters
    # Main logic: iterate and append if not already in list
    for c in s:
        if c not in seen:        # Check membership in list (O(n) per check)
            seen.append(c)        # Add to list
    return ''.join(seen)          # Convert list back to string

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using List Comprehension with index check
```python
def remove_duplicates(s):
    # Main logic: keep character if its first occurrence index equals current index
    # Special thing: uses enumerate and s.index to find first occurrence
    return ''.join([s[i] for i in range(len(s)) if i == s.index(s[i])])

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using filter with lambda and index check
```python
def remove_duplicates(s):
    # Main logic: filter characters where current index equals first occurrence index
    # Special thing: uses enumerate to get indices and filter based on s.find (first occurrence)
    return ''.join(filter(lambda x: s.index(x[1]) == x[0], enumerate(s)))

# Alternative using char and index separately
# return ''.join([c for i, c in enumerate(s) if s.find(c) == i])

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using Recursion
```python
def remove_duplicates(s):
    # Base case: if string is empty or single char, return itself
    if len(s) <= 1:
        return s
    # Main logic: if first char is in rest, skip it; else keep it
    if s[0] in s[1:]:
        return remove_duplicates(s[1:])  # First char is duplicate, recurse on rest
    else:
        return s[0] + remove_duplicates(s[1:])  # Keep first char, recurse on rest

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using Manual Loop with Result List and Join
```python
def remove_duplicates(s):
    seen = set()                  # Use set for O(1) membership check
    result_chars = []              # Use list for efficient appending
    # Main logic: iterate, add to result if not seen
    for c in s:
        if c not in seen:
            result_chars.append(c)  # Append to list
            seen.add(c)              # Add to set
    return ''.join(result_chars)    # Join list into string

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using str.join with generator and conditional (set)
```python
def remove_duplicates(s):
    seen = set()
    # Main logic: generator yields character if not seen, and adds to seen
    # Special thing: using set.add() within generator expression
    return ''.join(c for c in s if c not in seen and not seen.add(c))

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using While Loop and String Slicing
```python
def remove_duplicates(s):
    result = ""
    i = 0
    # Main logic: iterate with index, if character not in result so far, append
    while i < len(s):
        if s[i] not in result:
            result += s[i]        # Build result incrementally
        i += 1
    return result

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```

## Using reduce from functools
```python
from functools import reduce

def remove_duplicates(s):
    # Main logic: reduce accumulates result string, adding char if not already present
    # Special thing: uses lambda that checks membership in accumulator
    return reduce(lambda acc, c: acc + c if c not in acc else acc, s, "")

# Call the function and print output
result = remove_duplicates("aabbccddee")
print(result)  # Output: abcde
```