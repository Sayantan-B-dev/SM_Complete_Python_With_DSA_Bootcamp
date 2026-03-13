## 1. Classic Two-Pointer Recursive Palindrome Check

### Explanation
This variant uses two pointers, one starting from the beginning and one from the end, moving toward the center. It compares characters at each step and recurses until the pointers cross or a mismatch is found.

### Approach
- Base case: if start index is greater than or equal to end index, the substring is a palindrome (empty or single character).
- If characters at start and end don't match, return False.
- Otherwise, recursively check the substring from start+1 to end-1.

### Algorithm
1. If `start >= end`, return True.
2. If `s[start] != s[end]`, return False.
3. Return `palindrome(s, start+1, end-1)`.

### Pseudocode
```
function isPalindrome(s, start, end):
    if start >= end:
        return True
    if s[start] != s[end]:
        return False
    return isPalindrome(s, start+1, end-1)
```

### Code
```python
def is_palindrome(s, start, end):
    # Base case: if pointers cross or meet, it's a palindrome
    if start >= end:
        return True
    
    # If characters at current positions differ, not a palindrome
    if s[start] != s[end]:
        return False
    
    # Recursive call: move both pointers inward
    return is_palindrome(s, start + 1, end - 1)

def check_palindrome(s):
    # Wrapper function to start recursion
    return is_palindrome(s, 0, len(s) - 1)

# Example usage
print(check_palindrome('nitin'))   # Output: True
print(check_palindrome('nitinr'))  # Output: False
```

---

## 2. Single-Pointer Recursive Palindrome Check

### Explanation
Instead of two pointers, we can use a single index `i` and compute the symmetric index from the end. This reduces the number of parameters.

### Approach
- The function takes the string and an index `i` (starting at 0).
- Compute the mirrored index `j = len(s) - 1 - i`.
- Base case: if `i >= j`, return True.
- If `s[i] != s[j]`, return False.
- Recursively call with `i+1`.

### Algorithm
1. If `i >= len(s) - 1 - i`, return True.
2. If `s[i] != s[len(s)-1-i]`, return False.
3. Return `palindrome(s, i+1)`.

### Pseudocode
```
function isPalindrome(s, i):
    j = len(s) - 1 - i
    if i >= j:
        return True
    if s[i] != s[j]:
        return False
    return isPalindrome(s, i+1)
```

### Code
```python
def is_palindrome_single_pointer(s, i=0):
    # Compute the symmetric index from the end
    j = len(s) - 1 - i
    
    # Base case: pointers have met or crossed
    if i >= j:
        return True
    
    # Compare characters at symmetric positions
    if s[i] != s[j]:
        return False
    
    # Recursive call with next index
    return is_palindrome_single_pointer(s, i + 1)

# Example usage
print(is_palindrome_single_pointer('nitin'))   # Output: True
print(is_palindrome_single_pointer('nitinr'))  # Output: False
```

---

## 3. Recursive Palindrome Check Using Substring Slicing

### Explanation
This variant recursively checks by stripping the first and last characters and passing the remaining substring. It's simple but inefficient due to repeated string slicing.

### Approach
- Base case: if string length is 0 or 1, it's a palindrome.
- If first and last characters differ, return False.
- Otherwise, recursively check the substring from index 1 to len-2.

### Algorithm
1. If `len(s) <= 1`, return True.
2. If `s[0] != s[-1]`, return False.
3. Return `palindrome(s[1:-1])`.

### Pseudocode
```
function isPalindrome(s):
    if len(s) <= 1:
        return True
    if s[0] != s[-1]:
        return False
    return isPalindrome(s[1:-1])
```

### Code
```python
def is_palindrome_slice(s):
    # Base case: empty or single character is palindrome
    if len(s) <= 1:
        return True
    
    # Compare first and last characters
    if s[0] != s[-1]:
        return False
    
    # Recursive call on substring excluding first and last
    return is_palindrome_slice(s[1:-1])

# Example usage
print(is_palindrome_slice('nitin'))   # Output: True
print(is_palindrome_slice('nitinr'))  # Output: False
```

---

## 4. Recursive Palindrome with Default Arguments (Pythonic)

### Explanation
Using Python's default arguments, we can encapsulate the start and end indices within the same function without a helper. This is a compact way to implement the two-pointer method.

### Approach
- Define function with optional `start` and `end` parameters defaulting to 0 and len(s)-1.
- In the recursive call, explicitly pass updated indices.
- Base case when `start >= end`.

### Algorithm
1. If `start >= end`, return True.
2. If `s[start] != s[end]`, return False.
3. Return `palindrome(s, start+1, end-1)`.

