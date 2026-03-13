## 1. Classic Recursive Insertion (Return List)

### Explanation
This variant generates all permutations by inserting the first character into every possible position of each permutation of the remaining string. It returns a list of all permutations.

### Approach
- Base case: empty string returns a list containing an empty string.
- Recursive case: get all permutations of the string without the first character.
- For each such permutation, insert the first character at every possible position (0 to len(perm)) and add to result.

### Algorithm
1. If `s == ""`, return `[""]`.
2. Let `first = s[0]`, `rest = s[1:]`.
3. `perms_of_rest = permutations(rest)`.
4. Initialize empty result list.
5. For each `perm` in `perms_of_rest`:
   - For `i` from 0 to `len(perm)`:
     - Create new string = `perm[:i] + first + perm[i:]`.
     - Append to result.
6. Return result.

### Pseudocode
```
function permutations(s):
    if s == "":
        return [""]
    first = s[0]
    rest = s[1:]
    perms_rest = permutations(rest)
    result = []
    for perm in perms_rest:
        for i in range(len(perm) + 1):
            result.append(perm[:i] + first + perm[i:])
    return result
```

### Code
```python
def permutations_return(s):
    # Base case: empty string has one permutation: empty string
    if s == "":
        return [""]
    
    first = s[0]
    rest = s[1:]
    # Recursively get permutations of the rest
    perms_of_rest = permutations_return(rest)   # e.g., for "bc": ["bc", "cb"]
    
    result = []
    # For each permutation of rest, insert first char at every position
    for perm in perms_of_rest:
        for i in range(len(perm) + 1):          # positions 0..len(perm)
            # Insert first character at position i
            new_perm = perm[:i] + first + perm[i:]
            result.append(new_perm)
    return result

# Example usage
ans = permutations_return('abc')
print(ans)
# Output: ['abc', 'bac', 'bca', 'acb', 'cab', 'cba']
```

---

## 2. Classic Recursive Insertion (Print)

### Explanation
This variant prints all permutations instead of returning them. It uses a helper that carries the current permutation built so far and inserts the next character into all positions.

### Approach
- Base case: when the input string is empty, print the accumulated permutation.
- Recursive case: take the first character, and for each position in the current permutation, insert it and recurse on the remaining string.

### Algorithm
1. If `s == ""`, print `taken` and return.
2. Let `first = s[0]`, `rest = s[1:]`.
3. For `i` from 0 to `len(taken)`:
   - Create new string = `taken[:i] + first + taken[i:]`.
   - Recursively call with `rest` and `new_taken`.

### Pseudocode
```
function printPermutations(s, taken):
    if s == "":
        print(taken)
        return
    first = s[0]
    rest = s[1:]
    for i in range(len(taken) + 1):
        new_taken = taken[:i] + first + taken[i:]
        printPermutations(rest, new_taken)
```

### Code
```python
def print_permutations(s, taken=""):
    # Base case: no more characters to process
    if s == "":
        print(taken)          # output the complete permutation
        return
    
    first = s[0]
    rest = s[1:]
    
    # Insert first character at every possible position in current taken string
    for i in range(len(taken) + 1):
        # Build new taken by inserting first at position i
        new_taken = taken[:i] + first + taken[i:]   # main logic: insert
        print_permutations(rest, new_taken)

# Example usage
print_permutations('abc')
# Output:
# abc
# bac
# bca
# acb
# cab
# cba
```

---

## 3. Backtracking with Swapping (In-Place)

### Explanation
This variant generates permutations by swapping characters in the original string (or list) using backtracking. It's efficient and avoids creating many intermediate strings.

### Approach
- Convert string to list for mutability.
- Use recursion with an index `i` representing the current position to fix.
- At each level, swap the current index with every index from `i` to end, then recurse on the next index, then swap back.

### Algorithm
1. Convert `s` to list `chars`.
2. Define `backtrack(start)`:
   - If `start == len(chars) - 1`, add current list as string to result.
   - For `j` from `start` to `len(chars)-1`:
     - Swap `chars[start]` and `chars[j]`.
     - Recurse `backtrack(start+1)`.
     - Swap back.
3. Return result list.

### Pseudocode
```
function permutations(s):
    chars = list(s)
    result = []
    function backtrack(start):
        if start == len(chars) - 1:
            result.append("".join(chars))
            return
        for j in range(start, len(chars)):
            swap(chars[start], chars[j])
            backtrack(start+1)
            swap(chars[start], chars[j])
    backtrack(0)
    return result
```

