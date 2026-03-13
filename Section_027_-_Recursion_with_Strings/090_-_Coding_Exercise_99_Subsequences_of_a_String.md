## 1. Classic Recursive Subsequence Generation (Return List with Slicing)

### Explanation
This variant recursively generates all subsequences by considering the first character and combining it with all subsequences of the remaining string. It returns a list of all subsequences.

### Approach
- Base case: empty string returns a list containing an empty string.
- Recursive case: get all subsequences of the string without the first character (`s[1:]`).
- Then, for each subsequence in that result, prepend the first character and add to the final list, along with the original subsequences (without the first character).

### Algorithm
1. If string is empty, return `[""]`.
2. Let `smallAns` = subsequences of `s[1:]`.
3. Initialize `ans` as a copy of `smallAns`.
4. For each subsequence in `smallAns`, append `s[0] + subsequence` to `ans`.
5. Return `ans`.

### Pseudocode
```
function getSubsequences(s):
    if s == "":
        return [""]
    small = getSubsequences(s[1:])
    result = small[:]  # copy
    for sub in small:
        result.append(s[0] + sub)
    return result
```

### Code
```python
def get_subsequences(s):
    # Base case: empty string -> list with empty string
    if s == "":
        return [""]
    
    # Recursive call on string without first character
    small_ans = get_subsequences(s[1:])   # e.g., for "bc": ["", "b", "c", "bc"]
    first_char = s[0]
    result = []
    
    # First, include all subsequences that don't include first character
    result.extend(small_ans)               # critical: add subsequences without first char
    
    # Then, for each subsequence without first char, prepend first char
    for sub in small_ans:
        result.append(first_char + sub)    # main logic: combine first char with each subsequence
    
    return result

# Example usage
s = "abc"
subseqs = get_subsequences(s)
print(subseqs)  # Output: ['', 'c', 'b', 'bc', 'a', 'ac', 'ab', 'abc']
```

---

## 2. Recursive Subsequence Generation with Index (Efficient)

### Explanation
This variant avoids string slicing by using an index parameter, making it more memory-efficient. It builds subsequences by deciding at each index whether to include the character.

### Approach
- Use a helper function that takes the original string, current index, and a current subsequence being built.
- At each index, we have two choices: include the character at that index or exclude it.
- When index reaches the end of the string, add the built subsequence to the result list.

### Algorithm
1. Initialize result list.
2. Define recursive function `helper(s, i, current)`:
   - If `i == len(s)`, append `current` to result and return.
   - Recursively call with `i+1` and `current` (exclude current char).
   - Recursively call with `i+1` and `current + s[i]` (include current char).
3. Call `helper(s, 0, "")`.
4. Return result.

### Pseudocode
```
function getSubsequences(s):
    result = []
    function backtrack(i, curr):
        if i == len(s):
            result.append(curr)
            return
        backtrack(i+1, curr)          # exclude
        backtrack(i+1, curr + s[i])   # include
    backtrack(0, "")
    return result
```

### Code
```python
def get_subsequences_index(s):
    result = []
    
    def backtrack(index, current):
        # Base case: reached end of string
        if index == len(s):
            result.append(current)          # add current subsequence
            return
        
        # Recursive case 1: exclude current character
        backtrack(index + 1, current)       # main logic: exclude
        
        # Recursive case 2: include current character
        backtrack(index + 1, current + s[index])  # main logic: include
    
    backtrack(0, "")
    return result

# Example usage
s = "abc"
subseqs = get_subsequences_index(s)
print(subseqs)  # Output: ['', 'c', 'b', 'bc', 'a', 'ac', 'ab', 'abc']
```

---

## 3. Print Subsequences using Recursion (With TakenSoFar)

### Explanation
This variant prints all subsequences directly without storing them. It uses a helper that carries the current subsequence built so far.

### Approach
- Base case: when the input string is empty, print the current subsequence (`takenSoFar`).
- Recursive case: for the first character, generate subsequences by including it and by excluding it, then recurse on the remaining string.

