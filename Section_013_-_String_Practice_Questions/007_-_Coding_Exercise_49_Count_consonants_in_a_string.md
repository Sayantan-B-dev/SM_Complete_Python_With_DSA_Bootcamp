## Using sum with generator expression
```python
def count_consonants(s):
    v = "aeiouAEIOU"
    # Main logic: generator yields 1 for each char that is alphabetic and not a vowel
    # sum() adds them up to get total count
    return sum(1 for c in s if c.isalpha() and c not in v)

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using filter and len
```python
def count_consonants(s):
    v = "aeiouAEIOU"
    # Main logic: filter() keeps only characters that are consonants (alphabetic and not vowel)
    # Convert filter object to list and get length
    return len(list(filter(lambda c: c.isalpha() and c not in v, s)))

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using list comprehension and len
```python
def count_consonants(s):
    v = "aeiouAEIOU"
    # Main logic: create list of consonants using list comprehension
    consonants = [c for c in s if c.isalpha() and c not in v]
    return len(consonants)  # Return length of list

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using regular expression findall
```python
import re

def count_consonants(s):
    # Main logic: regex matches consonants: letters not in [aeiouAEIOU]
    # re.findall returns list of matches, len gives count
    return len(re.findall(r'(?i)[^aeiou]', re.sub(r'[^a-z]', '', s, flags=re.I)))

# Alternative simpler: match all letters that are not vowels (case-insensitive)
# r'(?i)[b-df-hj-np-tv-z]' matches consonants directly
# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using str.count in a loop over consonants
```python
def count_consonants(s):
    # Define all consonants (lower and upper)
    consonants = "bcdfghjklmnpqrstvwxyzBCDFGHJKLMNPQRSTVWXYZ"
    total = 0
    # Main logic: iterate over each consonant and count occurrences in s
    for c in consonants:
        total += s.count(c)
    return total

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using set of consonants for membership check
```python
def count_consonants(s):
    # Precompute set of consonants for O(1) lookup
    consonants = set("bcdfghjklmnpqrstvwxyzBCDFGHJKLMNPQRSTVWXYZ")
    count = 0
    # Main logic: iterate and check if char in consonant set
    for c in s:
        if c in consonants:
            count += 1
    return count

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using map with lambda and sum
```python
def count_consonants(s):
    v = "aeiouAEIOU"
    # Main logic: map each character to 1 if consonant else 0, then sum
    return sum(map(lambda c: 1 if c.isalpha() and c not in v else 0, s))

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using reduce from functools
```python
from functools import reduce

def count_consonants(s):
    v = "aeiouAEIOU"
    # Main logic: reduce accumulates count, adding 1 for each consonant
    return reduce(lambda acc, c: acc + (1 if c.isalpha() and c not in v else 0), s, 0)

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using manual while loop with index
```python
def count_consonants(s):
    v = "aeiouAEIOU"
    count = 0
    i = 0
    # Main logic: while loop iterates over indices
    while i < len(s):
        if s[i].isalpha() and s[i] not in v:
            count += 1
        i += 1
    return count

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```

## Using collections.Counter to subtract vowels from letters
```python
from collections import Counter

def count_consonants(s):
    # Count all alphabetic characters
    letters = [c for c in s if c.isalpha()]
    total_letters = len(letters)
    # Count vowels (case-sensitive)
    v = "aeiouAEIOU"
    vowel_count = sum(Counter(letters)[c] for c in v if c in Counter(letters))
    # Main logic: consonants = total letters - vowels
    return total_letters - vowel_count

# Call the function and print output
result = count_consonants("Hello World")
print(result)  # Output: 7
```