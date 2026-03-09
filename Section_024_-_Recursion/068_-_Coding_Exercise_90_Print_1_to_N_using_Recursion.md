## Original Recursive Version
### Explanation
This is the straightforward recursive implementation. It builds the list from 1 to n by first getting the list for n-1 and then appending n.

### Approach
- If n is 0, return an empty list.
- Otherwise, recursively get the list for n-1, append n to it, and return the updated list.

### Algorithm
1. Base case: if n == 0, return [].
2. Recursive case: call count_to_n(n-1) to get list for previous numbers.
3. Append n to that list.
4. Return the list.

### Pseudocode
```
FUNCTION count_to_n(n)
    IF n == 0 THEN
        RETURN []
    previous_list = count_to_n(n-1)
    previous_list.append(n)
    RETURN previous_list
END FUNCTION
```

### Code
```python
def count_to_n(n):
    # Base case: if n is 0, return an empty list
    if n == 0:
        return []

    # Recursive call to get the list for numbers 1..n-1
    previous_list = count_to_n(n - 1)

    # Main logic: append current number to the list
    previous_list.append(n)

    # Return the updated list
    return previous_list
```

### Example Output
```python
print(count_to_n(5))  # Output: [1, 2, 3, 4, 5]
```

---

## Iterative with For Loop
### Explanation
An iterative approach using a loop to build the list from 1 to n. This avoids recursion and is efficient.

### Approach
- Create an empty list.
- Loop from 1 to n, appending each number.
- Return the list.

### Algorithm
1. Initialize result = [].
2. For i from 1 to n:
   - Append i to result.
3. Return result.

### Pseudocode
```
FUNCTION count_to_n(n)
    result = []
    FOR i = 1 TO n DO
        result.append(i)
    END FOR
    RETURN result
END FUNCTION
```

### Code
```python
def count_to_n(n):
    # Initialize an empty list to store results
    result = []

    # Loop from 1 to n inclusive
    for i in range(1, n + 1):
        # Append each number to the list
        result.append(i)

    # Return the fully built list
    return result
```

### Example Output
```python
print(count_to_n(4))  # Output: [1, 2, 3, 4]
```

---

## Using List Comprehension
### Explanation
Python's list comprehension offers a concise way to generate lists by specifying an expression and an iteration.

### Approach
- Use a list comprehension with a range from 1 to n.
- The expression is simply i, which is included for each i.

### Algorithm
1. Return [i for i in range(1, n+1)]

### Pseudocode
```
FUNCTION count_to_n(n)
    RETURN [i FOR i IN range(1, n+1)]
END FUNCTION
```

### Code
```python
def count_to_n(n):
    # List comprehension: generate each i from 1 to n
    return [i for i in range(1, n + 1)]
```

### Example Output
```python
print(count_to_n(3))  # Output: [1, 2, 3]
```

---

## Using Built-in list and range
### Explanation
The simplest and most Pythonic way: convert the range object directly to a list.

### Approach
- Create a range from 1 to n and convert it to a list.

### Algorithm
1. Return list(range(1, n+1))

### Pseudocode
```
FUNCTION count_to_n(n)
    RETURN list(range(1, n+1))
END FUNCTION
```

### Code
```python
def count_to_n(n):
    # Convert the range (1..n) directly to a list
    return list(range(1, n + 1))
```

### Example Output
```python
print(count_to_n(7))  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## Recursive with Forward Building (Concatenation)
### Explanation
Instead of appending after the recursive call, this version builds the list by concatenating the current number with the result of the recursive call for the rest. This creates new lists at each step.

### Approach
- If n == 0, return [].
- Otherwise, return [n] + count_to_n(n-1). Note: This builds the list in reverse order if not careful. Actually [n] + count_to_n(n-1) will place n first, then the rest, so it would produce descending order. To get ascending, we need count_to_n(n-1) + [n] but that still uses recursion after call? Actually it's the same as original but using concatenation instead of append. So we'll do count_to_n(n-1) + [n] for ascending.

### Algorithm
1. Base case: if n == 0, return [].
2. Recursive case: get list for n-1, then concatenate [n] at the end.
3. Return the concatenated list.

### Pseudocode
```
FUNCTION count_to_n(n)
    IF n == 0 THEN
        RETURN []
    previous_list = count_to_n(n-1)
    RETURN previous_list + [n]
