## Palindrome Check: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
A palindrome is a sequence that reads the same forwards and backwards. For example, `[1,2,3,2,1]` is a palindrome, while `[1,2,3]` is not. Checking for a palindrome involves comparing the sequence with its reverse.

**Approach:**  
- The simplest way is to reverse the sequence and compare it with the original.  
- More efficient approaches compare elements from both ends moving inward, stopping when a mismatch is found.  
- For strings, additional considerations like case sensitivity or ignoring non-alphanumeric characters may be needed.

**Algorithm (Two-Pointer):**  
1. Set `left = 0`, `right = length(sequence) - 1`.  
2. While `left < right`:  
   - If `sequence[left] != sequence[right]`, return `False`.  
   - Increment `left`, decrement `right`.  
3. Return `True`.

**Pseudocode:**  
```
function isPalindrome(seq):
    left = 0
    right = len(seq) - 1
    while left < right:
        if seq[left] != seq[right]:
            return False
        left = left + 1
        right = right - 1
    return True
```

---

## Original Slicing Method (List)

```python
def is_palindrome_slice(lst):
    # Main logic: compare list with its reverse using slicing
    return lst == lst[::-1]          # returns True if equal, False otherwise

# Example call
test_list = [1, 2, 3, 2, 1]
result = is_palindrome_slice(test_list)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## Two-Pointer Iterative Method (List)

```python
def is_palindrome_two_pointer(lst):
    left = 0
    right = len(lst) - 1
    # Main logic: compare elements from both ends
    while left < right:
        if lst[left] != lst[right]:
            return False               # mismatch found
        left += 1
        right -= 1
    return True                        # all matched

# Example call
test_list = [1, 2, 3, 2, 1]
result = is_palindrome_two_pointer(test_list)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## Recursive Palindrome Check (List)

```python
def is_palindrome_recursive(lst):
    # Base cases: empty list or single element are palindromes
    if len(lst) <= 1:
        return True
    # Main logic: check first and last, then recurse on inner sublist
    if lst[0] != lst[-1]:
        return False
    return is_palindrome_recursive(lst[1:-1])   # recursive call on middle

# Example call
test_list = [1, 2, 3, 2, 1]
result = is_palindrome_recursive(test_list)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## String Palindrome (Case-Sensitive)

```python
def is_palindrome_string(s):
    # Main logic: compare string with its reverse using slicing
    return s == s[::-1]                # works for strings too

# Example call
test_str = "racecar"
result = is_palindrome_string(test_str)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## String Palindrome (Case-Insensitive)

```python
def is_palindrome_case_insensitive(s):
    # Convert to lowercase for case-insensitive comparison
    s_lower = s.lower()
    # Main logic: compare lowercased string with its reverse
    return s_lower == s_lower[::-1]

# Example call
test_str = "RaceCar"
result = is_palindrome_case_insensitive(test_str)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## Palindrome Ignoring Non-Alphanumeric Characters

```python
def is_palindrome_alnum(s):
    # Filter to keep only alphanumeric characters and convert to lowercase
    filtered = ''.join(ch.lower() for ch in s if ch.isalnum())
    # Main logic: compare filtered string with its reverse
    return filtered == filtered[::-1]

# Example call
test_str = "A man, a plan, a canal: Panama"
result = is_palindrome_alnum(test_str)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## Using `reversed()` and Comparison

```python
def is_palindrome_reversed(lst):
    # reversed() returns an iterator; compare list with list(reversed)
    return lst == list(reversed(lst))   # convert iterator to list for comparison

# Example call
test_list = [1, 2, 3, 2, 1]
result = is_palindrome_reversed(test_list)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## Using `collections.deque` for Two-Ended Comparison

```python
from collections import deque

def is_palindrome_deque(lst):
    dq = deque(lst)
    # Main logic: pop from both ends and compare
    while len(dq) > 1:
        if dq.popleft() != dq.pop():    # popleft from left, pop from right
            return False
    return True

# Example call
test_list = [1, 2, 3, 2, 1]
result = is_palindrome_deque(test_list)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## Number Palindrome (Convert to String)

```python
def is_palindrome_number(n):
    # Convert number to string and check palindrome
    s = str(n)
    return s == s[::-1]                # same string comparison

# Example call
test_num = 12321
result = is_palindrome_number(test_num)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```

---

## Using Stack (List as Stack)

```python
def is_palindrome_stack(lst):
    stack = []
    # Push first half onto stack
    n = len(lst)
    for i in range(n // 2):
        stack.append(lst[i])
    # Determine starting index for second half
    start = n // 2 if n % 2 == 0 else n // 2 + 1
    # Pop and compare with second half
    for i in range(start, n):
        if stack.pop() != lst[i]:
            return False
    return True

# Example call
test_list = [1, 2, 3, 2, 1]
result = is_palindrome_stack(test_list)
print(f"Is palindrome: {result}")    # Output: Is palindrome: True
```