## 1. Simple Recursive First Index (Original)
### Explanation
This is the original recursive implementation. It checks the first element; if it matches, returns 0 (index relative to current slice). Otherwise, it recurses on the rest and adjusts the result by adding 1 if found.

### Approach
- Base case: empty list returns -1.
- If first element equals x, return 0.
- Recursively search in the rest (list[1:]) to get ansFromRecursion.
- If ansFromRecursion is -1, return -1; else return ansFromRecursion + 1.

### Algorithm
1. If list is empty, return -1.
2. If list[0] == x, return 0.
3. Set result = firstIndexOfAnElement(list[1:], x).
4. If result == -1, return -1; else return result + 1.

### Pseudocode
```
FUNCTION firstIndexOfAnElement(list, x)
    IF len(list) == 0 THEN
        RETURN -1
    IF list[0] == x THEN
        RETURN 0
    result = firstIndexOfAnElement(list[1:], x)
    IF result == -1 THEN
        RETURN -1
    ELSE
        RETURN result + 1
    END IF
END FUNCTION
```

### Code
```python
def firstIndexOfAnElement(l1, x):
    # Base case: empty list means element not found
    if len(l1) == 0:
        return -1

    # If first element matches, index is 0 in this slice
    if l1[0] == x:
        return 0

    # Recursive call on the rest of the list
    ansFromRecursion = firstIndexOfAnElement(l1[1:], x)

    # If not found in the rest, propagate -1
    if ansFromRecursion == -1:
        return -1
    else:
        # Found in the rest, so actual index is 1 + that index
        return ansFromRecursion + 1
```

### Example Output
```python
print(firstIndexOfAnElement([3, 5, 2, 7, 5], 5))  # Output: 1
print(firstIndexOfAnElement([3, 5, 2, 7, 5], 9))  # Output: -1
```

---

## 2. Iterative First Index Using Loop
### Explanation
An iterative version that scans the list from the beginning using a for loop. It returns the first index where the element matches.

### Approach
- Loop through the list with index i and element value.
- If value equals x, return i.
- If loop finishes, return -1.

### Algorithm
1. For i from 0 to len(list)-1:
   - if list[i] == x, return i.
2. Return -1.

### Pseudocode
```
FUNCTION firstIndexOfAnElement_Iterative(list, x)
    FOR i FROM 0 TO len(list)-1 DO
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
    # Iterate through the list with index
    for i in range(len(l1)):
        # Check if current element matches x
        if l1[i] == x:
            return i  # Return the first matching index
    # If loop completes, element not found
    return -1
```

### Example Output
```python
print(firstIndexOfAnElement_Iterative([3, 5, 2, 7, 5], 5))  # Output: 1
```

---

## 3. Using enumerate and Loop
### Explanation
Similar to iterative but uses `enumerate` to get both index and value in a more Pythonic way.

### Approach
- Loop through index, value pairs from enumerate(list).
- If value == x, return index.
- Else return -1.

### Algorithm
1. For i, val in enumerate(list):
   - if val == x, return i.
2. Return -1.

### Pseudocode
```
FUNCTION firstIndexOfAnElement_Enumerate(list, x)
    FOR i, val IN enumerate(list) DO
        IF val == x THEN
            RETURN i
        END IF
    END FOR
    RETURN -1
END FUNCTION
```

### Code
```python
def firstIndexOfAnElement_Enumerate(l1, x):
    # Use enumerate to get index and value simultaneously
    for i, val in enumerate(l1):
        if val == x:
            return i  # Return index on first match
    return -1
```

### Example Output
```python
print(firstIndexOfAnElement_Enumerate([3, 5, 2, 7, 5], 5))  # Output: 1
```

---

## 4. Using list.index with Exception Handling
### Explanation
Python lists have a built-in `index()` method that returns the first index of a value. However, it raises a ValueError if the value is not found. We can catch it.

### Approach
- Use try-except block: call list.index(x) and return the result.
- If ValueError occurs, return -1.

### Algorithm
1. Try: return list.index(x)
2. Except ValueError: return -1

