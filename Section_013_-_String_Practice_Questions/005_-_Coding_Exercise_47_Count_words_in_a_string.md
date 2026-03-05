## Using split() (Original)
```python
def count_words(s):
    # Main logic: split string on whitespace (default) and count resulting list
    return len(s.split())  # Return number of words

# Call the function and print output
result = count_words("Hello world, how are you?")
print(result)  # Output: 5
```

## Manual Loop with State Flag
```python
def count_words(s):
    count = 0
    in_word = False  # Flag to track if currently inside a word
    # Main logic: iterate through characters and detect word boundaries
    for char in s:
        if char.isspace():
            in_word = False  # Space marks end of word
        else:
            if not in_word:  # Transition from space to non-space
                count += 1   # Found a new word
                in_word = True
    return count

# Call the function and print output
result = count_words("Hello world, how are you?")
print(result)  # Output: 5
```

## Using Regular Expression findall
```python
import re

def count_words(s):
    # Main logic: use regex to find all sequences of non-whitespace characters
    words = re.findall(r'\S+', s)
    return len(words)  # Return number of matches

# Call the function and print output
result = count_words("Hello world, how are you?")
print(result)  # Output: 5
```

## Using split() with Explicit Whitespace Handling
```python
def count_words(s):
    # Main logic: split on any whitespace and filter out empty strings (if multiple spaces)
    return len([word for word in s.split() if word])  # List comprehension ensures non-empty

# Call the function and print output
result = count_words("Hello world,  how are you?")
print(result)  # Output: 5
```

## Counting Spaces and Adding One (Simple but Limited)
```python
def count_words(s):
    # Main logic: count spaces and add 1 (assumes words separated by single spaces, no leading/trailing)
    # This is a simplistic version and fails on multiple spaces or empty string
    if not s.strip():  # Handle empty or whitespace-only string
        return 0
    return s.count(' ') + 1

# Call the function and print output
result = count_words("Hello world how are you")  # No punctuation, single spaces
print(result)  # Output: 5
```

## Using Regular Expression sub and Counting Spaces
```python
import re

def count_words(s):
    # Main logic: replace multiple spaces with single space, then count spaces and add 1
    s = re.sub(r'\s+', ' ', s.strip())  # Normalize spaces
    if not s:  # Empty string after stripping
        return 0
    return s.count(' ') + 1

# Call the function and print output
result = count_words("Hello   world,  how are you?")
print(result)  # Output: 5
```

## Using collections.Counter on Split Words
```python
from collections import Counter

def count_words(s):
    # Main logic: split into words, create Counter (counts occurrences), then sum counts
    words = s.split()
    word_counts = Counter(words)  # Counter of word frequencies
    return sum(word_counts.values())  # Total words = sum of frequencies

# Call the function and print output
result = count_words("Hello world, how are you?")
print(result)  # Output: 5
```

## Using reduce to Count Spaces
```python
from functools import reduce

def count_words(s):
    # Main logic: reduce processes string, counting transitions from space to non-space
    # Start with count 0 and last_char as space to detect first word
    return reduce(lambda acc, char: (acc[0] + (1 if acc[1] and not char.isspace() else 0), char.isspace()), 
                  s, (0, True))[0]

# Call the function and print output
result = count_words("Hello world, how are you?")
print(result)  # Output: 5
```

## Using Enumerate and Checking Character Classes
```python
def count_words(s):
    count = 0
    # Main logic: check each character; a word starts when current char is non-space and previous was space (or start)
    for i, char in enumerate(s):
        if not char.isspace() and (i == 0 or s[i-1].isspace()):
            count += 1
    return count

# Call the function and print output
result = count_words("Hello world, how are you?")
print(result)  # Output: 5
```

## Using str.find in a Loop
```python
def count_words(s):
    count = 0
    i = 0
    n = len(s)
    # Main logic: use str.find to locate next space and count words between
    while i < n:
        # Skip spaces
        while i < n and s[i].isspace():
            i += 1
        if i >= n:
            break
        # Found start of a word
        count += 1
        # Find next space
        while i < n and not s[i].isspace():
            i += 1
    return count

# Call the function and print output
result = count_words("Hello world, how are you?")
print(result)  # Output: 5
```