### Pseudocode
```
function isPalindrome(s, start=0, end=len(s)-1):
    if start >= end:
        return True
    if s[start] != s[end]:
        return False
    return isPalindrome(s, start+1, end-1)
```

### Code
```python
def is_palindrome_default(s, start=0, end=None):
    # Set end on first call
    if end is None:
        end = len(s) - 1
    
    # Base case: pointers crossed or met
    if start >= end:
        return True
    
    # Mismatch found
    if s[start] != s[end]:
        return False
    
    # Recursive call with incremented start and decremented end
    return is_palindrome_default(s, start + 1, end - 1)

# Example usage
print(is_palindrome_default('nitin'))   # Output: True
print(is_palindrome_default('nitinr'))  # Output: False
```

---

## 5. Tail-Recursive Style Palindrome (Conceptual)

### Explanation
Tail recursion occurs when the recursive call is the last operation. While Python doesn't optimize tail recursion, we can structure the code to be tail-recursive by passing an accumulator (here, a boolean) and using it to avoid further computation after a mismatch.

### Approach
- Use an accumulator `result` that is True initially.
- At each step, if mismatch, set result to False and propagate.
- Otherwise, continue recursion with updated indices.
- The recursive call returns the accumulator directly.

### Algorithm
1. If `start >= end`, return True.
2. If `s[start] != s[end]`, return False.
3. Return `palindrome(s, start+1, end-1, True)`.

### Pseudocode
```
function isPalindrome(s, start, end, acc):
    if start >= end:
        return acc
    if s[start] != s[end]:
        return False
    return isPalindrome(s, start+1, end-1, True)
```

### Code
```python
def is_palindrome_tail(s, start, end, acc=True):
    # Base case: if pointers cross, return accumulator (should be True)
    if start >= end:
        return acc
    
    # If mismatch, return False immediately (no further recursion needed)
    if s[start] != s[end]:
        return False
    
    # Tail-recursive call (last operation)
    return is_palindrome_tail(s, start + 1, end - 1, True)

def check_palindrome_tail(s):
    return is_palindrome_tail(s, 0, len(s)-1)

# Example usage
print(check_palindrome_tail('nitin'))   # Output: True
print(check_palindrome_tail('nitinr'))  # Output: False
```

---

## 6. Palindrome Check Using Recursive String Reversal

### Explanation
A string is a palindrome if it equals its reverse. This variant recursively builds the reversed string and compares at the end. It's not efficient but demonstrates recursion on strings.

### Approach
- Recursively reverse the string.
- Compare the original with the reversed version.

### Algorithm
1. Reverse the string recursively:
   - Base: if string is empty, return empty string.
   - Recursive: `reverse(s[1:]) + s[0]`.
2. Compare original with reversed.

### Pseudocode
```
function reverse(s):
    if s == "":
        return ""
    return reverse(s[1:]) + s[0]

function isPalindrome(s):
    return s == reverse(s)
```

### Code
```python
def reverse_string(s):
    # Base case: empty string
    if s == "":
        return ""
    # Recursive step: reverse the rest and append first character
    return reverse_string(s[1:]) + s[0]

def is_palindrome_reverse(s):
    # Check if string equals its reverse
    return s == reverse_string(s)

# Example usage
print(is_palindrome_reverse('nitin'))   # Output: True
print(is_palindrome_reverse('nitinr'))  # Output: False
```

---

## 7. Recursive Palindrome with Early Exit on Mismatch

### Explanation
This variant emphasizes early termination: as soon as a mismatch is found, it returns False without further recursion. It's essentially the same as the classic version but structured to highlight the early exit.

### Approach
- At each recursive call, check the current pair.
- If mismatch, return False immediately.
- Otherwise, recurse only if there are more characters to check.

### Algorithm
1. If `start >= end`, return True.
2. If `s[start] != s[end]`, return False.
3. If `start+1 < end-1`, recurse; else return True.

### Pseudocode
```
function isPalindrome(s, start, end):
    if start >= end:
        return True
    if s[start] != s[end]:
        return False
    return isPalindrome(s, start+1, end-1)
```

### Code
```python
def is_palindrome_early(s, start, end):
    # Base case: no more characters to compare
    if start >= end:
        return True
    
    # If current characters don't match, return False immediately
    if s[start] != s[end]:
        return False
    
    # Recursive call only if there are more characters to compare
    # (This condition is implicit; the recursive call will handle it)
    return is_palindrome_early(s, start + 1, end - 1)

# Example usage
print(is_palindrome_early('nitin', 0, 4))   # Output: True
print(is_palindrome_early('nitinr', 0, 5))  # Output: False
```

---

## 8. Recursive Palindrome Ignoring Case and Non-Alphanumeric Characters

