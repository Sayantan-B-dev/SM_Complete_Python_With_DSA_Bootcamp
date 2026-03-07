## Plus One: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
The function takes a list of digits representing a non-negative integer (e.g., `[1,2,3]` represents 123) and increments it by one. It returns the new list of digits. This is similar to adding 1 to a number represented in array form.

**Approach:**  
The problem is essentially performing addition with carry. Starting from the least significant digit (rightmost), we add 1. If the digit becomes 10, we set it to 0 and propagate the carry to the next digit. If all digits become 9, we need an extra digit at the front.

**Algorithm:**  
1. Let `n = len(digits)`.  
2. If all digits are 9, return `[1] + [0] * n`.  
3. Start from the last index (`p = n-1`).  
4. While `p >= 0`:  
   - If `digits[p] < 9`, increment `digits[p]` by 1 and return.  
   - Else set `digits[p] = 0` and move left (`p -= 1`).  
5. (The loop will always find a non-9 digit before reaching -1 due to step 2.)

**Pseudocode:**  
```
function plusOne(digits):
    n = length(digits)
    if all digits are 9:
        return [1] concatenated with n zeros
    p = n - 1
    while p >= 0:
        if digits[p] < 9:
            digits[p] = digits[p] + 1
            return digits
        else:
            digits[p] = 0
            p = p - 1
```

---

## Original Iterative Plus One

```python
def plus_one_original(digits):
    n = len(digits)
    # Special case: all digits are 9 (e.g., [9,9,9])
    if set(digits) == {9}:
        return [1] + [0] * n          # create new list with leading 1 and zeros
    p = n - 1
    for _ in range(n):                 # loop from rightmost digit
        if digits[p] < 9:
            digits[p] += 1              # increment and return
            break
        else:
            digits[p] = 0                # set to 0 and continue carry
        p -= 1
    return digits

# Example call
digits = [1, 2, 9]
result = plus_one_original(digits)
print("Result:", result)   # Output: Result: [1, 3, 0]
```

---

## Convert to Integer and Back

```python
def plus_one_int_conversion(digits):
    # Convert list of digits to an integer
    num = int(''.join(map(str, digits)))   # join digits into string, then to int
    num += 1                                 # add one
    # Convert back to list of digits
    return [int(d) for d in str(num)]        # iterate over string representation

# Example call
digits = [1, 2, 9]
result = plus_one_int_conversion(digits)
print("Result:", result)   # Output: Result: [1, 3, 0]
```

---

## Recursive Plus One

```python
def plus_one_recursive(digits, index=None):
    if index is None:
        index = len(digits) - 1
    # Base case: if index goes negative, we need an extra digit
    if index < 0:
        return [1] + digits          # prepend 1 (all digits were 9)
    # Recursive case: add one to current digit
    if digits[index] < 9:
        digits[index] += 1
        return digits
    else:
        digits[index] = 0
        return plus_one_recursive(digits, index - 1)   # carry to left

# Example call
digits = [1, 2, 9]
result = plus_one_recursive(digits)
print("Result:", result)   # Output: Result: [1, 3, 0]
```

---

## While Loop with Explicit Carry Flag

```python
def plus_one_carry_flag(digits):
    carry = 1                     # start with 1 to add
    i = len(digits) - 1
    while i >= 0 and carry:
        total = digits[i] + carry
        digits[i] = total % 10     # new digit
        carry = total // 10         # carry for next
        i -= 1
    if carry:                      # if carry remains, need extra digit
        return [1] + digits
    return digits

# Example call
digits = [9, 9, 9]
result = plus_one_carry_flag(digits)
print("Result:", result)   # Output: Result: [1, 0, 0, 0]
```

---

## Using List Comprehension and `enumerate` on Reversed List

```python
def plus_one_enumerate_reversed(digits):
    # Process from the end using enumerate on reversed list
    result = []
    carry = 1
    for i, digit in enumerate(reversed(digits)):
        total = digit + carry
        result.append(total % 10)      # append new digit (will be reversed later)
        carry = total // 10
    if carry:
        result.append(1)                # extra carry
    # Reverse back to original order
    return list(reversed(result))

# Example call
digits = [1, 2, 9]
result = plus_one_enumerate_reversed(digits)
print("Result:", result)   # Output: Result: [1, 3, 0]
```

---

## Using `divmod` and Manual Propagation

```python
def plus_one_divmod(digits):
    carry = 1
    for i in range(len(digits) - 1, -1, -1):
        # divmod returns (quotient, remainder)
        carry, digits[i] = divmod(digits[i] + carry, 10)
        if carry == 0:
            break
    if carry:
        digits.insert(0, 1)            # insert at beginning
    return digits

# Example call
digits = [9, 9, 9]
result = plus_one_divmod(digits)
print("Result:", result)   # Output: Result: [1, 0, 0, 0]
```

---

## Using `itertools.accumulate` with Carry (Functional Style)

```python
from itertools import accumulate

def plus_one_accumulate(digits):
    # Reverse digits, accumulate with carry, then reverse back
    rev_digits = digits[::-1]
    # Use accumulate to propagate carry; but accumulate is for running totals,
    # so we need a custom approach. Instead, we'll simulate with a loop.
    # (This is a conceptual variant: using reduce from functools)
    # Alternative: use functools.reduce
    from functools import reduce
    def add_with_carry(acc, x):
        # acc is (result_list, carry)
        res_list, carry = acc
        total = x + carry
        res_list.append(total % 10)
        return (res_list, total // 10)
    result_rev, final_carry = reduce(add_with_carry, rev_digits, ([], 1))
    if final_carry:
        result_rev.append(1)
    return result_rev[::-1]

# Example call
digits = [1, 2, 9]
result = plus_one_accumulate(digits)
print("Result:", result)   # Output: Result: [1, 3, 0]
```

---

## Using a Stack to Process Digits

```python
def plus_one_stack(digits):
    stack = digits[:]                 # copy digits to stack (rightmost at top)
    carry = 1
    result_stack = []
    while stack:
        digit = stack.pop()
        total = digit + carry
        result_stack.append(total % 10)
        carry = total // 10
    if carry:
        result_stack.append(1)
    # result_stack is in reverse order, so reverse it
    return result_stack[::-1]

# Example call
digits = [9, 9, 9]
result = plus_one_stack(digits)
print("Result:", result)   # Output: Result: [1, 0, 0, 0]
```

---

## Using `map` and `str` Conversion with Zfill (Alternative)

```python
def plus_one_map_str(digits):
    # Convert digits to string, increment as integer, then pad with zeros if needed
    num_str = ''.join(map(str, digits))
    incremented = str(int(num_str) + 1)
    # If the length increased (e.g., 999 -> 1000), we just return list of incremented
    return [int(c) for c in incremented]

# Example call
digits = [1, 2, 9]
result = plus_one_map_str(digits)
print("Result:", result)   # Output: Result: [1, 3, 0]
```

---

## Using `numpy` for Vectorized Operations (Conceptual)

```python
import numpy as np

def plus_one_numpy(digits):
    arr = np.array(digits)
    # Convert to integer, add 1, then convert back to digits
    # But numpy can handle large numbers, but we'll simulate by converting to int
    # This is similar to the int conversion but with numpy's array capabilities.
    num = np.sum(arr * 10**np.arange(len(arr)-1, -1, -1))
    num += 1
    # Convert back to digits
    return [int(d) for d in str(num)]

# Example call
digits = [1, 2, 9]
result = plus_one_numpy(digits)
print("Result:", result)   # Output: Result: [1, 3, 0]
```