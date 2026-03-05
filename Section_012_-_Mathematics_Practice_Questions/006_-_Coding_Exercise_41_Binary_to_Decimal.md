## Built-in int with Base
```python
def binary_to_decimal(binary_str):
    return int(binary_str, 2)               # Convert binary string to integer using base 2

print(binary_to_decimal("1010"))  # Call with "1010" - 10
print(binary_to_decimal("1111"))  # Call with "1111" - 15
# Special: Python's built-in int function with base argument is simplest and fastest
```

## Manual Power Calculation (Enumerate)
```python
def binary_to_decimal(binary_str):
    decimal = 0                             # Initialize result
    # Iterate from most significant to least, using index to compute power
    for i, digit in enumerate(binary_str):
        power = len(binary_str) - 1 - i      # Power of 2 for this digit position
        decimal += int(digit) * (2 ** power) # Add digit * 2^power
    return decimal                           # Return computed decimal

print(binary_to_decimal("1010"))  # 1*8 + 0*4 + 1*2 + 0*1 = 10
print(binary_to_decimal("1111"))  # 1*8+1*4+1*2+1*1 = 15
# Special: Explicitly calculates each digit's contribution using powers of 2
```

## Reduce with Lambda
```python
from functools import reduce

def binary_to_decimal(binary_str):
    # reduce accumulates: start with 0, for each digit d, new = acc*2 + int(d)
    return reduce(lambda acc, d: acc * 2 + int(d), binary_str, 0)

print(binary_to_decimal("1010"))  # (((0*2+1)*2+0)*2+1)*2+0 = 10
print(binary_to_decimal("1111"))  # 15
# Special: Functional style using reduce, follows the same algorithm as original
```

## While Loop with Index
```python
def binary_to_decimal(binary_str):
    decimal = 0                             # Initialize result
    i = 0                                   # Index for iterating
    while i < len(binary_str):               # Loop until end of string
        decimal = decimal * 2 + int(binary_str[i])  # Update decimal
        i += 1                               # Move to next digit
    return decimal                           # Return final decimal

print(binary_to_decimal("1010"))  # 10
print(binary_to_decimal("1111"))  # 15
# Special: While loop version of the original for loop, shows manual index control
```

## Recursive Accumulation
```python
def binary_to_decimal(binary_str, acc=0):
    if not binary_str:                       # Base case: empty string
        return acc                             # Return accumulated value
    # Recursive case: process first digit, multiply acc by 2, add digit, recurse on rest
    return binary_to_decimal(binary_str[1:], acc * 2 + int(binary_str[0]))

print(binary_to_decimal("1010"))  # 10
print(binary_to_decimal("1111"))  # 15
# Special: Recursive approach with accumulator, processes from left to right
```

## Bit Shifting (Using int conversion then shifting)
```python
def binary_to_decimal(binary_str):
    decimal = 0                             # Initialize
    for digit in binary_str:
        decimal = (decimal << 1) | int(digit)  # Left shift by 1 and OR with new digit
    return decimal

print(binary_to_decimal("1010"))  # 10
print(binary_to_decimal("1111"))  # 15
# Special: Uses bitwise left shift and OR to build the number, mimicking binary construction
```

## List Comprehension with Sum and Enumerate (Reverse)
```python
def binary_to_decimal(binary_str):
    # Process digits from least significant (right) using reversed enumerate
    return sum(int(digit) * (2 ** i) for i, digit in enumerate(reversed(binary_str)))

print(binary_to_decimal("1010"))  # 0*1 + 1*2 + 0*4 + 1*8 = 10
print(binary_to_decimal("1111"))  # 1+2+4+8 = 15
# Special: Sum over powers with digits from least significant side
```

## Math.pow with Reversed String
```python
def binary_to_decimal(binary_str):
    decimal = 0
    length = len(binary_str)
    for i, digit in enumerate(binary_str[::-1]):  # Process from LSB
        decimal += int(digit) * (2 ** i)           # Add digit * 2^i
    return decimal

print(binary_to_decimal("1010"))  # 10
print(binary_to_decimal("1111"))  # 15
# Special: Explicitly uses math concept of powers, iterating from least significant bit
```

## Eval with '0b' Prefix
```python
def binary_to_decimal(binary_str):
    # Prepend '0b' to indicate binary and evaluate
    return eval('0b' + binary_str)

print(binary_to_decimal("1010"))  # 10
print(binary_to_decimal("1111"))  # 15
# Special: Uses eval on Python literal, not recommended for production but demonstrates another way
```

## Using int.from_bytes (for completeness)
```python
def binary_to_decimal(binary_str):
    # Convert binary string to integer by interpreting as bytes
    # First convert each character to byte, then use int.from_bytes (big-endian)
    byte_array = binary_str.encode()         # Get bytes of the string (not binary conversion!)
    # This is not a correct method for binary string; it's a trick for demonstration.
    # Actually, we need to convert the string to integer using base 2.
    # This variant is intentionally wrong to show what not to do.
    # A correct but overcomplicated method: use int(binary_str, 2) or manual.
    # Instead, we'll show a correct one using int.from_bytes with bit_length?
    # Let's do a proper one: convert binary string to integer using bit shifting
    # But we already did that. I'll do a different one: using a dictionary mapping.
    # However, I need 10 variants, so I'll include this as a cautionary example.
    # Better: use a manual method with ord and bit shifting.
    # I'll do a correct but unusual method: using str.maketrans and int with base? That's still int.
    # Alternatively: use a while loop with character conversion.
    # I'll do a variant using a lookup table for each bit position.
    pass  # Not valid; I'll replace with a proper variant.

# Let's add a proper 10th: using a loop with string reversal and positional multiplication
# Already have that. Instead, I'll do one using a stack:
```

## Stack-Based Reversal
```python
def binary_to_decimal(binary_str):
    stack = list(binary_str)                 # Push all digits onto stack (as characters)
    decimal = 0
    power = 0
    while stack:
        digit = int(stack.pop())              # Pop from end (least significant first)
        decimal += digit * (2 ** power)       # Add contribution
        power += 1
    return decimal

print(binary_to_decimal("1010"))  # 0*1 + 1*2 + 0*4 + 1*8 = 10
print(binary_to_decimal("1111"))  # 1+2+4+8 = 15
# Special: Uses stack (list as stack) to process digits from least significant to most
```