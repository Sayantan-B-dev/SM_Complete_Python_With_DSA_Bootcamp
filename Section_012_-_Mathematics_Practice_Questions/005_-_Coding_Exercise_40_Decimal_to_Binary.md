## Built-in bin
```python
def int_to_binary(n):
    if n < 0:                               # Handle negative numbers
        return "-" + bin(abs(n))[2:]         # Add minus sign, use bin on absolute value, slice '0b'
    return bin(n)[2:]                        # Positive: bin gives '0b...', slice off prefix

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Uses Python's built-in bin function, most concise
```

## Manual Division Loop
```python
def int_to_binary(n):
    if n == 0:                              # Special case for zero
        return "0"
    sign = "-" if n < 0 else ""              # Remember sign
    n = abs(n)                               # Work with absolute value
    binary = ""
    while n > 0:                             # Continue until n becomes 0
        binary = str(n % 2) + binary          # Prepend remainder (LSB)
        n //= 2                               # Integer division by 2
    return sign + binary                      # Add sign and return

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Classic algorithm using repeated division by 2
```

## Recursive with String Concatenation
```python
def int_to_binary(n):
    if n < 0:                                # Handle negative
        return "-" + int_to_binary(abs(n))
    if n == 0:                               # Base case
        return "0"
    return int_to_binary(n // 2) + str(n % 2)  # Recurse on quotient, append remainder

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Recursive approach, builds string from most significant to least
```

## Bitwise Right Shift
```python
def int_to_binary(n):
    if n < 0:
        return "-" + int_to_binary(abs(n))
    if n == 0:
        return "0"
    binary = ""
    while n > 0:
        binary = ("1" if n & 1 else "0") + binary  # Check LSB using bitwise AND
        n >>= 1                                    # Right shift by one bit
    return binary

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Uses bitwise operations (AND and shift) instead of modulo and division
```

## Format Specifier (f-string)
```python
def int_to_binary(n):
    if n < 0:
        return "-" + f"{abs(n):b}"           # Use format specifier :b for binary
    return f"{n:b}"                          # Positive uses same format

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Python's formatted string literal with binary conversion
```

## While Loop with List and Join
```python
def int_to_binary(n):
    if n == 0:
        return "0"
    sign = "-" if n < 0 else ""
    n = abs(n)
    bits = []                                 # Collect bits in list
    while n > 0:
        bits.append(str(n % 2))                # Append remainder (LSB first)
        n //= 2
    bits.reverse()                             # Reverse to correct order
    return sign + "".join(bits)

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Collects bits in list then joins, avoids repeated string concatenation
```

## Stack-Based
```python
def int_to_binary(n):
    if n == 0:
        return "0"
    sign = "-" if n < 0 else ""
    n = abs(n)
    stack = []                                # Use stack to store remainders
    while n > 0:
        stack.append(str(n % 2))               # Push remainder
        n //= 2
    binary = ""
    while stack:                               # Pop in LIFO order (reverse)
        binary += stack.pop()
    return sign + binary

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Explicit stack (list) to reverse the order naturally
```

## Bit-Length Loop
```python
def int_to_binary(n):
    if n < 0:
        return "-" + int_to_binary(abs(n))
    if n == 0:
        return "0"
    # Determine number of bits needed (bit_length returns position of highest 1)
    bits = n.bit_length()
    binary = ""
    for i in range(bits-1, -1, -1):           # From most significant bit down
        binary += "1" if (n >> i) & 1 else "0"  # Check each bit using shift
    return binary

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Iterates over each bit position using bit_length and bit masking
```

## Using math.log and Powers
```python
import math

def int_to_binary(n):
    if n < 0:
        return "-" + int_to_binary(abs(n))
    if n == 0:
        return "0"
    # Find largest power of 2 <= n
    highest_power = int(math.log2(n))
    binary = ""
    for power in range(highest_power, -1, -1):
        if n >= (1 << power):                  # If 2^power fits
            binary += "1"
            n -= (1 << power)                    # Subtract that power
        else:
            binary += "0"
    return binary

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Builds binary by subtracting powers of two (like greedy algorithm)
```

## Using array of bits and bit_length
```python
def int_to_binary(n):
    if n < 0:
        return "-" + int_to_binary(abs(n))
    if n == 0:
        return "0"
    # Create a list of bits initially all zeros, then fill from MSB
    bits = ['0'] * n.bit_length()
    for i in range(len(bits)):
        if n >> (len(bits)-1-i) & 1:           # Check each bit from left
            bits[i] = '1'
    return ''.join(bits)

print(int_to_binary(42))   # "101010"
print(int_to_binary(-42))  # "-101010"
# Special: Pre-allocates list of correct size, then fills bits by position
```