### Algorithm
1. If string is empty, print `takenSoFar` and return.
2. Let `first_char = s[0]`, `rest = s[1:]`.
3. Recursively call with `rest` and `takenSoFar + first_char` (include).
4. Recursively call with `rest` and `takenSoFar` (exclude).

### Pseudocode
```
function printSubsequences(s, taken):
    if s == "":
        print(taken)
        return
    first = s[0]
    rest = s[1:]
    printSubsequences(rest, taken + first)   # include
    printSubsequences(rest, taken)           # exclude
```

### Code
```python
def print_subsequences(s, taken=""):
    # Base case: no characters left
    if s == "":
        print(taken)          # output the built subsequence
        return
    
    first_char = s[0]
    rest = s[1:]
    
    # Recursive call including first character
    print_subsequences(rest, taken + first_char)   # include
    
    # Recursive call excluding first character
    print_subsequences(rest, taken)                 # exclude

# Example usage
print_subsequences("abc")
# Output:
# abc
# ab
# ac
# a
# bc
# b
# c
# (empty line for empty string)
```

---

## 4. Generate Subsequences using Iterative Bitmask

### Explanation
This variant uses the fact that for a string of length `n`, there are `2^n` subsequences, each corresponding to a binary mask of length `n` where bit `i` indicates whether to include character `i`.

### Approach
- Compute total number of subsequences = `2^n`.
- Iterate `mask` from 0 to `2^n - 1`.
- For each mask, build the subsequence by including characters where the corresponding bit is set.
- Add the built subsequence to a result list.

### Algorithm
1. Let `n = len(s)`, `total = 1 << n`.
2. Initialize result list.
3. For `mask` in range(total):
   - Initialize `sub = ""`.
   - For `i` in range(n):
     - If `mask >> i & 1`: `sub += s[i]`.
   - Append `sub` to result.
4. Return result.

### Pseudocode
```
function getSubsequencesBitmask(s):
    n = len(s)
    result = []
    for mask in range(1 << n):
        sub = ""
        for i in range(n):
            if mask & (1 << i):
                sub += s[i]
        result.append(sub)
    return result
```

### Code
```python
def get_subsequences_bitmask(s):
    n = len(s)
    total = 1 << n          # 2^n
    result = []
    
    for mask in range(total):
        subsequence = ""
        # Build subsequence for current mask
        for i in range(n):
            if mask & (1 << i):          # check if i-th bit is set
                subsequence += s[i]      # include character i
        result.append(subsequence)
    
    return result

# Example usage
s = "abc"
subseqs = get_subsequences_bitmask(s)
print(subseqs)  # Output: ['', 'a', 'b', 'ab', 'c', 'ac', 'bc', 'abc']
```

---

## 5. Generate Subsequences using Python's itertools.combinations

### Explanation
This variant uses the built-in `itertools.combinations` to generate combinations of all possible lengths, which are essentially subsequences (preserving order). It's concise and leverages Python's standard library.

### Approach
- For each length `r` from 0 to `n`, generate all combinations of indices of length `r`.
- For each combination, join the corresponding characters to form a subsequence.
- Collect all results.

### Algorithm
1. Import `itertools`.
2. For `r` in range(0, n+1):
   - For each combination of indices from `range(n)` with length `r`:
     - Build string by joining `s[i]` for `i` in combination.
     - Add to result.
3. Return result.

### Pseudocode
```
import itertools

function getSubsequences(s):
    result = []
    n = len(s)
    for r in range(n+1):
        for indices in combinations(range(n), r):
            sub = "".join(s[i] for i in indices)
            result.append(sub)
    return result
```

### Code
```python
import itertools

def get_subsequences_itertools(s):
    n = len(s)
    result = []
    # Iterate over all possible lengths
    for r in range(n + 1):
        # Generate all combinations of indices of length r
        for indices in itertools.combinations(range(n), r):
            # Build subsequence from indices
            subsequence = "".join(s[i] for i in indices)
            result.append(subsequence)
    return result

# Example usage
s = "abc"
subseqs = get_subsequences_itertools(s)
print(subseqs)  # Output: ['', 'a', 'b', 'c', 'ab', 'ac', 'bc', 'abc']
```