### Pseudocode
```
FUNCTION firstIndexOfAnElement_Index(list, x)
    TRY
        RETURN list.index(x)
    EXCEPT ValueError
        RETURN -1
    END TRY
END FUNCTION
```

### Code
```python
def firstIndexOfAnElement_Index(l1, x):
    # Use built-in index method; catch exception if not found
    try:
        return l1.index(x)  # Returns first occurrence
    except ValueError:
        return -1  # Element not present
```

### Example Output
```python
print(firstIndexOfAnElement_Index([3, 5, 2, 7, 5], 5))  # Output: 1
print(firstIndexOfAnElement_Index([3, 5, 2, 7, 5], 9))  # Output: -1
```

---

## 5. Recursive with Index Parameter (No Slicing)
### Explanation
This version avoids creating new list slices by passing an index parameter that tracks the current position in the original list.

### Approach
- Helper function takes list, x, and current index i.
- Base: if i == len(list), return -1.
- If list[i] == x, return i.
- Else return helper(list, x, i+1).

### Algorithm
1. If i == len(list), return -1.
2. If list[i] == x, return i.
3. Else return firstAtIndex(list, x, i+1).

### Pseudocode
```
FUNCTION firstAtIndex(list, x, i=0)
    IF i == len(list) THEN
        RETURN -1
    IF list[i] == x THEN
        RETURN i
    RETURN firstAtIndex(list, x, i+1)
END FUNCTION
```

### Code
```python
def firstAtIndex(l1, x, i=0):
    # Base case: reached end without finding
    if i == len(l1):
        return -1
    # Check current element
    if l1[i] == x:
        return i
    # Recursive call with next index
    return firstAtIndex(l1, x, i + 1)

def firstIndexOfAnElement_IndexParam(l1, x):
    # Wrapper to start recursion from index 0
    return firstAtIndex(l1, x, 0)
```

### Example Output
```python
print(firstIndexOfAnElement_IndexParam([3, 5, 2, 7, 5], 5))  # Output: 1
```

---

## 6. Tail Recursive First Index with Accumulator
### Explanation
Tail recursion uses an accumulator to carry the result. Here the accumulator can be the current index or a flag. We'll use an accumulator to store the found index, but it's easier to use the index parameter approach which is already tail-recursive if the recursive call is the last operation. The previous version (with index) is tail-recursive because after checking the element, it returns the recursive call directly. So we can present that as tail-recursive.

### Approach
- Same as index-parameter version: the recursive call is the last operation.

### Algorithm
Same as above.

### Pseudocode
Same as above.

### Code
```python
def firstAtIndex_Tail(l1, x, i=0):
    # Base case: end of list, not found
    if i == len(l1):
        return -1
    # If found at current index, return i
    if l1[i] == x:
        return i
    # Tail-recursive call with incremented index
    return firstAtIndex_Tail(l1, x, i + 1)

def firstIndexOfAnElement_Tail(l1, x):
    return firstAtIndex_Tail(l1, x, 0)
```

### Example Output
```python
print(firstIndexOfAnElement_Tail([3, 5, 2, 7, 5], 5))  # Output: 1
```

---

## 7. Using next and enumerate with Generator Expression
### Explanation
This functional approach uses `next()` on a generator expression that yields indices where the value matches. It returns the first such index or -1 if none.

### Approach
- Create a generator (i for i, val in enumerate(list) if val == x).
- Use next() with a default of -1 to get the first element.

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
    # Generator yields indices where element equals x
    gen = (i for i, val in enumerate(l1) if val == x)
    # next returns first yielded index, or -1 if generator is empty
    return next(gen, -1)
