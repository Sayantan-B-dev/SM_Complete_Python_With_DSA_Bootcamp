## Basic Loop with Manual Counting
```python
def count_word_frequency(sentence):
    result = {}                            # Initialize empty dictionary for word counts
    words = sentence.lower().split(" ")     # Convert to lowercase and split on spaces
    for w in words:                         # Iterate through each word
        if w == "":                          # Skip empty strings from multiple spaces
            continue
        if w not in result:                  # Check if word not yet in dictionary
            result[w] = 1                      # First occurrence: set count to 1
        else:                                # Word already exists
            result[w] += 1                      # Increment existing count
    return result                            # Return frequency dictionary

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: Classic manual counting with dictionary
```

## Collections Counter
```python
from collections import Counter

def count_word_frequency(sentence):
    words = sentence.lower().split(" ")     # Convert to lowercase and split
    words = [w for w in words if w]          # Filter out empty strings
    return dict(Counter(words))              # Use Counter then convert to dict

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: Python's specialized Counter class for frequency counting
```

## DefaultDict
```python
from collections import defaultdict

def count_word_frequency(sentence):
    result = defaultdict(int)                # Default dict with int factory (default 0)
    words = sentence.lower().split(" ")     # Split into words
    for w in words:                         # Iterate through words
        if w:                                 # Skip empty strings
            result[w] += 1                      # Automatically handles missing keys
    return dict(result)                      # Convert to regular dict

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: defaultdict eliminates manual key checking
```

## Get Method
```python
def count_word_frequency(sentence):
    result = {}                            # Initialize dictionary
    words = sentence.lower().split(" ")     # Split into words
    for w in words:                         # Iterate through words
        if w:                                 # Skip empty strings
            result[w] = result.get(w, 0) + 1   # Use get() with default 0 and increment
    return result                            # Return frequency dict

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: dict.get() method handles missing keys gracefully
```

## Split with Multiple Spaces Handling
```python
import re

def count_word_frequency(sentence):
    result = {}                            # Initialize dictionary
    words = re.findall(r'\b\w+\b', sentence.lower())  # Use regex to find all words
    for w in words:                        # Iterate through words
        result[w] = result.get(w, 0) + 1     # Count using get method
    return result                           # Return frequency dict

print(count_word_frequency("The  cat   and  the dog  and  the bird"))  # Call with multiple spaces - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: Regex handles punctuation and multiple spaces better
```

## List Comprehension with Count (Inefficient)
```python
def count_word_frequency(sentence):
    words = sentence.lower().split(" ")     # Split into words
    words = [w for w in words if w]          # Filter empties
    return {w: words.count(w) for w in set(words)}  # Count each unique word

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: Dictionary comprehension but O(n²) complexity from repeated count()
```

## For Loop with Enumerate and Setdefault
```python
def count_word_frequency(sentence):
    result = {}                            # Initialize dictionary
    words = sentence.lower().split(" ")     # Split into words
    for w in words:                         # Iterate through words
        if w:                                 # Skip empty strings
            result.setdefault(w, 0)             # Set default if key missing
            result[w] += 1                      # Increment count
    return result                            # Return frequency dict

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: setdefault() method provides default value
```

## While Loop
```python
def count_word_frequency(sentence):
    result = {}                            # Initialize dictionary
    words = sentence.lower().split(" ")     # Split into words
    i = 0                                   # Initialize index
    while i < len(words):                   # Loop through words
        w = words[i]                         # Get current word
        if w:                                 # Skip empty strings
            if w not in result:                # Check if new word
                result[w] = 1                    # First occurrence
            else:                               # Existing word
                result[w] += 1                    # Increment count
        i += 1                                # Move to next word
    return result                            # Return frequency dict

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: Manual index control with while loop
```

## Pandas Series Value Counts
```python
import pandas as pd

def count_word_frequency(sentence):
    words = sentence.lower().split(" ")     # Split into words
    words = [w for w in words if w]          # Filter empty strings
    return pd.Series(words).value_counts().to_dict()  # Pandas value_counts then to dict

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'and': 2, 'cat': 1, 'dog': 1, 'bird': 1}
# Special: Pandas Series value_counts for statistical counting
```

## Numpy Unique with Counts
```python
import numpy as np

def count_word_frequency(sentence):
    words = sentence.lower().split(" ")     # Split into words
    words = [w for w in words if w]          # Filter empty strings
    unique, counts = np.unique(words, return_counts=True)  # Get unique values and counts
    return dict(zip(unique, counts))         # Zip together and convert to dict

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'and': 2, 'bird': 1, 'cat': 1, 'dog': 1, 'the': 3}
# Special: NumPy's unique with return_counts for array-based counting
```

## Recursive Count
```python
def count_word_frequency(sentence, result=None):
    if result is None:                     # Initialize on first call
        result = {}
    
    words = sentence.lower().split(" ", 1)  # Split into first word and rest
    first_word = words[0]                    # Get first word
    
    if first_word:                           # Skip empty strings
        result[first_word] = result.get(first_word, 0) + 1  # Count first word
    
    if len(words) > 1:                       # If there's more sentence
        return count_word_frequency(words[1], result)  # Recurse on remainder
    return result                            # Return final result

print(count_word_frequency("The cat and the dog and the bird"))  # Call with sentence - {'the': 3, 'cat': 1, 'and': 2, 'dog': 1, 'bird': 1}
# Special: Recursive approach processing one word at a time
```