### Code
```python
def permutations_swap(s):
    chars = list(s)
    result = []
    
    def backtrack(start):
        # If we've fixed all positions, add current permutation
        if start == len(chars) - 1:
            result.append("".join(chars))   # add copy
            return
        
        for j in range(start, len(chars)):
            # Swap current index with j
            chars[start], chars[j] = chars[j], chars[start]   # main logic: swap to include different chars at start
            # Recurse on next index
            backtrack(start + 1)
            # Backtrack: swap back
            chars[start], chars[j] = chars[j], chars[start]   # restore original order
    
    backtrack(0)
    return result

# Example usage
print(permutations_swap('abc'))  # Output: ['abc', 'acb', 'bac', 'bca', 'cab', 'cba']
```

---

## 4. Backtracking with Index and Swapping (Print)

### Explanation
This variant prints permutations using the swapping backtracking method, similar to variant 3 but printing directly.

### Approach
- Convert string to list.
- Use recursion with `start` index.
- When `start` reaches the last index, print the current list as string.
- Otherwise, swap each element from `start` to end, recurse, then swap back.

### Algorithm
1. Convert `s` to list `chars`.
2. Define `backtrack(start)`:
   - If `start == len(chars) - 1`, print `"".join(chars)`.
   - For `j` from `start` to `len(chars)-1`:
     - Swap, recurse, swap back.

### Pseudocode
```
function printPermutations(s):
    chars = list(s)
    function backtrack(start):
        if start == len(chars) - 1:
            print("".join(chars))
            return
        for j in range(start, len(chars)):
            swap(chars[start], chars[j])
            backtrack(start+1)
            swap(chars[start], chars[j])
    backtrack(0)
```

### Code
```python
def print_permutations_swap(s):
    chars = list(s)
    
    def backtrack(start):
        if start == len(chars) - 1:
            print("".join(chars))      # print the permutation
            return
        
        for j in range(start, len(chars)):
            # Swap to bring different character to position start
            chars[start], chars[j] = chars[j], chars[start]   # main logic: try each candidate at start
            backtrack(start + 1)
            # Undo swap for next iteration
            chars[start], chars[j] = chars[j], chars[start]   # backtrack step
    
    backtrack(0)

# Example usage
print_permutations_swap('abc')
# Output:
# abc
# acb
# bac
# bca
# cab
# cba
```

---

## 5. Permutations with Duplicate Handling (Unique Permutations)

### Explanation
When the input string contains duplicate characters, the above methods will produce duplicate permutations. This variant generates only unique permutations by skipping duplicate swaps using a set at each recursion level.

### Approach
- Sort the string or use a set to track characters already used at the current level.
- At each recursion level, before swapping, check if the character has been used before in that level; if so, skip.
- This ensures that we don't generate duplicate permutations.

### Algorithm
1. Convert `s` to list and sort it (optional but helps).
2. Define `backtrack(start)`:
   - If `start == len(chars)-1`, add permutation.
   - Use a set `seen` to track characters already placed at position `start`.
   - For `j` from `start` to `len(chars)-1`:
     - If `chars[j]` in `seen`, skip.
     - Else add to `seen`, swap, recurse, swap back.

### Pseudocode
```
function uniquePermutations(s):
    chars = sorted(list(s))  # or just list
    result = []
    function backtrack(start):
        if start == len(chars)-1:
            result.append("".join(chars))
            return
        seen = set()
        for j in range(start, len(chars)):
            if chars[j] in seen:
                continue
            seen.add(chars[j])
            swap(chars[start], chars[j])
            backtrack(start+1)
            swap(chars[start], chars[j])
    backtrack(0)
    return result
```

### Code
```python
def unique_permutations(s):
    chars = list(s)
    # Optional: sort to bring duplicates together, but not necessary for set approach
    result = []
    
    def backtrack(start):
        if start == len(chars) - 1:
            result.append("".join(chars))
            return
        
        seen = set()   # track characters used at this level
        for j in range(start, len(chars)):
            if chars[j] in seen:
                continue          # skip duplicate
            seen.add(chars[j])
            chars[start], chars[j] = chars[j], chars[start]
            backtrack(start + 1)
            chars[start], chars[j] = chars[j], chars[start]   # backtrack
    
    backtrack(0)
    return result

# Example usage with duplicates
print(unique_permutations('aab'))  # Output: ['aab', 'aba', 'baa']
```