```

### Example Output
```python
print(firstIndexOfAnElement_Next([3, 5, 2, 7, 5], 5))  # Output: 1
print(firstIndexOfAnElement_Next([3, 5, 2, 7, 5], 9))  # Output: -1
```

---

## 8. Using while Loop with Early Break
### Explanation
A while loop with an index variable. It checks each element and breaks when found.

### Approach
- Initialize i = 0.
- While i < len(list):
   - if list[i] == x: break and return i
   - else: i += 1
- If loop ends without break, return -1.

### Algorithm
1. i = 0
2. While i < len(list):
   - if list[i] == x: return i
   - i += 1
3. Return -1

### Pseudocode
```
FUNCTION firstIndexOfAnElement_While(list, x)
    i = 0
    WHILE i < len(list) DO
        IF list[i] == x THEN
            RETURN i
        END IF
        i = i + 1
    END WHILE
    RETURN -1
END FUNCTION
```

### Code
```python
def firstIndexOfAnElement_While(l1, x):
    i = 0
    while i < len(l1):
        if l1[i] == x:
            return i  # Found at current index
        i += 1
    return -1  # Not found after loop
```

### Example Output
```python
print(firstIndexOfAnElement_While([3, 5, 2, 7, 5], 5))  # Output: 1
```

---

## 9. Recursive with Slicing and Direct Return
### Explanation
This version is a compact form of the original where the recursive call and addition are combined, but we still need to handle -1. It's the same logic but written in a more concise way.

### Approach
- Base: if empty, return -1.
- If first matches, return 0.
- Otherwise, get result from rest; if -1 return -1 else result+1.

### Algorithm
Same as original.

### Pseudocode
Same as original.

### Code
```python
def firstIndexOfAnElement_Compact(l1, x):
    # Base case: empty list -> not found
    if not l1:
        return -1
    # If first element matches, index 0
    if l1[0] == x:
        return 0
    # Recursive search in the rest
    res = firstIndexOfAnElement_Compact(l1[1:], x)
    # If not found in rest, propagate -1; else add 1
    return -1 if res == -1 else res + 1
```

### Example Output
```python
print(firstIndexOfAnElement_Compact([3, 5, 2, 7, 5], 5))  # Output: 1
```

---

## 10. Using filter and lambda (Less Efficient)
### Explanation
A more academic approach: use `filter` to get a list of indices where the element matches, then take the first. This is not efficient because it creates a full list of all matches.

### Approach
- Use enumerate to get indices and values, filter based on value, then map to indices, then get first or -1.

### Algorithm
1. Create list of indices where value == x using list comprehension or filter.
2. If list is empty, return -1; else return first element.

### Pseudocode
```
FUNCTION firstIndexOfAnElement_Filter(list, x)
    indices = [i FOR i, val IN enumerate(list) IF val == x]
    IF indices THEN
        RETURN indices[0]
    ELSE
        RETURN -1
    END IF
END FUNCTION
```

### Code
```python
def firstIndexOfAnElement_Filter(l1, x):
    # Build list of indices where element equals x
    indices = [i for i, val in enumerate(l1) if val == x]
    # Return first index if list is non-empty, else -1
    return indices[0] if indices else -1
```

### Example Output
```python
print(firstIndexOfAnElement_Filter([3, 5, 2, 7, 5], 5))  # Output: 1
print(firstIndexOfAnElement_Filter([3, 5, 2, 7, 5], 9))  # Output: -1
```

---

## 11. Simple Recursive Last Index (Original)
### Explanation
This is the recursive implementation for the last index. It checks the rest first, then the first element, ensuring we find the last occurrence. The provided code only had the base case, so we complete it.

### Approach
- Base case: empty list returns -1.
- Recursively search in the rest (list[1:]) to get ansFromRecursion.
- If ansFromRecursion is not -1, that means we found a later occurrence, so return that (adjusted by +1? Wait careful: In last index, we need to check the rest first. If the rest returns an index (which is relative to the rest), that index + 1 is the actual index. But we also need to check the first element if it matches and the rest didn't find any? Actually for last index, we should check the rest first; if found, return that (adjusted). If not found, then check if first element matches; if yes, return 0; else return -1. This ensures we get the last occurrence because we prioritize later ones.

### Algorithm
1. If list empty, return -1.
2. Let restIndex = lastIndexOfAnElement(list[1:], x).
3. If restIndex != -1, return restIndex + 1 (because we found a later occurrence).
4. Else if list[0] == x, return 0.
5. Else return -1.

### Pseudocode
```
FUNCTION lastIndexOfAnElement(list, x)
    IF len(list) == 0 THEN
        RETURN -1
    restIndex = lastIndexOfAnElement(list[1:], x)
    IF restIndex != -1 THEN
        RETURN restIndex + 1
    ELSE IF list[0] == x THEN
        RETURN 0
    ELSE
        RETURN -1
    END IF
