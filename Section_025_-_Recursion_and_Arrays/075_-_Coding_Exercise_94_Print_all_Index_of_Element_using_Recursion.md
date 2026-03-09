## 1. Recursive First Index with Index Parameter
### Explanation
This function recursively searches for the first occurrence of an element in a list using an index parameter to avoid slicing. It starts from the given index (0 initially) and checks each element sequentially. This approach is efficient in terms of memory as it does not create new list slices.

### Approach
- Base case: if index reaches the length of the list, return -1 (element not found).
- If the element at the current index matches x, return that index.
- Otherwise, make a recursive call with the next index (index+1).

### Algorithm
1. If index == len(list): return -1.
2. If list[index] == x: return index.
3. Else return firstIndexOfAnElementBetter(list, x, index+1).

### Pseudocode
```
FUNCTION firstIndexOfAnElementBetter(list, x, index)
    IF index == len(list) THEN
        RETURN -1
    IF list[index] == x THEN
        RETURN index
    RETURN firstIndexOfAnElementBetter(list, x, index+1)
END FUNCTION
```

### Code
```python
def firstIndexOfAnElementBetter(l1, x, index):
    # Base case: reached the end without finding the element
    if len(l1) == index:
        return -1

    # Main logic: check current element
    if l1[index] == x:
        return index  # Found, return current index

    # Recursive call with next index (tail recursion style)
    return firstIndexOfAnElementBetter(l1, x, index + 1)
```

### Example Output
```python
print(firstIndexOfAnElementBetter([3, 2, 5, 2, 8, 2, 1], 2, 0))  # Output: 1
```

---

## 2. Recursive Print All Indices (Reverse Order)
### Explanation
This function prints all indices where a given element appears in a list. It uses a helper that first recurses to the end of the list and then checks elements on the way back, resulting in indices printed in reverse order (from last to first). The commented alternative in the original code shows how to print in forward order by checking before recursing.

### Approach
- Base case: if index reaches the length of the list, return.
- Recursively call the helper with index+1 (go deeper first).
- After returning, check if the current element matches x and print the index.

### Algorithm
1. If index == len(list): return.
2. Call printAllIndicesOfAnElementHelper(list, x, index+1).
3. If list[index] == x: print(index).

### Pseudocode
```
FUNCTION printAllIndicesOfAnElementHelper(list, x, index)
    IF index == len(list) THEN
        RETURN
    printAllIndicesOfAnElementHelper(list, x, index+1)
    IF list[index] == x THEN
        PRINT index
    END IF
END FUNCTION

FUNCTION printAllIndicesOfAnElement(list, x)
    printAllIndicesOfAnElementHelper(list, x, 0)
END FUNCTION
```

### Code
```python
def printAllIndicesOfAnElementHelper(l1, x, index):
    # Base case: end of list
    if len(l1) == index:
        return

    # Recursive call to go deeper first (post-order processing)
    printAllIndicesOfAnElementHelper(l1, x, index + 1)

    # After returning from deeper, check current element
    if l1[index] == x:
        print(index)  # Prints in reverse order

def printAllIndicesOfAnElement(l1, x):
    # Wrapper to start recursion
    printAllIndicesOfAnElementHelper(l1, x, 0)
```

### Example Output
```python
printAllIndicesOfAnElement([3, 2, 5, 2, 8, 2, 1], 2)
# Output:
# 5
# 3
# 1
```

---

## 3. Recursive Return All Indices (with Accumulator)
### Explanation
Instead of printing, this version recursively collects all indices where the element occurs and returns them as a list. It uses an accumulator list passed through recursive calls to gather indices in ascending order.

### Approach
- Define a helper that takes the list, element, current index, and a result list (accumulator).
- Base case: if index reaches the length, simply return (the result list is already built).
- If the current element matches, append the index to the result list.
- Recursively call with index+1 and the same result list.
- The wrapper initializes an empty list and returns it after the helper completes.

### Algorithm
1. result = []
2. Helper(index):
   - if index == len(list): return
   - if list[index] == x: result.append(index)
   - helper(index+1)
3. Return result.

### Pseudocode
```
FUNCTION helper(arr, element, index, result)
    IF index == len(arr) THEN
        RETURN
    IF arr[index] == element THEN
        result.append(index)
    helper(arr, element, index+1, result)
END FUNCTION

FUNCTION find_indices(arr, element)
    result = []
    helper(arr, element, 0, result)
    RETURN result
END FUNCTION
```

