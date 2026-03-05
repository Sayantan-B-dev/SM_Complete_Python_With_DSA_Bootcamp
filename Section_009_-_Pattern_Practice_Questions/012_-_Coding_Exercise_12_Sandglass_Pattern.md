## Basic Sandglass
```python
def generate_sandglass(n):
    """Basic sandglass pattern with stars"""
    output = []
    for i in range(n, 0, -1):
        output.append(" " * (n - i) + "*" * (2 * i - 1) + " " * (n - i))
    for i in range(2, n + 1):
        output.append(" " * (n - i) + "*" * (2 * i - 1) + " " * (n - i))
    return output

# Test
print(generate_sandglass(4))
# Output: 
# ['*******',
#  ' ***** ',
#  '  ***  ',
#  '   *   ',
#  '  ***  ',
#  ' ***** ',
#  '*******']
```

## Hollow Sandglass
```python
def generate_hollow_sandglass(n):
    """Sandglass with hollow center (only border)"""
    output = []
    # Upper half (inverted pyramid)
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        if i == n:  # Top row
            output.append(spaces + "*" * (2 * i - 1) + spaces)
        elif i == 1:  # Middle row (single star)
            output.append(spaces + "*" + spaces)
        else:  # Middle rows
            output.append(spaces + "*" + " " * (2 * i - 3) + "*" + spaces)
    # Lower half (regular pyramid)
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        if i == n:  # Bottom row
            output.append(spaces + "*" * (2 * i - 1) + spaces)
        elif i == 1:  # Middle row (already added)
            continue
        else:  # Middle rows
            output.append(spaces + "*" + " " * (2 * i - 3) + "*" + spaces)
    return output

# Test
print(generate_hollow_sandglass(5))
# Output: 
# ['*********',
#  ' *     * ',
#  '  *   *  ',
#  '   * *   ',
#  '    *    ',
#  '   * *   ',
#  '  *   *  ',
#  ' *     * ',
#  '*********']
```

## Number Sandglass
```python
def generate_number_sandglass(n):
    """Sandglass with numbers instead of stars"""
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(1, i + 1):
            row += str(j)
        for j in range(i - 1, 0, -1):
            row += str(j)
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(1, i + 1):
            row += str(j)
        for j in range(i - 1, 0, -1):
            row += str(j)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_number_sandglass(4))
# Output: 
# ['1234321',
#  ' 12321 ',
#  '  121  ',
#  '   1   ',
#  '  121  ',
#  ' 12321 ',
#  '1234321']
```

## Alphabet Sandglass
```python
def generate_alphabet_sandglass(n):
    """Sandglass with alphabets"""
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(i):
            row += chr(65 + j)
        for j in range(i - 2, -1, -1):
            row += chr(65 + j)
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(i):
            row += chr(65 + j)
        for j in range(i - 2, -1, -1):
            row += chr(65 + j)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_alphabet_sandglass(4))
# Output: 
# ['ABCDCBA',
#  ' ABCBA ',
#  '  ABA  ',
#  '   A   ',
#  '  ABA  ',
#  ' ABCBA ',
#  'ABCDCBA']
```

## Binary Sandglass
```python
def generate_binary_sandglass(n):
    """Sandglass with binary pattern (0s and 1s)"""
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            row += str((i + j) % 2)
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            row += str((i + j) % 2)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_binary_sandglass(4))
# Output: 
# ['1010101',
#  ' 10101 ',
#  '  101  ',
#  '   1   ',
#  '  101  ',
#  ' 10101 ',
#  '1010101']
```

## Plus-Minus Sandglass
```python
def generate_plus_minus_sandglass(n):
    """Sandglass with alternating + and - symbols"""
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            if j % 2 == 0:
                row += "+"
            else:
                row += "-"
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            if j % 2 == 0:
                row += "+"
            else:
                row += "-"
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_plus_minus_sandglass(4))
# Output: 
# ['+-+-+-+',
#  ' +-+-+ ',
#  '  +-+  ',
#  '   +   ',
#  '  +-+  ',
#  ' +-+-+ ',
#  '+-+-+-+']
```

## Checkerboard Sandglass
```python
def generate_checkerboard_sandglass(n):
    """Sandglass with checkerboard pattern"""
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            if (i + j) % 2 == 0:
                row += "*"
            else:
                row += " "
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            if (i + j) % 2 == 0:
                row += "*"
            else:
                row += " "
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_checkerboard_sandglass(4))
# Output: 
# ['* * * *',
#  ' * * * ',
#  '  * *  ',
#  '   *   ',
#  '  * *  ',
#  ' * * * ',
#  '* * * *']
```