END FUNCTION
```

### Code
```python
def lastIndexOfAnElement(l1, x):
    # Base case: empty list -> not found
    if len(l1) == 0:
        return -1

    # Recursively search in the rest (all elements except first)
    restIndex = lastIndexOfAnElement(l1[1:], x)

    # If found in the rest, that's a later occurrence -> return adjusted index
    if restIndex != -1:
        return restIndex + 1
    # If not found in rest, check first element
    elif l1[0] == x:
        return 0
    # Element not present
    else:
        return -1
```

### Example Output
```python
print(lastIndexOfAnElement([3, 5, 2, 7, 5], 5))  # Output: 4
print(lastIndexOfAnElement([3, 5, 2, 7, 5], 9))  # Output: -1
```

---

## 12. Iterative Last Index Using Loop (Reverse Scan)
### Explanation
The most efficient way to find the last index is to scan from the end backwards.

### Approach
- Loop from the last index down to 0.
- If element matches x, return current index.
- If loop finishes, return -1.

### Algorithm
1. For i from len(list)-1 down to 0:
   - if list[i] == x, return i.
2. Return -1.

### Pseudocode
```
FUNCTION lastIndexOfAnElement_Iterative(list, x)
    FOR i FROM len(list)-1 DOWNTO 0 DO
        IF list[i] == x THEN
            RETURN i
        END IF
    END FOR
    RETURN -1
END FUNCTION
```

### Code
```python
def lastIndexOfAnElement_Iterative(l1, x):
    # Iterate from last index down to 0
    for i in range(len(l1) - 1, -1, -1):
        if l1[i] == x:
            return i  # Return the first match from the right
    return -1
```

### Example Output
```python
print(lastIndexOfAnElement_Iterative([3, 5, 2, 7, 5], 5))  # Output: 4
```

---

## 13. Using reversed and enumerate
### Explanation
Combine `reversed` and `enumerate` to scan from the end, but we need to compute the actual index.

### Approach
- Use enumerate on the reversed list; the index i (0-based from the end) corresponds to original index = len(list)-1-i.
- When we find a match, return that computed index.

### Algorithm
1. For i, val in enumerate(reversed(list)):
   - if val == x, return len(list)-1-i.
2. Return -1.

### Pseudocode
```
FUNCTION lastIndexOfAnElement_Reversed(list, x)
    FOR i, val IN enumerate(reversed(list)) DO
        IF val == x THEN
            RETURN len(list)-1-i
        END IF
    END FOR
    RETURN -1
END FUNCTION
```

### Code
```python
def lastIndexOfAnElement_Reversed(l1, x):
    # Enumerate over reversed list
    for i, val in enumerate(reversed(l1)):
        if val == x:
            # Convert reverse index to original index
            return len(l1) - 1 - i
    return -1
```

### Example Output
```python
print(lastIndexOfAnElement_Reversed([3, 5, 2, 7, 5], 5))  # Output: 4
```

---

## 14. Using list.index with reverse trick
### Explanation
We can reverse the list and use index to find the first occurrence in reversed list, then map back.

### Approach
- Reverse the list: rev = list[::-1].
- Use rev.index(x) to get the position in reversed list.
- Original index = len(list)-1 - rev_index.
- Handle ValueError with try-except.

### Algorithm
1. rev = list[::-1]
2. Try: rev_index = rev.index(x); return len(list)-1 - rev_index
3. Except ValueError: return -1

### Pseudocode
```
FUNCTION lastIndexOfAnElement_ReverseIndex(list, x)
    rev = list[::-1]
    TRY
        revIndex = rev.index(x)
        RETURN len(list)-1 - revIndex
    EXCEPT ValueError
        RETURN -1
    END TRY