### Code
```python
def helper(arr, element, index, result):
    # Base case: end of array
    if index == len(arr):
        return

    # If current element matches, add its index to result
    if arr[index] == element:
        result.append(index)

    # Recursive call with next index (continues building result)
    helper(arr, element, index + 1, result)

def find_indices(arr, element):
    result = []  # Initialize accumulator
    helper(arr, element, 0, result)  # Start recursion
    return result  # Return the accumulated list
```

### Example Output
```python
print(find_indices([3, 2, 5, 2, 8, 2, 1], 2))  # Output: [1, 3, 5]
```

---

## 4. Iterative First Index using For Loop
### Explanation
A straightforward iterative version that scans the list from the beginning using a for loop and returns the first matching index. This avoids recursion entirely and is efficient.

### Approach
- Loop through indices from 0 to len(list)-1.
- If the element at the current index equals x, return that index.
- If the loop completes, return -1.

### Algorithm
1. For i from 0 to len(list)-1:
   - if list[i] == x: return i.
2. Return -1.

### Pseudocode
```
FUNCTION firstIndexOfAnElement_Iterative(list, x)
    FOR i = 0 TO len(list)-1 DO
        IF list[i] == x THEN
            RETURN i
        END IF
    END FOR
    RETURN -1
END FUNCTION
```

### Code
```python
def firstIndexOfAnElement_Iterative(l1, x):
    # Iterate through the list with index i
    for i in range(len(l1)):
        # Check if current element matches x
        if l1[i] == x:
            return i  # Found, return the index
    return -1  # Not found after full scan
```

### Example Output
```python
print(firstIndexOfAnElement_Iterative([3, 2, 5, 2, 8, 2, 1], 2))  # Output: 1
```

---

## 5. Iterative Print All Indices using For Loop
### Explanation
This function prints all indices where the element occurs by iterating through the list in order. It prints indices as they are encountered, resulting in ascending order.

### Approach
- Loop through indices from 0 to len(list)-1.
- If list[i] == x, print i.

### Algorithm
1. For i from 0 to len(list)-1:
   - if list[i] == x: print(i)

### Pseudocode
```
FUNCTION printAllIndices_Iterative(list, x)
    FOR i = 0 TO len(list)-1 DO
        IF list[i] == x THEN
            PRINT i
        END IF
    END FOR
END FUNCTION
```

### Code
```python
def printAllIndices_Iterative(l1, x):
    # Iterate through the list with index i
    for i in range(len(l1)):
        # If current element matches, print its index
        if l1[i] == x:
            print(i)  # Prints in ascending order
```

### Example Output
```python
printAllIndices_Iterative([3, 2, 5, 2, 8, 2, 1], 2)
# Output:
# 1
# 3
# 5
```

---

## 6. Iterative Return All Indices using List
### Explanation
This version builds a list of all matching indices iteratively and returns it. It is simple, efficient, and produces ascending order.

### Approach
- Initialize an empty list result.
- Loop through indices; if element matches, append index to result.
- Return result.

### Algorithm
1. result = []
2. For i from 0 to len(list)-1:
   - if list[i] == x: result.append(i)
3. Return result.

### Pseudocode
```
FUNCTION allIndices_Iterative(list, x)
    result = []
    FOR i = 0 TO len(list)-1 DO
        IF list[i] == x THEN
            result.append(i)
        END IF
    END FOR
    RETURN result
END FUNCTION
```

### Code
```python
def allIndices_Iterative(l1, x):
    # Initialize empty list to store indices
    result = []

    # Iterate through the list
    for i in range(len(l1)):
        # If match found, append current index
        if l1[i] == x:
            result.append(i)

    return result  # Return the list of indices
```

### Example Output
```python
print(allIndices_Iterative([3, 2, 5, 2, 8, 2, 1], 2))  # Output: [1, 3, 5]
```

---

## 7. First Index using Built-in `list.index`
### Explanation
Python's built-in `list.index(x)` returns the first index of element x. However, it raises a ValueError if x is not present. We can handle this with a try-except block to return -1 when the element is not found.

### Approach
- Use a try block to call `list.index(x)` and return the result.
- Catch ValueError and return -1.

### Algorithm
1. Try: return list.index(x)
2. Except ValueError: return -1

