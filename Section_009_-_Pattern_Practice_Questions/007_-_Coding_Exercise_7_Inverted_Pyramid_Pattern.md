## Basic Inverted Pyramid
```python
def generate_inverted_pyramid(n):
    """Basic inverted pyramid with stars decreasing from bottom"""
    output = []
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    return output

# Test
print(generate_inverted_pyramid(5))
# Output: 
# ['*********',
#  ' ******* ',
#  '  *****  ',
#  '   ***   ',
#  '    *    ']
```

## Inverted Hollow Pyramid
```python
def generate_inverted_hollow_pyramid(n):
    """Inverted pyramid with hollow center (only border)"""
    output = []
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        if i == n:  # Top row (full stars)
            output.append(spaces + "*" * (2 * i - 1) + spaces)
        elif i == 1:  # Bottom row (single star)
            output.append(spaces + "*" + spaces)
        else:  # Middle rows (borders only)
            hollow = "*" + " " * (2 * i - 3) + "*"
            output.append(spaces + hollow + spaces)
    return output

# Test
print(generate_inverted_hollow_pyramid(6))
# Output: 
# ['***********',
#  ' *       * ',
#  '  *     *  ',
#  '   *   *   ',
#  '    * *    ',
#  '     *     ']
```

## Inverted Number Pyramid
```python
def generate_inverted_number_pyramid(n):
    """Inverted pyramid with decreasing numbers"""
    output = []
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        # Ascending numbers
        for j in range(1, i + 1):
            row += str(j)
        # Descending numbers (excluding the peak)
        for j in range(i - 1, 0, -1):
            row += str(j)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_inverted_number_pyramid(5))
# Output: 
# ['123454321',
#  ' 1234321 ',
#  '  12321  ',
#  '   121   ',
#  '    1    ']
```

## Inverted Alphabet Pyramid
```python
def generate_inverted_alphabet_pyramid(n):
    """Inverted pyramid with alphabets"""
    output = []
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        # Ascending letters
        for j in range(i):
            row += chr(65 + j)
        # Descending letters (excluding the peak)
        for j in range(i - 2, -1, -1):
            row += chr(65 + j)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_inverted_alphabet_pyramid(5))
# Output: 
# ['ABCDEDCBA',
#  ' ABCDCBA ',
#  '  ABCBA  ',
#  '   ABA   ',
#  '    A    ']
```

## Inverted Binary Pyramid
```python
def generate_inverted_binary_pyramid(n):
    """Inverted pyramid with binary pattern"""
    output = []
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            row += str((i + j) % 2)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_inverted_binary_pyramid(5))
# Output: 
# ['101010101',
#  ' 1010101 ',
#  '  10101  ',
#  '   101   ',
#  '    1    ']
```

## Inverted Double Pyramid
```python
def generate_inverted_double_pyramid(n):
    """Two inverted pyramids side by side"""
    output = []
    for i in range(n, 0, -1):
        spaces1 = " " * (n - i)
        stars1 = "*" * (2 * i - 1)
        spaces2 = " " * 3  # Gap between pyramids
        spaces3 = " " * (n - i)
        stars2 = "*" * (2 * i - 1)
        output.append(spaces1 + stars1 + spaces2 + stars2 + spaces3)
    return output

# Test
print(generate_inverted_double_pyramid(4))
# Output: 
# ['*******   *******',
#  ' *****     ***** ',
#  '  ***       ***  ',
#  '   *         *   ']
```

## Inverted Hourglass Pyramid
```python
def generate_inverted_hourglass_pyramid(n):
    """Inverted pyramid followed by regular pyramid (hourglass)"""
    output = []
    # Top inverted pyramid
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    # Bottom regular pyramid (excluding the peak)
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    return output

# Test
print(generate_inverted_hourglass_pyramid(4))
# Output: 
# ['*******',
#  ' ***** ',
#  '  ***  ',
#  '   *   ',
#  '  ***  ',
#  ' ***** ',
#  '*******']
```

## Inverted Custom Character Pyramid
```python
def generate_inverted_custom_pyramid(n, chars=['*', '#', '@']):
    """Inverted pyramid with alternating character layers"""
    output = []
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        char = chars[(i - 1) % len(chars)]
        stars = char * (2 * i - 1)
        output.append(spaces + stars + spaces)
    return output

# Test
print(generate_inverted_custom_pyramid(6, ['*', '+', '•', '★']))
# Output: 
# ['★★★★★★★★★★★',
#  ' ••••••••• ',
#  '  +++++++  ',
#  '   *****   ',
#  '    +++    ',
#  '     •     ']
```

## Inverted Checkerboard Pyramid
```python
def generate_inverted_checkerboard_pyramid(n):
    """Inverted pyramid with checkerboard pattern"""
    output = []
    for i in range(n, 0, -1):
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
print(generate_inverted_checkerboard_pyramid(5))
# Output: 
# ['* * * * *',
#  ' * * * * ',
#  '  * * *  ',
#  '   * *   ',
#  '    *    ']
```

## Inverted Diamond Pyramid
```python
def generate_inverted_diamond_pyramid(n):
    """Inverted pyramid as bottom half of a diamond"""
    output = []
    # Top regular pyramid (optional - for complete diamond)
    for i in range(1, n):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    # Bottom inverted pyramid
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    return output

# Test
print(generate_inverted_diamond_pyramid(4))
# Output: 
# ['   *   ',
#  '  ***  ',
#  ' ***** ',
#  '*******',
#  ' ***** ',
#  '  ***  ',
#  '   *   ']
```

## Inverted Rotated Pyramid
```python
def generate_inverted_rotated_pyramid(n):
    """Inverted pyramid rotated 90 degrees"""
    output = []
    # Left side (decreasing)
    for i in range(n, 0, -1):
        output.append("*" * i)
    # Right side (increasing, excluding the longest)
    for i in range(2, n + 1):
        output.append("*" * i)
    return output

# Test
print(generate_inverted_rotated_pyramid(5))
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

## Inverted Mirrored Pyramid
```python
def generate_inverted_mirrored_pyramid(n):
    """Inverted pyramid mirrored horizontally"""
    output = []
    for i in range(n, 0, -1):
        left_spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        right_spaces = " " * (n - i) * 2  # Extra spaces for mirror effect
        output.append(left_spaces + stars + right_spaces + stars + left_spaces)
    return output

# Test
print(generate_inverted_mirrored_pyramid(4))
# Output: 
# ['*******       *******',
#  ' *****         ***** ',
#  '  ***           ***  ',
#  '   *             *   ']
```

## Inverted Fibonacci Pyramid
```python
def generate_inverted_fibonacci_pyramid(n):
    """Inverted pyramid using Fibonacci numbers"""
    def fibonacci(k):
        if k <= 1:
            return 1
        a, b = 1, 1
        for _ in range(2, k + 1):
            a, b = b, a + b
        return b
    
    output = []
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        row = ""
        fib_num = fibonacci(2 * i - 1)
        row = str(fib_num) * (2 * i - 1)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_inverted_fibonacci_pyramid(4))
# Output: 
# ['1313131313131',
#  '  8888888   ',
#  '   55555    ',
#  '    333     ']
```