END FUNCTION
```

### Code
```python
def lastIndexOfAnElement_ReverseIndex(l1, x):
    # Reverse the list
    rev = l1[::-1]
    try:
        # Find first occurrence in reversed list
        revIndex = rev.index(x)
        # Convert to original index
        return len(l1) - 1 - revIndex
    except ValueError:
        return -1
```

### Example Output
```python
print(lastIndexOfAnElement_ReverseIndex([3, 5, 2, 7, 5], 5))  # Output: 4
```

---

## 15. Recursive Last Index with Index Parameter (No Slicing)
### Explanation
This version uses an index parameter to traverse the list without slicing. It finds the last occurrence by recursing to the end first, then checking on the way back.

### Approach
- Helper function takes list, x, and current index i.
- If i == len(list), return -1.
- Recursively call for i+1 to get resultFromRest.
- If resultFromRest != -1, return it (since it's later).
- Else if list[i] == x, return i.
- Else return -1.

### Algorithm
1. If i == len: return -1
2. restResult = helper(list, x, i+1)
3. If restResult != -1: return restResult
4. Else if list[i] == x: return i
5. Else return -1

### Pseudocode
```
FUNCTION lastAtIndex(list, x, i)
    IF i == len(list) THEN
        RETURN -1
    restResult = lastAtIndex(list, x, i+1)
    IF restResult != -1 THEN
        RETURN restResult
    ELSE IF list[i] == x THEN
        RETURN i
    ELSE
        RETURN -1
    END IF
END FUNCTION
```

### Code
```python
def lastAtIndex(l1, x, i):
    # Base case: reached end, not found
    if i == len(l1):
        return -1
    # Recursively check the rest first
    restResult = lastAtIndex(l1, x, i + 1)
    # If found in the rest, return that (it's later)
    if restResult != -1:
        return restResult
    # Otherwise, check current element
    elif l1[i] == x:
        return i
    else:
        return -1

def lastIndexOfAnElement_IndexParam(l1, x):
    return lastAtIndex(l1, x, 0)
```

### Example Output
```python
print(lastIndexOfAnElement_IndexParam([3, 5, 2, 7, 5], 5))  # Output: 4
```

---

## 16. Tail Recursive Last Index with Accumulator (Forward Scan)
### Explanation
For last index, tail recursion can be achieved by scanning forward and carrying the last found index. We can use an accumulator that stores the index of the last occurrence seen so far.

### Approach
- Helper function takes list, x, current index i, and lastFound accumulator (initial -1).
- If i == len: return lastFound.
- If list[i] == x: update lastFound to i.
- Tail-recursive call with i+1 and updated lastFound.

### Algorithm
1. If i == len: return lastFound
2. If list[i] == x: lastFound = i
3. Return helper(list, x, i+1, lastFound)

### Pseudocode
```
FUNCTION lastAtIndexTail(list, x, i, lastFound)
    IF i == len(list) THEN
        RETURN lastFound
    IF list[i] == x THEN
        lastFound = i
    RETURN lastAtIndexTail(list, x, i+1, lastFound)
END FUNCTION
```

### Code
```python
def lastAtIndexTail(l1, x, i, lastFound):
    # Base case: end of list, return the last found index (or -1)
    if i == len(l1):
        return lastFound
    # Update lastFound if current element matches
    if l1[i] == x:
        lastFound = i
    # Tail-recursive call with next index and possibly updated lastFound
    return lastAtIndexTail(l1, x, i + 1, lastFound)

def lastIndexOfAnElement_Tail(l1, x):
    return lastAtIndexTail(l1, x, 0, -1)