---

## 6. Using Python's itertools.permutations

### Explanation
Python's standard library provides a built-in function `itertools.permutations` that generates all permutations of an iterable. This variant uses it to produce permutations of the string.

### Approach
- Import `itertools`.
- Use `itertools.permutations(s)` which returns tuples of characters.
- Join each tuple into a string.
- Collect in a list.

### Algorithm
1. Import itertools.
2. For each tuple `t` in `itertools.permutations(s)`:
   - Convert to string using `"".join(t)`.
   - Append to result.
3. Return result.

### Pseudocode
```
import itertools
function permutations_itertools(s):
    return ["".join(p) for p in itertools.permutations(s)]
```

### Code
```python
import itertools

def permutations_itertools(s):
    # itertools.permutations returns tuples of characters
    perms_tuples = itertools.permutations(s)
    # Convert each tuple to a string
    result = ["".join(t) for t in perms_tuples]
    return result

# Example usage
print(permutations_itertools('abc'))  # Output: ['abc', 'acb', 'bac', 'bca', 'cab', 'cba']
```

---

## 7. Iterative Approach (Heap's Algorithm)

### Explanation
Heap's algorithm generates all permutations by generating each permutation from the previous one using swaps. It's an efficient iterative method.

### Approach
- Use an array `c` to keep track of swap indices.
- Start with the original list.
- Iterate through indices and perform swaps based on the state array.

### Algorithm (Heap's algorithm)
1. Convert string to list `a`.
2. Initialize result list with current list as string.
3. Create an array `c` of length n, all zeros.
4. Set `i = 0`.
5. While `i < n`:
   - If `c[i] < i`:
     - If `i` is even, swap `a[0]` and `a[i]`; else swap `a[c[i]]` and `a[i]`.
     - Append new permutation to result.
     - Increment `c[i]`.
     - Set `i = 0`.
   - Else:
     - Set `c[i] = 0`, `i += 1`.

### Pseudocode
```
function heapPermutations(s):
    a = list(s)
    result = ["".join(a)]
    c = [0] * len(a)
    i = 0
    while i < len(a):
        if c[i] < i:
            if i % 2 == 0:
                swap(a[0], a[i])
            else:
                swap(a[c[i]], a[i])
            result.append("".join(a))
            c[i] += 1
            i = 0
        else:
            c[i] = 0
            i += 1
    return result
```

### Code
```python
def permutations_heap(s):
    a = list(s)
    n = len(a)
    result = ["".join(a)]          # include original
    c = [0] * n                     # control array
    i = 0
    while i < n:
        if c[i] < i:
            # Determine swap based on parity of i
            if i % 2 == 0:
                a[0], a[i] = a[i], a[0]      # swap first and i
            else:
                a[c[i]], a[i] = a[i], a[c[i]]  # swap a[c[i]] and a[i]
            result.append("".join(a))        # add new permutation
            c[i] += 1
            i = 0                             # reset i
        else:
            c[i] = 0
            i += 1
    return result

# Example usage
print(permutations_heap('abc'))  # Output: ['abc', 'bac', 'cab', 'acb', 'bca', 'cba']
```

---

## 8. Permutations using BFS / Queue (Level Order)

### Explanation
This variant generates permutations iteratively using a queue. Starting with an empty string, we insert each character of the original string into every position of the current strings in the queue.

### Approach
- Initialize a queue with an empty string.
- For each character in the original string:
   - For each string currently in the queue, generate new strings by inserting the character at every position.
   - Replace the queue with these new strings.
- After processing all characters, the queue contains all permutations.

### Algorithm
1. `queue = [""]`
2. For each `ch` in `s`:
   - Create new empty list `new_queue`.
   - For each `curr` in `queue`:
     - For `i` from 0 to `len(curr)`:
       - `new_queue.append(curr[:i] + ch + curr[i:])`
   - `queue = new_queue`
3. Return `queue`.

### Pseudocode
```
function permutations_bfs(s):
    queue = [""]
    for ch in s:
        new_queue = []
        for curr in queue:
            for i in range(len(curr) + 1):
                new_queue.append(curr[:i] + ch + curr[i:])
        queue = new_queue
    return queue
```

