# 1. Original Recursive with Accumulator
### Explanation
This is the provided recursive implementation. It uses an optional result list parameter to accumulate numbers in reverse order (from n down to 1). The function checks if result is None to initialize a new list on the first call, then appends the current n, and recurses with n-1.

### Approach
- If result is None, initialize an empty list.
- Base case: if n == 0, return the accumulated result.
- Recursive case: append n to result and call count_down(n-1, result).

### Algorithm
1. If result is None, set result = [].
2. If n == 0, return result.
3. Append n to result.
4. Return count_down(n-1, result).

### Pseudocode
```
FUNCTION count_down(n, result=None)
    IF result IS None THEN
        result = []
    IF n == 0 THEN
        RETURN result
    result.append(n)
    RETURN count_down(n-1, result)
END FUNCTION
```

### Code
```python
def count_down(n, result=None):
    # Initialize accumulator on first call
    if result is None:
        result = []

    # Base case: when n reaches 0, return the built list
    if n == 0:
        return result

    # Main logic: append current number to result
    result.append(n)

    # Recursive call with decremented n and updated result
    return count_down(n - 1, result)
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 2. Iterative Using While Loop
### Explanation
An iterative version that builds the list by looping from n down to 1. This avoids recursion and is more memory-efficient for large n.

### Approach
- Initialize an empty list result.
- Use a while loop that runs while n > 0, appending n then decrementing n.
- Return the list.

### Algorithm
1. result = []
2. While n > 0:
   - Append n to result
   - n = n - 1
3. Return result

### Pseudocode
```
FUNCTION count_down(n)
    result = []
    WHILE n > 0 DO
        result.append(n)
        n = n - 1
    END WHILE
    RETURN result
END FUNCTION
```

### Code
```python
def count_down(n):
    # Initialize empty list
    result = []

    # Loop until n becomes 0
    while n > 0:
        # Append current n to the list
        result.append(n)
        # Decrement n
        n -= 1

    # Return the fully built list
    return result
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 3. Using List Comprehension with Reversed Range
### Explanation
Python's list comprehension can generate the list directly by iterating over a range in reverse order.

### Approach
- Use a list comprehension with `range(n, 0, -1)` to generate numbers from n down to 1.

### Algorithm
1. Return [i for i in range(n, 0, -1)]

### Pseudocode
```
FUNCTION count_down(n)
    RETURN [i FOR i IN range(n, 0, -1)]
END FUNCTION
```

### Code
```python
def count_down(n):
    # List comprehension: generate i from n down to 1
    return [i for i in range(n, 0, -1)]
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 4. Using Built-in list and Reversed Range
### Explanation
The simplest and most Pythonic way: convert a reversed range object directly to a list.

### Approach
- Create a range from n down to 1 using `range(n, 0, -1)` and convert it to a list.

### Algorithm
1. Return list(range(n, 0, -1))

### Pseudocode
```
FUNCTION count_down(n)
    RETURN list(range(n, 0, -1))
END FUNCTION
```

### Code
```python
def count_down(n):
    # Convert the descending range directly to a list
    return list(range(n, 0, -1))
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 5. Recursive Without Accumulator (Building by Concatenation)
### Explanation
This version does not use an accumulator. Instead, it builds the list by concatenating the current number with the result of the recursive call for the rest. Note: This creates new lists at each step and is inefficient for large n.

### Approach
- Base case: if n == 0, return [].
- Recursive case: return [n] + count_down(n-1).

### Algorithm
1. If n == 0, return [].
2. Return [n] + count_down(n-1).

### Pseudocode
```
FUNCTION count_down(n)
    IF n == 0 THEN
        RETURN []
    RETURN [n] + count_down(n-1)
END FUNCTION
```

### Code
```python
def count_down(n):
    # Base case: empty list when n reaches 0
    if n == 0:
        return []

    # Recursive case: prepend n to the list from n-1
    # This creates a new list by concatenation
    return [n] + count_down(n - 1)
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 6. Tail Recursive with Explicit Accumulator
### Explanation
A tail-recursive version (though Python does not optimize tail calls) where the recursive call is the last operation. The accumulator carries the partial result.

### Approach
- Define a helper that takes n and an accumulator list.
- If n == 0, return accumulator.
- Otherwise, call the helper with n-1 and accumulator + [n] (or append then pass). Using concatenation keeps the call as the last operation. For efficiency, we can append and then pass, but that would not be tail-recursive because the append modifies before the call. We'll use concatenation to keep it pure.

### Algorithm
1. Helper(n, acc):
   - if n == 0: return acc
   - else: return helper(n-1, acc + [n])
2. count_down(n) calls helper(n, [])

### Pseudocode
```
FUNCTION helper(n, acc)
    IF n == 0 THEN
        RETURN acc
    ELSE
        RETURN helper(n-1, acc + [n])
    END IF