```

### Example Output
```python
print(lastIndexOfAnElement_Tail([3, 5, 2, 7, 5], 5))  # Output: 4
```

---

## 17. Using while Loop (Reverse Scan)
### Explanation
A while loop that starts from the end and decrements.

### Approach
- i = len(list)-1
- While i >= 0:
   - if list[i] == x: return i
   - i -= 1
- Return -1

### Algorithm
Same as iterative but with while.

### Pseudocode
```
FUNCTION lastIndexOfAnElement_While(list, x)
    i = len(list)-1
    WHILE i >= 0 DO
        IF list[i] == x THEN
            RETURN i
        END IF
        i = i - 1
    END WHILE
    RETURN -1
END FUNCTION
```

### Code
```python
def lastIndexOfAnElement_While(l1, x):
    i = len(l1) - 1
    while i >= 0:
        if l1[i] == x:
            return i
        i -= 1
    return -1
```

### Example Output
```python
print(lastIndexOfAnElement_While([3, 5, 2, 7, 5], 5))  # Output: 4
```

---

## 18. Using next on reversed enumerate with condition
### Explanation
Similar to the first-index next approach, but we reverse the list and adjust indices.

### Approach
- Create a generator that yields (i, val) for the reversed list? Actually we need original indices. We can use `enumerate(reversed(list))` and compute original index.
- Then use `next` on a generator that filters matches.

### Algorithm
1. gen = (len(list)-1-i for i, val in enumerate(reversed(list)) if val == x)
2. Return next(gen, -1)

### Pseudocode
```
FUNCTION lastIndexOfAnElement_Next(list, x)
    gen = (len(list)-1-i FOR i, val IN enumerate(reversed(list)) IF val == x)
    RETURN next(gen, -1)
END FUNCTION
```

### Code
```python
def lastIndexOfAnElement_Next(l1, x):
    # Generator yields original indices for matches in reverse order
    gen = (len(l1) - 1 - i for i, val in enumerate(reversed(l1)) if val == x)
    # First yielded index is the last occurrence (since we go from end)
    return next(gen, -1)
```

### Example Output
```python
print(lastIndexOfAnElement_Next([3, 5, 2, 7, 5], 5))  # Output: 4
```

---

## 19. Using filter and lambda to get all indices then last
### Explanation
Collect all indices where element matches, then return the last one.

### Approach
- Use list comprehension to get all indices.
- If list not empty, return indices[-1]; else -1.

### Algorithm
1. indices = [i for i, val in enumerate(list) if val == x]
2. Return indices[-1] if indices else -1

### Pseudocode
```
FUNCTION lastIndexOfAnElement_Filter(list, x)
    indices = [i FOR i, val IN enumerate(list) IF val == x]
    IF indices THEN
        RETURN indices[-1]
    ELSE
        RETURN -1
    END IF
END FUNCTION
```

### Code
```python
def lastIndexOfAnElement_Filter(l1, x):
    # Collect all indices where x occurs
    indices = [i for i, val in enumerate(l1) if val == x]
    # Return last index if any, else -1
    return indices[-1] if indices else -1
```

### Example Output
```python
print(lastIndexOfAnElement_Filter([3, 5, 2, 7, 5], 5))  # Output: 4
```

---

## 20. Using recursion that returns from deeper calls (alternative order)
### Explanation
A different recursive approach: check the first element, if it matches, we still need to check later ones. We can return the max of (0 if found here) and (1 + index from rest) but only if found. This is less intuitive but works.

### Approach
- Base: empty -> -1.
- Let restIndex = lastIndexOfAnElement_Alt(l1[1:], x).
- If restIndex != -1, return restIndex + 1.
- Else if l1[0] == x, return 0.
- Else return -1.
(Actually same as original)

But we can also do: return max( (0 if l1[0]==x else -1), (1+restIndex if restIndex!=-1 else -1) ) but careful with -1.

### Algorithm
Same as original.

### Pseudocode
Same.

### Code
```python
def lastIndexOfAnElement_Alt(l1, x):
    if not l1:
        return -1
    restIndex = lastIndexOfAnElement_Alt(l1[1:], x)
    if restIndex != -1:
        return restIndex + 1
    elif l1[0] == x:
        return 0
    else:
        return -1
```

### Example Output
```python
print(lastIndexOfAnElement_Alt([3, 5, 2, 7, 5], 5))  # Output: 4
```