END FUNCTION
```

### Code
```python
def count_to_n(n):
    # Base case: empty list for n == 0
    if n == 0:
        return []

    # Recursive call to get list for numbers 1..n-1
    previous_list = count_to_n(n - 1)

    # Main logic: concatenate current number at the end (creates new list)
    result = previous_list + [n]

    # Return the new list
    return result
```

### Example Output
```python
print(count_to_n(6))  # Output: [1, 2, 3, 4, 5, 6]
```

---

## Tail Recursive with Accumulator
### Explanation
Tail recursion uses an accumulator to carry the partial result, avoiding post-recursion work. Although Python doesn't optimize tail calls, it's a useful pattern.

### Approach
- Define a helper function that takes n and an accumulator list.
- When n reaches 0, return the accumulator (which already contains numbers in order).
- Otherwise, call helper with n-1 and accumulator with n prepended (or appended depending on order). To maintain ascending order, we'll prepend in reverse (or build in reverse then reverse). Simpler: build descending and then reverse. But we'll build ascending by appending at the end? That would be inefficient with list copying. We'll use a technique: start with empty accumulator, and when n > 0, we call helper with n-1 and [n] + accumulator? That would produce descending. To get ascending, we can build descending and then reverse at the end. Or we can use a list and append but then it's not tail recursive because the append is after call. Better: we can pass an accumulator that grows at the front (so descending) and then reverse at the base. That's common.

### Algorithm
1. Helper function tail_helper(n, acc):
   - if n == 0: return acc
   - else: return tail_helper(n-1, [n] + acc)  # builds descending, e.g., for n=3: acc becomes [3], then [2,3], then [1,2,3]
2. count_to_n(n) calls tail_helper(n, []) and returns that (already ascending because we prepended? Wait [n] + acc with decreasing n yields ascending at the end: for n=3: start acc=[], call helper(2, [3]), then helper(1, [2,3]), then helper(0, [1,2,3]) -> returns [1,2,3]. So it's correct.

### Pseudocode
```
FUNCTION tail_helper(n, acc)
    IF n == 0 THEN
        RETURN acc
    ELSE
        RETURN tail_helper(n-1, [n] + acc)
    END IF
END FUNCTION

FUNCTION count_to_n(n)
    RETURN tail_helper(n, [])
END FUNCTION
```

### Code
```python
def tail_helper(n, acc):
    # Base case: when n reaches 0, return the accumulated list
    if n == 0:
        return acc
    # Recursive call with n-1 and n prepended to accumulator
    # This builds the list in correct order because we add larger numbers later? Actually for n=3:
    # call helper(3, []) -> helper(2, [3]) -> helper(1, [2,3]) -> helper(0, [1,2,3]) -> [1,2,3]
    return tail_helper(n - 1, [n] + acc)

def count_to_n(n):
    # Start with empty accumulator
    return tail_helper(n, [])
```

### Example Output
```python
print(count_to_n(8))  # Output: [1, 2, 3, 4, 5, 6, 7, 8]
```

---

## Using Generator and List Conversion
### Explanation
A generator function yields numbers one by one, and we can collect them into a list.

### Approach
- Define a generator that yields i from 1 to n.
- Convert the generator to a list.

### Algorithm
1. Generator gen():
   - for i in range(1, n+1): yield i
2. count_to_n(n) returns list(gen())

### Pseudocode
```
GENERATOR generate(n)
    FOR i = 1 TO n DO
        YIELD i
    END FOR
END GENERATOR

FUNCTION count_to_n(n)
    RETURN list(generate(n))
END FUNCTION
```

### Code
```python
def generate(n):
    # Generator: yield numbers from 1 to n
    for i in range(1, n + 1):
        yield i  # Yielding each number one at a time

def count_to_n(n):
    # Convert generator output to a list
    return list(generate(n))