END FUNCTION

FUNCTION count_down(n)
    RETURN helper(n, [])
END FUNCTION
```

### Code
```python
def helper(n, acc):
    # Base case: when n reaches 0, return the accumulated list
    if n == 0:
        return acc
    # Recursive call with n-1 and current n appended to accumulator
    # The call is the last operation, making it tail-recursive
    return helper(n - 1, acc + [n])

def count_down(n):
    # Start with an empty accumulator
    return helper(n, [])
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 7. Using Generator and List Conversion
### Explanation
Define a generator that yields numbers from n down to 1, then convert the generator output to a list.

### Approach
- Generator function: while n > 0, yield n and decrement n.
- count_down(n) returns list(generator).

### Algorithm
1. Generator gen(n):
   - while n > 0: yield n; n -= 1
2. count_down(n) returns list(gen(n))

### Pseudocode
```
GENERATOR generate(n)
    WHILE n > 0 DO
        YIELD n
        n = n - 1
    END WHILE
END GENERATOR

FUNCTION count_down(n)
    RETURN list(generate(n))
END FUNCTION
```

### Code
```python
def generate(n):
    # Generator that yields numbers from n down to 1
    while n > 0:
        yield n   # Yield current number
        n -= 1    # Decrement for next iteration

def count_down(n):
    # Collect all yielded values into a list
    return list(generate(n))
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 8. Using functools.reduce
### Explanation
`reduce` applies a function cumulatively to items of a sequence. We can start with an empty list and for each number in a descending range, append it to the accumulator.

### Approach
- Import reduce.
- Use a descending range as the sequence.
- The lambda function takes accumulator (list) and current value, returns accumulator with value appended.
- reduce returns the final list.

### Algorithm
1. Import reduce from functools.
2. Return reduce(lambda acc, x: acc + [x], range(n, 0, -1), [])

### Pseudocode
```
IMPORT reduce FROM functools

FUNCTION count_down(n)
    RETURN reduce((acc, x) -> acc + [x], range(n, 0, -1), [])
END FUNCTION
```

### Code
```python
from functools import reduce

def count_down(n):
    # reduce: start with [] and for each x in descending range, append x to acc
    # The lambda creates a new list each time (inefficient but demonstrates the concept)
    return reduce(lambda acc, x: acc + [x], range(n, 0, -1), [])
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 9. Recursive with Mutable Default Argument (Original but Without None Check)
### Explanation
This version uses a mutable default argument directly, but it has a pitfall: the default list persists across calls. However, if the user always calls with a single argument, it works. It's a concise variant but not recommended for production.

### Approach
- Define function count_down(n, result=[]).
- If n == 0, return result.
- Append n, then return count_down(n-1, result).

### Algorithm
1. If n == 0, return result.
2. Append n to result.
3. Return count_down(n-1, result).

### Pseudocode
```
FUNCTION count_down(n, result=[])
    IF n == 0 THEN
        RETURN result
    result.append(n)
    RETURN count_down(n-1, result)
END FUNCTION
```

### Code
```python
def count_down(n, result=[]):
    # Base case: when n reaches 0, return the accumulated list
    if n == 0:
        return result

    # Append current number to the list
    result.append(n)

    # Recursive call with decremented n and the same list
    return count_down(n - 1, result)
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```

---

# 10. Using collections.deque and While Loop
### Explanation
Using a deque for efficient left appends if needed, but here we simply append to the right and then convert to list. This is similar to the iterative version but demonstrates the use of deque.

### Approach
- Initialize a deque.
- While n > 0, append n and decrement n.
- Convert deque to list and return.

### Algorithm
1. from collections import deque
2. dq = deque()
3. while n > 0:
   - dq.append(n)
   - n -= 1
4. Return list(dq)

### Pseudocode
```
IMPORT deque FROM collections

FUNCTION count_down(n)
    dq = deque()
    WHILE n > 0 DO
        dq.append(n)
        n = n - 1
    END WHILE
    RETURN list(dq)
END FUNCTION
```

### Code
```python
from collections import deque

def count_down(n):
    # Create an empty deque
    dq = deque()

    # Loop while n is positive
    while n > 0:
        # Append current n to the right of the deque
        dq.append(n)
        # Decrement n
        n -= 1

    # Convert deque to a list and return
    return list(dq)
```

### Example Output
```python
print(count_down(5))  # Output: [5, 4, 3, 2, 1]
```