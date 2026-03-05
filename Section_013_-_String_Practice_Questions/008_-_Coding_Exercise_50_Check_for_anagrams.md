## Sorted Strings
```python
def is_anagram(s, t):
    # Normalize: remove spaces and convert to lowercase (case-insensitive)
    s = s.replace(" ", "").lower()
    t = t.replace(" ", "").lower()
    # Main logic: sorted strings should be identical if they are anagrams
    return sorted(s) == sorted(t)  # Return result of comparison

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Using collections.Counter
```python
from collections import Counter

def is_anagram(s, t):
    # Normalize: remove spaces and convert to lowercase
    s = s.replace(" ", "").lower()
    t = t.replace(" ", "").lower()
    # Main logic: Counters compare frequency dictionaries
    return Counter(s) == Counter(t)  # Return equality of Counters

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Manual Frequency Dictionary
```python
def is_anagram(s, t):
    # Normalize: remove spaces and convert to lowercase
    s = s.replace(" ", "").lower()
    t = t.replace(" ", "").lower()
    # First quick check: lengths must be equal
    if len(s) != len(t):
        return False
    # Build frequency dictionary for first string
    freq = {}
    for c in s:
        freq[c] = freq.get(c, 0) + 1
    # Check second string against the dictionary
    for c in t:
        if c not in freq:
            return False  # Character not in first string
        freq[c] -= 1
        if freq[c] < 0:
            return False  # More occurrences than in first
    return True  # All checks passed

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Array of 26 Counters (for lowercase letters only)
```python
def is_anagram(s, t):
    # Normalize: remove spaces, convert to lowercase, keep only letters
    s = ''.join(filter(str.isalpha, s.lower()))
    t = ''.join(filter(str.isalpha, t.lower()))
    if len(s) != len(t):
        return False
    # Initialize array of 26 zeros for 'a' to 'z'
    counts = [0] * 26
    # Increment for each char in s
    for c in s:
        counts[ord(c) - ord('a')] += 1
    # Decrement for each char in t
    for c in t:
        index = ord(c) - ord('a')
        counts[index] -= 1
        if counts[index] < 0:
            return False  # More occurrences in t
    # All counts should be zero
    return all(count == 0 for count in counts)

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Using all() with count comparisons
```python
def is_anagram(s, t):
    # Normalize: remove spaces and convert to lowercase
    s = s.replace(" ", "").lower()
    t = t.replace(" ", "").lower()
    # Main logic: check if every unique character in s has same count in t
    # Also need to check lengths (but counts comparison will catch if lengths differ)
    return len(s) == len(t) and all(s.count(c) == t.count(c) for c in set(s))

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Using set and count for each unique character (both strings)
```python
def is_anagram(s, t):
    # Normalize: remove spaces and convert to lowercase
    s = s.replace(" ", "").lower()
    t = t.replace(" ", "").lower()
    # Get union of unique characters from both strings
    unique_chars = set(s) | set(t)
    # Main logic: check counts for each unique character
    for c in unique_chars:
        if s.count(c) != t.count(c):
            return False
    return True

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Recursive Removal
```python
def is_anagram(s, t):
    # Normalize: remove spaces and convert to lowercase
    s = s.replace(" ", "").lower()
    t = t.replace(" ", "").lower()
    # Base case: both empty -> anagram
    if not s and not t:
        return True
    # If lengths differ, can't be anagram
    if len(s) != len(t):
        return False
    # Find first character of s in t, remove it and recurse
    c = s[0]
    if c in t:
        # Remove first occurrence of c from t
        t = t.replace(c, '', 1)
        return is_anagram(s[1:], t)
    else:
        return False

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Using reduce to build frequency map
```python
from functools import reduce

def is_anagram(s, t):
    # Normalize: remove spaces and convert to lowercase
    s = s.replace(" ", "").lower()
    t = t.replace(" ", "").lower()
    if len(s) != len(t):
        return False
    # Build frequency dict for s using reduce
    freq_s = reduce(lambda d, c: {**d, c: d.get(c, 0) + 1}, s, {})
    # Build frequency dict for t using reduce
    freq_t = reduce(lambda d, c: {**d, c: d.get(c, 0) + 1}, t, {})
    return freq_s == freq_t

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Prime Multiplication (for lowercase letters only)
```python
def is_anagram(s, t):
    # Normalize: remove spaces, convert to lowercase, keep only letters
    s = ''.join(filter(str.isalpha, s.lower()))
    t = ''.join(filter(str.isalpha, t.lower()))
    # Assign a prime number to each letter (a=2, b=3, c=5, ...)
    primes = {'a':2, 'b':3, 'c':5, 'd':7, 'e':11, 'f':13, 'g':17, 'h':19, 'i':23, 'j':29,
              'k':31, 'l':37, 'm':41, 'n':43, 'o':47, 'p':53, 'q':59, 'r':61, 's':67, 't':71,
              'u':73, 'v':79, 'w':83, 'x':89, 'y':97, 'z':101}
    # Compute product for s and t
    product_s = 1
    for c in s:
        product_s *= primes.get(c, 1)
    product_t = 1
    for c in t:
        product_t *= primes.get(c, 1)
    # Main logic: if products equal, they are anagrams (due to unique prime factorization)
    return product_s == product_t

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```

## Using str.replace in loop
```python
def is_anagram(s, t):
    # Normalize: remove spaces and convert to lowercase
    s = s.replace(" ", "").lower()
    t = t.replace(" ", "").lower()
    if len(s) != len(t):
        return False
    # Make a mutable copy of t as list
    t_list = list(t)
    # Main logic: for each char in s, remove first occurrence from t_list
    for c in s:
        try:
            index = t_list.index(c)
            t_list.pop(index)
        except ValueError:
            return False  # Character not found
    return len(t_list) == 0  # All characters matched

# Call the function and print output
result = is_anagram("listen", "silent")
print(result)  # Output: True
```