```

### Example Output
```python
print(count_to_n(5))  # Output: [1, 2, 3, 4, 5]
```

---

## Using functools.reduce
### Explanation
`reduce` from functools can be used to build the list by successively appending numbers to an accumulator.

### Approach
- Start with an empty list as initial accumulator.
- For each number in range(1, n+1), apply a lambda that appends the number to the accumulator and returns it.
- reduce returns the final list.

### Algorithm
1. Import reduce.
2. Return reduce(lambda acc, x: acc + [x], range(1, n+1), [])

### Pseudocode
```
IMPORT reduce FROM functools

FUNCTION count_to_n(n)
    RETURN reduce((acc, x) -> acc + [x], range(1, n+1), [])
END FUNCTION
```

### Code
```python
from functools import reduce

def count_to_n(n):
    # reduce: start with [] and for each x in 1..n, append x to accumulator
    # The lambda takes accumulator and current value, returns new accumulator
    return reduce(lambda acc, x: acc + [x], range(1, n + 1), [])
```

### Example Output
```python
print(count_to_n(4))  # Output: [1, 2, 3, 4]
```

---

## Using map with range
### Explanation
`map` applies a function to each item of an iterable. We can use `map` with `range` to create numbers, but `range` already yields numbers; we just need to wrap it in a list.

### Approach
- Use `map` with an identity function (lambda x: x) on range(1, n+1) and convert to list.

### Algorithm
1. Return list(map(lambda x: x, range(1, n+1)))

### Pseudocode
```
FUNCTION count_to_n(n)
    RETURN list(map(x -> x, range(1, n+1)))
END FUNCTION
```

### Code
```python
def count_to_n(n):
    # map the identity function over the range, then convert to list
    # The identity lambda simply returns its argument
    return list(map(lambda x: x, range(1, n + 1)))
```

### Example Output
```python
print(count_to_n(6))  # Output: [1, 2, 3, 4, 5, 6]
```

---

## Recursive with Helper and Start Parameter
### Explanation
This version uses a helper that takes both start and end, building the list incrementally from start to end. It can be used to count from any start to n.

### Approach
- Define a helper count_from(start, n) that returns list from start to n.
- If start > n: return [].
- Else return [start] + count_from(start+1, n).
- count_to_n(n) simply calls count_from(1, n).

### Algorithm
1. Helper count_from(start, end):
   - if start > end: return []
   - else: return [start] + count_from(start+1, end)
2. count_to_n(n): return count_from(1, n)

### Pseudocode
```
FUNCTION count_from(start, end)
    IF start > end THEN
        RETURN []
    ELSE
        RETURN [start] + count_from(start+1, end)
    END IF
END FUNCTION

FUNCTION count_to_n(n)
    RETURN count_from(1, n)
END FUNCTION
```

### Code
```python
def count_from(start, end):
    # Base case: if start exceeds end, return empty list
    if start > end:
        return []
    # Recursive case: include start, then get the rest from start+1 to end
    return [start] + count_from(start + 1, end)

def count_to_n(n):
    # Start from 1 and go up to n
    return count_from(1, n)
```

### Example Output
```python
print(count_to_n(7))  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## Using While Loop with Deque
### Explanation
Using a `collections.deque` for efficient left appends if we wanted descending, but here we'll just build normally. This variant uses a while loop and a deque for potential efficiency if we needed to append left, but we'll just append right. The difference is using deque instead of list, then converting to list at the end.

### Approach
- Initialize an empty deque.
- While a counter <= n, append the counter to the deque and increment.
- Convert deque to list and return.

### Algorithm
1. from collections import deque
2. dq = deque()
3. i = 1
4. while i <= n:
   - dq.append(i)
   - i += 1
5. Return list(dq)

### Pseudocode
```
IMPORT deque FROM collections

FUNCTION count_to_n(n)
    dq = deque()
    i = 1
    WHILE i <= n DO
        dq.append(i)
        i = i + 1
    END WHILE
    RETURN list(dq)
END FUNCTION
```

### Code
```python
from collections import deque

def count_to_n(n):
    # Create an empty deque
    dq = deque()

    # Start counter at 1
    i = 1
    # Loop until counter exceeds n
    while i <= n:
        # Append current number to the deque (right side)
        dq.append(i)
        i += 1  # Increment counter

    # Convert deque to list and return
    return list(dq)
```

### Example Output
```python
print(count_to_n(5))  # Output: [1, 2, 3, 4, 5]
```