---

## 6. Generate Subsequences of a List (Instead of String)

### Explanation
This variant generalizes subsequence generation to lists. It returns a list of lists, where each inner list is a subsequence (preserving order).

### Approach
- Use recursion with index or bitmask to generate subsequences of list elements.
- Base case: when index reaches end, add current subsequence (as a copy) to result.

### Algorithm (using index recursion)
1. Define helper `backtrack(i, current)`.
2. If `i == len(lst)`: append a copy of `current` to result.
3. Else:
   - Exclude current element: `backtrack(i+1, current)`.
   - Include current element: `backtrack(i+1, current + [lst[i]])`.
4. Start with `backtrack(0, [])`.

### Pseudocode
```
function listSubsequences(lst):
    result = []
    function backtrack(i, curr):
        if i == len(lst):
            result.append(curr[:])
            return
        backtrack(i+1, curr)                # exclude
        backtrack(i+1, curr + [lst[i]])      # include
    backtrack(0, [])
    return result
```

### Code
```python
def list_subsequences(lst):
    result = []
    
    def backtrack(index, current):
        # Base case: processed all elements
        if index == len(lst):
            result.append(current[:])        # add a copy of current subsequence
            return
        
        # Option 1: exclude current element
        backtrack(index + 1, current)
        
        # Option 2: include current element
        current.append(lst[index])           # include
        backtrack(index + 1, current)
        current.pop()                         # backtrack: remove last element
    
    backtrack(0, [])
    return result

# Example usage
lst = [1, 2, 3]
subseqs = list_subsequences(lst)
print(subseqs)  # Output: [[], [3], [2], [2, 3], [1], [1, 3], [1, 2], [1, 2, 3]]
```

---

## 7. Generate Subsequences with Duplicate Handling (Unique Subsequences)

### Explanation
If the input string contains duplicate characters, the standard recursion will produce duplicate subsequences (e.g., "aba" has duplicate "a" subsequences). This variant generates only unique subsequences by using a set or sorting and skipping duplicates.

### Approach
- Use recursion with index, but at each level, skip subsequent calls if the current character is the same as the previous one (when excluding/including decisions are made). Alternatively, collect results in a set.
- To avoid duplicates without a set, we can sort the string first and then, when including a character, skip if it's the same as the previous character at the same decision level.

### Algorithm (using set)
1. Use recursion with index and build subsequences.
2. Store results in a set to eliminate duplicates.
3. Convert set to list at the end.

### Pseudocode
```
function uniqueSubsequences(s):
    result = set()
    function backtrack(i, curr):
        if i == len(s):
            result.add(curr)
            return
        backtrack(i+1, curr)          # exclude
        backtrack(i+1, curr + s[i])   # include
    backtrack(0, "")
    return list(result)
```

### Code
```python
def unique_subsequences(s):
    result_set = set()
    
    def backtrack(index, current):
        if index == len(s):
            result_set.add(current)          # add to set to ensure uniqueness
            return
        
        # Exclude current character
        backtrack(index + 1, current)
        
        # Include current character
        backtrack(index + 1, current + s[index])
    
    backtrack(0, "")
    return list(result_set)

# Example usage with duplicates
s = "aba"
subseqs = unique_subsequences(s)
print(subseqs)  # Output: ['', 'a', 'b', 'aa', 'ab', 'ba', 'aba'] (order may vary)
```

---

## 8. Generate Subsequences of Fixed Length (k-length Subsequences)

### Explanation
This variant generates only subsequences of a specified length `k`. It uses recursion with a counter to track the number of characters included so far.

### Approach
- Use recursion with index and current subsequence, and also a `count` of characters taken.
- Only add to result when `count == k` and index reaches end (or early prune when remaining characters are insufficient).
- Include/exclude decisions, but only proceed if we can still achieve length `k`.

### Algorithm
1. Define `backtrack(i, curr, taken)`.
2. If `taken == k`: add `curr` to result and return.
3. If `i == len(s)` or remaining characters < needed: return.
4. Exclude: `backtrack(i+1, curr, taken)`.
5. Include: `backtrack(i+1, curr + s[i], taken+1)`.