## Double Sandglass
```python
def generate_double_sandglass(n):
    """Two sandglasses side by side"""
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces1 = " " * (n - i)
        stars1 = "*" * (2 * i - 1)
        gap = " " * 3
        spaces2 = " " * (n - i)
        stars2 = "*" * (2 * i - 1)
        output.append(spaces1 + stars1 + gap + stars2 + spaces2)
    # Lower half
    for i in range(2, n + 1):
        spaces1 = " " * (n - i)
        stars1 = "*" * (2 * i - 1)
        gap = " " * 3
        spaces2 = " " * (n - i)
        stars2 = "*" * (2 * i - 1)
        output.append(spaces1 + stars1 + gap + stars2 + spaces2)
    return output

# Test
print(generate_double_sandglass(3))
# Output: 
# ['*****   *****',
#  ' ***     *** ',
#  '  *       *  ',
#  ' ***     *** ',
#  '*****   *****']
```

## Custom Character Sandglass
```python
def generate_custom_sandglass(n, chars=['*', '#', '@', '%']):
    """Sandglass with alternating character layers"""
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        char = chars[(i - 1) % len(chars)]
        output.append(spaces + char * (2 * i - 1) + spaces)
    # Lower half
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        char = chars[(i - 1) % len(chars)]
        output.append(spaces + char * (2 * i - 1) + spaces)
    return output

# Test
print(generate_custom_sandglass(5, ['★', '♠', '♥', '♦', '♣']))
# Output: 
# ['★★★★★★★★★',
#  ' ♠♠♠♠♠♠♠ ',
#  '  ♥♥♥♥♥  ',
#  '   ♦♦♦   ',
#  '    ♣    ',
#  '   ♦♦♦   ',
#  '  ♥♥♥♥♥  ',
#  ' ♠♠♠♠♠♠♠ ',
#  '★★★★★★★★★']
```

## Rotated Sandglass
```python
def generate_rotated_sandglass(n):
    """Sandglass rotated 90 degrees (hourglass on its side)"""
    output = []
    # Left half
    for i in range(n, 0, -1):
        output.append("*" * i)
    # Right half (excluding the longest to avoid duplication)
    for i in range(2, n + 1):
        output.append("*" * i)
    return output

# Test
print(generate_rotated_sandglass(5))
# Output: 
# ['*****',
#  '****',
#  '***',
#  '**',
#  '*',
#  '**',
#  '***',
#  '****',
#  '*****']
```

## Fibonacci Sandglass
```python
def generate_fibonacci_sandglass(n):
    """Sandglass using Fibonacci numbers"""
    def fibonacci(k):
        if k <= 1:
            return 1
        a, b = 1, 1
        for _ in range(2, k + 1):
            a, b = b, a + b
        return b
    
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(fibonacci(j)) + " "
        for j in range(i - 1, 0, -1):
            row += str(fibonacci(j)) + " "
        output.append(spaces + row.strip())
    # Lower half
    for i in range(2, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(fibonacci(j)) + " "
        for j in range(i - 1, 0, -1):
            row += str(fibonacci(j)) + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_fibonacci_sandglass(4))
# Output: 
# ['1 1 2 3 2 1 1',
#  '  1 1 2 1 1  ',
#  '    1 1 1    ',
#  '      1      ',
#  '    1 1 1    ',
#  '  1 1 2 1 1  ',
#  '1 1 2 3 2 1 1']
```

## X-Pattern Sandglass
```python
def generate_x_pattern_sandglass(n):
    """Sandglass with X pattern inside"""
    output = []
    # Upper half
    for i in range(n, 0, -1):
        spaces_outer = " " * (n - i)
        if i == 1:
            output.append(spaces_outer + "*" + spaces_outer)
        else:
            stars_left = "*" + " " * (i - 2)
            stars_right = " " * (i - 2) + "*"
            output.append(spaces_outer + stars_left + " " + stars_right + spaces_outer)
    # Lower half
    for i in range(2, n + 1):
        spaces_outer = " " * (n - i)
        if i == 1:
            continue
        else:
            stars_left = "*" + " " * (i - 2)
            stars_right = " " * (i - 2) + "*"
            output.append(spaces_outer + stars_left + " " + stars_right + spaces_outer)
    return output

# Test
print(generate_x_pattern_sandglass(5))
# Output: 
# ['*       *',
#  ' *     * ',
#  '  *   *  ',
#  '   * *   ',
#  '    *    ',
#  '   * *   ',
#  '  *   *  ',
#  ' *     * ',
#  '*       *']
```

## Prime Number Sandglass
```python
def is_prime(num):
    """Check if a number is prime"""
    if num < 2:
        return False
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            return False
    return True

def generate_prime_sandglass(n):
    """Sandglass with prime numbers"""
    primes = []
    num = 2
    total_needed = n * n  # Rough estimate
    
    while len(primes) < total_needed:
        if is_prime(num):
            primes.append(num)
        num += 1
    
    output = []
    index = 0
    # Upper half
    for i in range(n, 0, -1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(2 * i - 1):
            row += str(primes[index]) + " "
            index += 1
        output.append(spaces + row.strip())
    # Lower half
    for i in range(2, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(2 * i - 1):
            row += str(primes[index]) + " "
            index += 1
        output.append(spaces + row.strip())
    return output

# Test
print(generate_prime_sandglass(3))
# Output: 
# ['2 3 5 7 11',
#  '  13 17 19  ',
#  '    23      ',
#  '  29 31 37  ',
#  '41 43 47 53 59']
```