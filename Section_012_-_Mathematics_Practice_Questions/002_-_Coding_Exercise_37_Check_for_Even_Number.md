## Modulo Operator
```python
def is_even(n):
    return n % 2 == 0                    # Check remainder when divided by 2

print(is_even(4))  # Call with even number - True
print(is_even(7))  # Call with odd number - False
# Special: Most common and readable approach
```

## Bitwise AND
```python
def is_even(n):
    return (n & 1) == 0                  # Check least significant bit (1 for odd)

print(is_even(4))  # 4 in binary 100, LSB 0 -> True
print(is_even(7))  # 7 in binary 111, LSB 1 -> False
# Special: Fast bit manipulation, works on integers
```

## Division and Multiplication
```python
def is_even(n):
    return (n // 2) * 2 == n             # Integer divide by 2, multiply back

print(is_even(4))  # (4//2)*2 = 2*2 = 4 -> True
print(is_even(7))  # (7//2)*2 = 3*2 = 6 != 7 -> False
# Special: Uses property that even numbers are divisible by 2
```

## Integer Division Check
```python
def is_even(n):
    return n / 2 == n // 2               # Compare float division with integer division

print(is_even(4))  # 4/2=2.0, 4//2=2 -> True
print(is_even(7))  # 7/2=3.5, 7//2=3 -> False
# Special: Relies on exact division for even numbers
```

## Last Digit Check
```python
def is_even(n):
    return str(n)[-1] in '02468'         # Check if last digit is even

print(is_even(4))   # '4' in '02468' -> True
print(is_even(7))   # '7' in '02468' -> False
print(is_even(-4))  # '-4' last digit '4' -> True (handles negatives)
# Special: Works on string representation, handles negatives
```

## Recursive Subtraction
```python
def is_even(n):
    n = abs(n)                            # Work with absolute value
    if n == 0:                             # Base case: zero is even
        return True
    if n == 1:                             # Base case: one is odd
        return False
    return is_even(n - 2)                  # Recurse subtracting 2

print(is_even(4))  # 4 -> 2 -> 0 -> True
print(is_even(7))  # 7 -> 5 -> 3 -> 1 -> False
# Special: Recursive approach, demonstrates base cases
```

## Lambda Function
```python
is_even = lambda n: n % 2 == 0           # Lambda expression assigned to variable

print(is_even(4))  # Call lambda - True
print(is_even(7))  # Call lambda - False
# Special: Anonymous function style, concise
```

## Using Divmod
```python
def is_even(n):
    _, remainder = divmod(n, 2)           # divmod returns quotient and remainder
    return remainder == 0                 # Check remainder

print(is_even(4))  # divmod(4,2) = (2,0) -> True
print(is_even(7))  # divmod(7,2) = (3,1) -> False
# Special: Uses built-in divmod function
```

## Type Conversion Trick
```python
def is_even(n):
    return float(n).is_integer() and int(n) % 2 == 0  # Ensure integer then check even

print(is_even(4))   # True
print(is_even(7))   # False
print(is_even(4.0)) # Also True (accepts float but checks integer)
# Special: Handles floats that represent integers
```

## Using List of Booleans
```python
def is_even(n):
    # Create a list where index 0 is even, index 1 is odd
    return [True, False][n % 2]           # Index into list based on modulo

print(is_even(4))  # 4%2=0 -> index 0 -> True
print(is_even(7))  # 7%2=1 -> index 1 -> False
# Special: Uses list indexing as a lookup table
```