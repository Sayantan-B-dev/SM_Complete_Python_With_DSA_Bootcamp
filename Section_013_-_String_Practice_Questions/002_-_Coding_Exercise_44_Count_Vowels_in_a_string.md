## Using filter and lambda (Original)
```python
def count_vowels(s):
    # Main logic: filter characters that are vowels (case-insensitive)
    vowels = filter(lambda c: c.lower() in "aeiou", s)
    # Return the count by converting filter object to list and getting length
    return len(list(vowels))

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using sum with generator expression
```python
def count_vowels(s):
    # Main logic: generator expression yields 1 for each vowel, sum adds them
    return sum(c.lower() in "aeiou" for c in s)

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using for loop with counter
```python
def count_vowels(s):
    count = 0                     # Initialize counter
    # Main logic: iterate over each character
    for char in s:
        if char.lower() in "aeiou":  # Check if character is a vowel
            count += 1                # Increment counter
    return count                  # Return final count

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using list comprehension and len
```python
def count_vowels(s):
    # Main logic: create list of vowels using list comprehension
    vowels = [c for c in s if c.lower() in "aeiou"]
    return len(vowels)            # Return length of list

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using regular expression findall
```python
import re

def count_vowels(s):
    # Main logic: use regex to find all vowels (case-insensitive)
    vowels = re.findall(r'[aeiou]', s, re.IGNORECASE)
    return len(vowels)            # Return number of matches

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using str.count in a loop over vowels
```python
def count_vowels(s):
    vowels = "aeiou"
    total = 0
    # Main logic: for each vowel, count occurrences (case-insensitive)
    for v in vowels:
        total += s.lower().count(v)  # Count current vowel in lowercased string
    return total                   # Return accumulated count

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using collections.Counter
```python
from collections import Counter

def count_vowels(s):
    # Main logic: count all characters, then sum counts of vowels
    counts = Counter(s.lower())    # Case-insensitive by lowercasing
    vowels = "aeiou"
    # Sum counts for each vowel (default 0 if not present)
    return sum(counts.get(v, 0) for v in vowels)

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using recursion
```python
def count_vowels(s):
    # Base case: empty string has no vowels
    if not s:
        return 0
    # Main logic: check first char, add 1 if vowel, then recurse on rest
    first_is_vowel = 1 if s[0].lower() in "aeiou" else 0
    return first_is_vowel + count_vowels(s[1:])

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using map and sum
```python
def count_vowels(s):
    # Main logic: map each character to 1 if vowel else 0, then sum
    return sum(map(lambda c: 1 if c.lower() in "aeiou" else 0, s))

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```

## Using reduce from functools
```python
from functools import reduce

def count_vowels(s):
    # Main logic: reduce accumulates count by adding 1 for each vowel
    return reduce(lambda acc, c: acc + (1 if c.lower() in "aeiou" else 0), s, 0)

# Call the function and print output
result = count_vowels("Hello World")
print(result)  # Output: 3
```