### Pseudocode
```
function kSubsequences(s, k):
    result = []
    function backtrack(i, curr, taken):
        if taken == k:
            result.append(curr)
            return
        if i == len(s) or (len(s)-i) < (k-taken):
            return
        backtrack(i+1, curr, taken)                # exclude
        backtrack(i+1, curr + s[i], taken+1)       # include
    backtrack(0, "", 0)
    return result
```

### Code
```python
def k_subsequences(s, k):
    result = []
    n = len(s)
    
    def backtrack(index, current, taken):
        # If we have taken exactly k characters, add to result
        if taken == k:
            result.append(current)
            return
        
        # If no more characters or not enough remaining, stop
        if index == n or (n - index) < (k - taken):
            return
        
        # Exclude current character
        backtrack(index + 1, current, taken)
        
        # Include current character
        backtrack(index + 1, current + s[index], taken + 1)
    
    backtrack(0, "", 0)
    return result

# Example usage
s = "abc"
k = 2
subseqs = k_subsequences(s, k)
print(subseqs)  # Output: ['ab', 'ac', 'bc']
```

---

## 9. Generate Subsequences using Backtracking (DFS Style)

### Explanation
This variant is a classic backtracking approach where we build subsequences incrementally and use a loop to iterate over possible next indices. It's similar to combinations but generates all subsequences in a depth-first manner.

### Approach
- Use a recursive function that takes the current starting index and the current subsequence.
- At each call, add the current subsequence to result.
- Then, for each index from `start` to `n-1`, include the character at that index and recurse with `i+1`.

### Algorithm
1. Initialize result list.
2. Define `backtrack(start, current)`:
   - Append `current` to result.
   - For `i` from `start` to `n-1`:
     - Call `backtrack(i+1, current + s[i])`.
3. Start with `backtrack(0, "")`.

### Pseudocode
```
function dfsSubsequences(s):
    result = []
    function backtrack(start, curr):
        result.append(curr)
        for i in range(start, len(s)):
            backtrack(i+1, curr + s[i])
    backtrack(0, "")
    return result
```

### Code
```python
def dfs_subsequences(s):
    result = []
    n = len(s)
    
    def backtrack(start, current):
        # Add current subsequence (including empty at top)
        result.append(current)
        
        # Explore all possible next characters
        for i in range(start, n):
            # Include character at i and move forward
            backtrack(i + 1, current + s[i])
    
    backtrack(0, "")
    return result

# Example usage
s = "abc"
subseqs = dfs_subsequences(s)
print(subseqs)  # Output: ['', 'a', 'ab', 'abc', 'ac', 'b', 'bc', 'c']
```

---

## 10. Generate Subsequences using Dynamic Programming (Iterative)

### Explanation
This variant builds subsequences iteratively: start with a list containing the empty string, then for each character, extend the list by appending the character to all existing subsequences.

### Approach
- Initialize `result = [""]`.
- For each character `ch` in the string:
  - Create new subsequences by appending `ch` to each existing subsequence in `result`.
  - Extend `result` with these new subsequences.
- This is similar to the classic recursion but done iteratively.

### Algorithm
1. `result = [""]`.
2. For each `ch` in `s`:
   - `new_subs = [sub + ch for sub in result]`.
   - `result.extend(new_subs)`.
3. Return `result`.

### Pseudocode
```
function iterativeSubsequences(s):
    result = [""]
    for ch in s:
        new = [sub + ch for sub in result]
        result.extend(new)
    return result
```

### Code
```python
def iterative_subsequences(s):
    result = [""]          # start with empty subsequence
    
    for ch in s:
        # Generate new subsequences by appending current char to all existing
        new_subs = [sub + ch for sub in result]   # main logic: combine char with all previous
        result.extend(new_subs)                    # add new subsequences to result
    
    return result

# Example usage
s = "abc"
subseqs = iterative_subsequences(s)
print(subseqs)  # Output: ['', 'a', 'b', 'ab', 'c', 'ac', 'bc', 'abc']
```