### Pseudocode
```
FUNCTION firstIndexOfAnElement_Builtin(list, x)
    TRY
        RETURN list.index(x)
    EXCEPT ValueError
        RETURN -1
    END TRY
END FUNCTION
```

### Code
```python
def firstIndexOfAnElement_Builtin(l1, x):
    # Attempt to use built-in index method
    try:
        return l1.index(x)  # Returns first occurrence if found
    except ValueError:
        return -1  # Element not present
```

### Example Output
```python
print(firstIndexOfAnElement_Builtin([3, 2, 5, 2, 8, 2, 1], 2))  # Output: 1
print(firstIndexOfAnElement_Builtin([3, 2, 5, 2, 8, 2, 1], 9))  # Output: -1
```

---

## 8. All Indices using List Comprehension
### Explanation
A concise and Pythonic way to generate a list of all indices where the element occurs using a list comprehension with `enumerate`. This is both readable and efficient.

### Approach
- Use `enumerate(list)` to get (index, value) pairs.
- Filter pairs where value == x and extract the index.
- The comprehension directly builds the list.

### Algorithm
1. Return [i for i, val in enumerate(list) if val == x]

### Pseudocode
```
FUNCTION allIndices_Comprehension(list, x)
    RETURN [i FOR i, val IN enumerate(list) IF val == x]
END FUNCTION
```

### Code
```python
def allIndices_Comprehension(l1, x):
    # List comprehension with enumerate: for each index and value,
    # include index if value matches x
    return [i for i, val in enumerate(l1) if val == x]
```

### Example Output
```python
print(allIndices_Comprehension([3, 2, 5, 2, 8, 2, 1], 2))  # Output: [1, 3, 5]
```

---

## 9. First Index using `next` and Generator Expression
### Explanation
A functional approach: create a generator that yields indices where the element matches, then use `next()` to retrieve the first one. A default value of -1 ensures we return -1 if no match.

### Approach
- Generator expression: (i for i, val in enumerate(list) if val == x)
- Call next() on the generator with default -1.

### Algorithm
1. gen = (i for i, val in enumerate(list) if val == x)
2. Return next(gen, -1)

### Pseudocode
```
FUNCTION firstIndexOfAnElement_Next(list, x)
    gen = (i FOR i, val IN enumerate(list) IF val == x)
    RETURN next(gen, -1)
END FUNCTION
```

### Code
```python
def firstIndexOfAnElement_Next(l1, x):
    # Generator yields indices where value matches x
    gen = (i for i, val in enumerate(l1) if val == x)
    # next returns first element of generator, or -1 if empty
    return next(gen, -1)
```

### Example Output
```python
print(firstIndexOfAnElement_Next([3, 2, 5, 2, 8, 2, 1], 2))  # Output: 1
print(firstIndexOfAnElement_Next([3, 2, 5, 2, 8, 2, 1], 9))  # Output: -1
```

---

## 10. All Indices using `filter` and `map`
### Explanation
A functional style combining `filter` and `map`. First, `enumerate` creates (index, value) pairs. `filter` selects those pairs where the value equals x. Then `map` extracts the index from each pair. Finally, we convert the result to a list.

### Approach
- Use `enumerate(list)` to get pairs.
- Filter with lambda that checks if the value (second element) equals x.
- Map the lambda to get the first element (index).
- Convert to list.

### Algorithm
1. pairs = enumerate(list)
2. filtered = filter(lambda pair: pair[1] == x, pairs)
3. indices = map(lambda pair: pair[0], filtered)
4. Return list(indices)

### Pseudocode
```
FUNCTION allIndices_FilterMap(list, x)
    pairs = enumerate(list)
    filtered = filter((pair) -> pair[1] == x, pairs)
    indices = map((pair) -> pair[0], filtered)
    RETURN list(indices)
END FUNCTION
```

### Code
```python
def allIndices_FilterMap(l1, x):
    # Get (index, value) pairs
    pairs = enumerate(l1)
    # Keep only pairs where value matches x
    filtered = filter(lambda pair: pair[1] == x, pairs)
    # Extract the index from each pair
    indices = map(lambda pair: pair[0], filtered)
    # Convert to list and return
    return list(indices)
```

### Example Output
```python
print(allIndices_FilterMap([3, 2, 5, 2, 8, 2, 1], 2))  # Output: [1, 3, 5]
```