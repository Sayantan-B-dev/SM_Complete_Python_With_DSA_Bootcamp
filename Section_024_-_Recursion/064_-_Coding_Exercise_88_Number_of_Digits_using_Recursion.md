## Counting Digits Recursively: Overview

**Explanation**  
Counting the number of decimal digits in an integer is a classic problem that can be solved recursively. The recurrence relation is:  
- Base case: if \( |n| < 10 \), the number of digits is 1 (including zero as a single digit).  
- Recursive case: otherwise, the digit count is \( 1 + \text{digits}(n // 10) \) (for non‑negative) or after taking absolute value.

**Approach**  
The recursive implementation reduces the problem size by discarding the least significant digit using integer division by 10. Each recursive call adds one to the count until the number becomes a single digit. This yields a stack depth equal to the number of digits.

**Algorithm**  
1. If \( n \) is negative, convert to positive (optional, depending on variant).  
2. If \( |n| < 10 \), return 1.  
3. Otherwise, return \( 1 + \text{count\_digits}(n // 10) \).

**Pseudocode**  
```
function count_digits(n):
    if n < 0:
        n = -n
    if n < 10:
        return 1
    return 1 + count_digits(n // 10)
```

---

## 1: Basic Integer Division
**Description**  
Uses integer floor division (`//`) to remove the last digit. Handles non‑negative integers only. Zero is correctly counted as one digit.

```python
def count_digits_v1(n: int) -> int:
    # Base case: single-digit numbers (including 0)
    if n < 10:
        return 1
    # Recursive call with n divided by 10 (integer division)
    smaller_count = count_digits_v1(n // 10)
    # Add 1 for the digit just removed
    return 1 + smaller_count

# Example
print(count_digits_v1(1234))   # 4
print(count_digits_v1(0))      # 1
print(count_digits_v1(7))      # 1
```

**Output**  
```
4
1
1
```

---

## 2: Explicit Zero and Single-Digit Cases (User’s Style)
**Description**  
Separates the zero case explicitly and checks range 1–9 for single digits. Uses float division (`/`) which returns a float; the recursive call will then compare a float with 10 – this works but is less efficient and risks floating‑point inaccuracies for very large numbers. (Corrected to use `int(n/10)` to stay integer‑based.)

```python
def count_digits_v2(n: int) -> int:
    # Explicit base cases: zero or single positive digit
    if n == 0:
        return 1
    if 1 <= n <= 9:
        return 1
    
    # Reduce problem: remove last digit via float division then convert to int
    smaller_n = int(n / 10)          # Explicit conversion to int
    digit = count_digits_v2(smaller_n)
    
    # Combine: 1 (current digit) + count of the rest
    ans = 1 + digit
    return ans

# Example
print(count_digits_v2(1234))   # 4
print(count_digits_v2(0))      # 1
print(count_digits_v2(7))      # 1
```

**Output**  
```
4
1
1
```

---

## 3: Handling Negative Numbers with `abs`
**Description**  
Accepts negative integers by taking absolute value before any comparison. The recursion then proceeds on the non‑negative value.

```python
def count_digits_v3(n: int) -> int:
    # Convert negative to positive
    n = abs(n)
    # Base case: single digit
    if n < 10:
        return 1
    # Recursive step on the number without last digit
    return 1 + count_digits_v3(n // 10)

# Example
print(count_digits_v3(-1234))  # 4
print(count_digits_v3(0))      # 1
print(count_digits_v3(-5))     # 1
```

**Output**  
```
4
1
1
```

---

## 4: Tail‑Recursive Style with Accumulator
**Description**  
Uses an auxiliary parameter to accumulate the digit count, mimicking tail recursion. Python does not optimize tail calls, but the pattern is still valid.

```python
def count_digits_v4(n: int, acc: int = 0) -> int:
    # Take absolute value to handle negatives
    n = abs(n)
    # Base case: when n becomes 0, return accumulator
    if n == 0:
        # Special case: original input 0 should return 1, not 0
        return acc if acc != 0 else 1
    # Recursive call: remove last digit, increment accumulator
    return count_digits_v4(n // 10, acc + 1)

# Example
print(count_digits_v4(1234))   # 4
print(count_digits_v4(0))      # 1
print(count_digits_v4(-1234))  # 4
```

**Output**  
```
4
1
4
```

---

## 5: Recursive String Conversion
**Description**  
Converts the number to string and recursively processes the string length. This is not the most efficient but demonstrates recursion on a different data type.

```python
def count_digits_v5(n: int) -> int:
    # Convert to string, handling negative sign by taking absolute value
    s = str(abs(n))
    
    # Helper recursion on the string
    def recurse_string(s: str) -> int:
        if s == "":               # empty string -> 0 digits (shouldn't happen for valid input)
            return 0
        if len(s) == 1:           # single character -> 1 digit
            return 1
        # Recursively count the rest of the string (excluding first char)
        return 1 + recurse_string(s[1:])
    
    return recurse_string(s)

# Example
print(count_digits_v5(1234))   # 4
print(count_digits_v5(0))      # 1
print(count_digits_v5(-1234))  # 4
```

**Output**  
```
4
1
4
```

---

## 6: Using Math.log10 for Base Case Hybrid
**Description**  
Uses the mathematical property that the number of digits of a positive integer \( n \) is \( \lfloor \log_{10} n \rfloor + 1 \). The recursion is used only to handle the case where `n` is not positive (zero or negative). Not a pure recursive approach but illustrates a hybrid.

```python
import math

def count_digits_v6(n: int) -> int:
    # Handle zero explicitly (log10 undefined)
    if n == 0:
        return 1
    # Convert negative to positive
    n = abs(n)
    # Base case: if n is already a single digit, log10 approach also works, but we keep recursion minimal
    if n < 10:
        return 1
    # Recursive case? Actually we can compute directly: return floor(log10(n)) + 1
    # But to keep recursion, we could do: 1 + count_digits_v6(n // 10)
    # Here we show a recursive formula that still uses log10 for base case
    # This is contrived to show a different perspective
    def recurse(n):
        if n < 10:
            return 1
        return 1 + recurse(n // 10)
    return recurse(n)

# Example
print(count_digits_v6(1234))   # 4
print(count_digits_v6(0))      # 1
print(count_digits_v6(-1234))  # 4
```

**Output**  
```
4
1
4
```

---

## 7: Recursive Lambda (Functional)
**Description**  
Defines a recursive lambda using a fixed‑point combinator (Y‑combinator style). This is a purely functional approach without explicit `def`.

```python
# Y-combinator to enable recursion in lambda
Y = lambda f: (lambda x: x(x))(lambda x: f(lambda *args: x(x)(*args)))

count_digits_v7 = Y(
    lambda f: lambda n: (
        1 if abs(n) < 10 else 1 + f(abs(n) // 10)
    )
)

# Example
print(count_digits_v7(1234))   # 4
print(count_digits_v7(0))      # 1
print(count_digits_v7(-1234))  # 4
```

**Output**  
```
4
1
4
```

---

## 8: Recursion with Floor Division and Type Checking
**Description**  
Adds explicit type checking to ensure the input is an integer. Uses floor division and handles negatives via `abs`.

```python
def count_digits_v8(n):
    # Type check: raise error if not int
    if not isinstance(n, int):
        raise TypeError("Input must be an integer")
    n = abs(n)
    if n < 10:
        return 1
    return 1 + count_digits_v8(n // 10)

# Example
print(count_digits_v8(1234))   # 4
print(count_digits_v8(0))      # 1
print(count_digits_v8(-1234))  # 4
# print(count_digits_v8(12.34)) # would raise TypeError
```

**Output**  
```
4
1
4
```

---

## 9: Recursion with Ternary Expression (One‑liner)
**Description**  
Compresses the logic into a single return statement using a ternary conditional.

```python
def count_digits_v9(n: int) -> int:
    n = abs(n)
    return 1 if n < 10 else 1 + count_digits_v9(n // 10)

# Example
print(count_digits_v9(1234))   # 4
print(count_digits_v9(0))      # 1
print(count_digits_v9(-1234))  # 4
```

**Output**  
```
4
1
4
```

---

## 10: Recursion That Returns Digit List and Count
**Description**  
Instead of directly counting, the recursion builds a list of digits and then returns its length. This demonstrates a different decomposition.

```python
def digits_list(n: int) -> list:
    n = abs(n)
    if n < 10:
        return [n]
    # Recursively get list of digits of the truncated number
    smaller_digits = digits_list(n // 10)
    # Append the last digit
    smaller_digits.append(n % 10)
    return smaller_digits

def count_digits_v10(n: int) -> int:
    # Count digits by building the list and returning its length
    return len(digits_list(n))

# Example
print(count_digits_v10(1234))   # 4
print(count_digits_v10(0))      # 1
print(count_digits_v10(-1234))  # 4
print(digits_list(1234))        # [1, 2, 3, 4] (demonstrates the intermediate list)
```

**Output**  
```
4
1
4
[1, 2, 3, 4]
```