### Explanation
This variant preprocesses the string to consider only alphanumeric characters and ignores case. It then applies a recursive palindrome check on the cleaned string.

### Approach
- Clean the string: keep only alphanumeric characters and convert to lowercase.
- Then use any recursive method on the cleaned string.

### Algorithm
1. Clean string: `filter(str.isalnum, s.lower())` to get a new string `clean`.
2. Apply recursive palindrome check on `clean`.

### Pseudocode
```
function cleanString(s):
    result = ""
    for ch in s:
        if ch.isalnum():
            result += ch.lower()
    return result

function isPalindrome(s):
    clean = cleanString(s)
    return isPalindromeHelper(clean, 0, len(clean)-1)
```

### Code
```python
def clean_string(s):
    # Build cleaned string: alphanumeric only, lowercased
    cleaned = []
    for ch in s:
        if ch.isalnum():
            cleaned.append(ch.lower())
    return ''.join(cleaned)

def is_palindrome_helper(s, start, end):
    if start >= end:
        return True
    if s[start] != s[end]:
        return False
    return is_palindrome_helper(s, start + 1, end - 1)

def is_palindrome_ignore_case(s):
    cleaned = clean_string(s)
    return is_palindrome_helper(cleaned, 0, len(cleaned) - 1)

# Example usage
print(is_palindrome_ignore_case("A man, a plan, a canal: Panama"))  # Output: True
print(is_palindrome_ignore_case("race a car"))                      # Output: False
```

---

## 9. Recursive Palindrome Check for Integers

### Explanation
An integer palindrome reads the same forwards and backwards. This variant converts the integer to a string and then applies recursion, or uses mathematical recursion to extract digits.

### Approach
- Convert integer to string and use any string palindrome method.
- Alternatively, use recursion to build the reversed number and compare.

### Algorithm (string-based)
1. Convert integer to string: `str(num)`.
2. Apply recursive palindrome check on the string.

### Pseudocode
```
function isPalindromeInt(num):
    s = str(num)
    return isPalindromeHelper(s, 0, len(s)-1)
```

### Code
```python
def is_palindrome_int_string(num):
    # Convert to string and use recursive helper
    s = str(num)
    return is_palindrome_helper(s, 0, len(s) - 1)  # using helper from variant 1

# Alternatively, mathematical recursion
def is_palindrome_int_math(num, original=None, reversed_num=0):
    # First call initialization
    if original is None:
        original = num
    # Base case: when num becomes 0
    if num == 0:
        return original == reversed_num
    # Recursive step: build reversed number
    reversed_num = reversed_num * 10 + num % 10
    return is_palindrome_int_math(num // 10, original, reversed_num)

# Example usage
print(is_palindrome_int_string(12321))   # Output: True
print(is_palindrome_int_string(12345))   # Output: False
print(is_palindrome_int_math(12321))     # Output: True
```

---

## 10. Recursive Palindrome Check for Linked List

### Explanation
A linked list can be checked for palindrome by recursively comparing the first half with the reversed second half. This variant uses recursion to reach the end and then compares nodes as the recursion unwinds.

### Approach
- Use a recursive function that returns a boolean and also carries a pointer to the corresponding node from the front.
- The recursion goes to the end, then on return, compares the current node with the front pointer and moves the front pointer forward.

### Algorithm
1. Define a recursive function that takes a node `right` and a mutable reference to a `left` node.
2. If `right` is None, return True (base case).
3. Recursively call with `right.next`.
4. On return, check if `left.val == right.val`. If not, return False.
5. Move `left` to `left.next`.
6. Return the result of the recursive call (which is already processed).

### Pseudocode
```
class ListNode:
    val, next

function isPalindromeList(head):
    left = head
    return check(head, left)

function check(right, leftRef):
    if right is None:
        return True
    if not check(right.next, leftRef):
        return False
    if right.val != leftRef.val:
        return False
    leftRef = leftRef.next
    return True
```

### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def is_palindrome_list(head):
    # Use a mutable container for left pointer
    left = [head]

    def recursive_check(right):
        # Base case: reached end
        if right is None:
            return True

        # Recursively go to the end
        if not recursive_check(right.next):
            return False

        # Compare current right node with left node
        if right.val != left[0].val:
            return False

        # Move left pointer forward
        left[0] = left[0].next
        return True

    return recursive_check(head)

# Helper to create linked list from list
def array_to_list(arr):
    dummy = ListNode()
    current = dummy
    for val in arr:
        current.next = ListNode(val)
        current = current.next
    return dummy.next

# Example usage
head = array_to_list([1, 2, 3, 2, 1])
print(is_palindrome_list(head))  # Output: True

head2 = array_to_list([1, 2, 3, 4, 5])
print(is_palindrome_list(head2)) # Output: False
```