### Code
```python
def permutations_bfs(s):
    # Start with a queue containing the empty string
    queue = [""]
    
    # Process each character of the input string
    for ch in s:
        new_queue = []
        # For each partial permutation in the queue
        for curr in queue:
            # Insert current character at every possible position
            for i in range(len(curr) + 1):
                new_perm = curr[:i] + ch + curr[i:]   # main logic: insert ch at position i
                new_queue.append(new_perm)
        queue = new_queue   # replace queue with new permutations
    
    return queue

# Example usage
print(permutations_bfs('abc'))  # Output: ['cba', 'bca', 'bac', 'cab', 'acb', 'abc'] (order may vary)
```

---

## 9. Permutations of a List (instead of string)

### Explanation
This variant generates all permutations of a list of elements (could be any type). It uses the swapping backtracking method but works on a list and returns a list of lists.

### Approach
- Use recursion with index swapping, similar to variant 3, but operate on a list directly.
- Base case: when index reaches the end, add a copy of the list to result.
- Recurse by swapping each element with the current index.

### Algorithm
1. Let `arr` be the input list.
2. Define `backtrack(start)`:
   - If `start == len(arr)`, append a copy of `arr` to result.
   - For `j` from `start` to `len(arr)-1`:
     - Swap `arr[start]` and `arr[j]`.
     - Recurse `backtrack(start+1)`.
     - Swap back.
3. Return result.

### Pseudocode
```
function listPermutations(arr):
    result = []
    function backtrack(start):
        if start == len(arr):
            result.append(arr[:])
            return
        for j in range(start, len(arr)):
            swap(arr[start], arr[j])
            backtrack(start+1)
            swap(arr[start], arr[j])
    backtrack(0)
    return result
```

### Code
```python
def list_permutations(arr):
    result = []
    
    def backtrack(start):
        if start == len(arr):
            result.append(arr[:])      # add a copy of current list
            return
        
        for j in range(start, len(arr)):
            # Swap to put arr[j] at position start
            arr[start], arr[j] = arr[j], arr[start]   # main logic: try each element at start
            backtrack(start + 1)
            # Backtrack: undo swap
            arr[start], arr[j] = arr[j], arr[start]
    
    backtrack(0)
    return result

# Example usage
lst = [1, 2, 3]
perms = list_permutations(lst)
print(perms)  # Output: [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,2,1], [3,1,2]]
```

---

## 10. Permutations of Fixed Length (k-permutations)

### Explanation
This variant generates all permutations of a given length `k` from the string (i.e., all ordered selections of `k` characters without repetition). It uses backtracking to build permutations of length `k`.

### Approach
- Use a boolean array `used` to track which characters are already taken.
- Recursively build a list of length `k` by picking unused characters.
- When the built list has length `k`, add it as a string to result.

### Algorithm
1. Let `n = len(s)`. If `k > n`, return empty list.
2. Initialize `used = [False] * n`.
3. Define `backtrack(current)`:
   - If `len(current) == k`, append `"".join(current)` to result.
   - Else:
     - For `i` from 0 to `n-1`:
       - If not `used[i]`:
         - Mark `used[i] = True`, append `s[i]` to current, recurse, then pop and unmark.
4. Start with `backtrack([])`.

### Pseudocode
```
function k_permutations(s, k):
    n = len(s)
    if k > n: return []
    result = []
    used = [False] * n
    function backtrack(curr):
        if len(curr) == k:
            result.append("".join(curr))
            return
        for i in range(n):
            if not used[i]:
                used[i] = True
                curr.append(s[i])
                backtrack(curr)
                curr.pop()
                used[i] = False
    backtrack([])
    return result
```

### Code
```python
def k_permutations(s, k):
    n = len(s)
    if k > n:
        return []   # not enough characters
    
    result = []
    used = [False] * n
    
    def backtrack(current):
        # If we've selected k characters, add to result
        if len(current) == k:
            result.append("".join(current))
            return
        
        for i in range(n):
            if not used[i]:
                # Choose character at index i
                used[i] = True
                current.append(s[i])                # include
                backtrack(current)
                # Backtrack
                current.pop()                       # undo
                used[i] = False
    
    backtrack([])
    return result

# Example usage
s = "abc"
k = 2
perms = k_permutations(s, k)
print(perms)  # Output: ['ab', 'ac', 'ba', 'bc', 'ca', 'cb']
```