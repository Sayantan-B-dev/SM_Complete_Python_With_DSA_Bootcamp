## Using max() with key=len
```python
def longest_word_length(s):
    # Main logic: split into words, then use max() with key=len to find longest word
    words = s.split()
    # Handle empty string case: if no words, max would raise ValueError, so default
    return len(max(words, key=len)) if words else 0

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5 (jumps or quick? both length 5)
```

## Using sorted() with reverse
```python
def longest_word_length(s):
    # Main logic: sort words by length descending, take first word's length
    words = s.split()
    if not words:
        return 0
    # sorted returns list sorted by length (largest first)
    sorted_words = sorted(words, key=len, reverse=True)
    return len(sorted_words[0])

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```

## Using reduce() from functools
```python
from functools import reduce

def longest_word_length(s):
    # Main logic: reduce over words, keeping the maximum length so far
    words = s.split()
    if not words:
        return 0
    # reduce applies lambda that returns max of current max and current word length
    return reduce(lambda max_len, word: max(max_len, len(word)), words, 0)

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```

## Using list comprehension to get lengths then max
```python
def longest_word_length(s):
    # Main logic: create list of word lengths using list comprehension, then take max
    lengths = [len(word) for word in s.split()]
    # max() works on empty list? It raises ValueError, so handle empty
    return max(lengths) if lengths else 0

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```

## Using manual for loop (original style)
```python
def longest_word_length(s):
    # Main logic: iterate through words, update max_length when larger word found
    words = s.split()
    max_length = 0
    for word in words:
        if len(word) > max_length:
            max_length = len(word)
    return max_length

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```

## Using while loop with index
```python
def longest_word_length(s):
    # Main logic: split into words, then use while loop to find max length
    words = s.split()
    max_length = 0
    i = 0
    while i < len(words):
        if len(words[i]) > max_length:
            max_length = len(words[i])
        i += 1
    return max_length

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```

## Using map() to get lengths then max
```python
def longest_word_length(s):
    # Main logic: map len function to each word, then get max
    lengths = map(len, s.split())
    # max() on map object works, but handle empty
    return max(lengths, default=0)

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```

## Using regular expression to find words
```python
import re

def longest_word_length(s):
    # Main logic: use regex to find all word sequences (alphanumeric)
    words = re.findall(r'\b\w+\b', s)  # \b for word boundaries
    max_length = 0
    for word in words:
        if len(word) > max_length:
            max_length = len(word)
    return max_length

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```

## Using recursion (split on spaces)
```python
def longest_word_length(s):
    # Recursive approach: split into first word and rest, compare lengths
    words = s.split()
    if not words:
        return 0
    # Helper recursion
    def find_max(idx, current_max):
        if idx >= len(words):
            return current_max
        current_max = max(current_max, len(words[idx]))
        return find_max(idx + 1, current_max)
    return find_max(0, 0)

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```

## Using generator expression with max
```python
def longest_word_length(s):
    # Main logic: generator yields length of each word, max picks the largest
    return max((len(word) for word in s.split()), default=0)

# Call the function and print output
result = longest_word_length("The quick brown fox jumps over the lazy dog")
print(result)  # Output: 5
```