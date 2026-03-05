## Basic Pyramid
```python
def generate_pyramid(n):
    """Basic pyramid with centered stars"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    return output

# Test
print(generate_pyramid(5))
# Output: 
# ['    *    ',
#  '   ***   ',
#  '  *****  ',
#  ' ******* ',
#  '*********']
```

## Inverted Pyramid
```python
def generate_inverted_pyramid(n):
    """Inverted pyramid (upside down)"""
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

## Hollow Pyramid
```python
def generate_hollow_pyramid(n):
    """Pyramid with hollow center (only border)"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        if i == 1:  # Top row (single star)
            output.append(spaces + "*" + spaces)
        elif i == n:  # Bottom row (full stars)
            output.append(spaces + "*" * (2 * i - 1) + spaces)
        else:  # Middle rows (borders only)
            hollow = "*" + " " * (2 * i - 3) + "*"
            output.append(spaces + hollow + spaces)
    return output

# Test
print(generate_hollow_pyramid(6))
# Output: 
# ['     *     ',
#  '    * *    ',
#  '   *   *   ',
#  '  *     *  ',
#  ' *       * ',
#  '***********']
```

## Number Pyramid
```python
def generate_number_pyramid(n):
    """Pyramid with increasing numbers"""
    output = []
    for i in range(1, n + 1):
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
print(generate_number_pyramid(5))
# Output: 
# ['    1    ',
#  '   121   ',
#  '  12321  ',
#  ' 1234321 ',
#  '123454321']
```

## Alphabet Pyramid
```python
def generate_alphabet_pyramid(n):
    """Pyramid with alphabets"""
    output = []
    for i in range(1, n + 1):
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
print(generate_alphabet_pyramid(5))
# Output: 
# ['    A    ',
#  '   ABA   ',
#  '  ABCBA  ',
#  ' ABCDCBA ',
#  'ABCDEDCBA']
```

## Diamond Pyramid (Full Diamond)
```python
def generate_diamond_pyramid(n):
    """Complete diamond shape (pyramid + inverted pyramid)"""
    output = []
    # Upper pyramid
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    # Lower inverted pyramid
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    return output

# Test
print(generate_diamond_pyramid(4))
# Output: 
# ['   *   ',
#  '  ***  ',
#  ' ***** ',
#  '*******',
#  ' ***** ',
#  '  ***  ',
#  '   *   ']
```

## Binary Pyramid
```python
def generate_binary_pyramid(n):
    """Pyramid with binary pattern"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            row += str((i + j) % 2)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_binary_pyramid(5))
# Output: 
# ['    1    ',
#  '   101   ',
#  '  10101  ',
#  ' 1010101 ',
#  '101010101']
```

## Double Pyramid (Side by Side)
```python
def generate_double_pyramid(n):
    """Two pyramids side by side"""
    output = []
    for i in range(1, n + 1):
        spaces1 = " " * (n - i)
        stars1 = "*" * (2 * i - 1)
        spaces2 = " " * 3  # Gap between pyramids
        spaces3 = " " * (n - i)
        stars2 = "*" * (2 * i - 1)
        output.append(spaces1 + stars1 + spaces2 + stars2 + spaces3)
    return output

# Test
print(generate_double_pyramid(4))
# Output: 
# ['   *      *   ',
#  '  ***    ***  ',
#  ' *****  ***** ',
#  '***************']
```

## Rotated Pyramid (Right Side)
```python
def generate_rotated_pyramid(n):
    """Pyramid rotated 90 degrees to the right"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        output.append("*" * i)
    # Lower half
    for i in range(n - 1, 0, -1):
        output.append("*" * i)
    return output

# Test
print(generate_rotated_pyramid(5))
# Output: 
# ['*',
#  '**',
#  '***',
#  '****',
#  '*****',
#  '****',
#  '***',
#  '**',
#  '*']
```

## Custom Character Pyramid
```python
def generate_custom_pyramid(n, chars=['*', '#', '@']):
    """Pyramid with alternating character layers"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        char = chars[(i - 1) % len(chars)]
        stars = char * (2 * i - 1)
        output.append(spaces + stars + spaces)
    return output

# Test
print(generate_custom_pyramid(6, ['*', '+', '•']))
# Output: 
# ['     *     ',
#  '    +++    ',
#  '   •••••   ',
#  '  *******  ',
#  ' +++++++++ ',
#  '•••••••••••']
```

## Pascal's Triangle Pyramid
```python
def generate_pascal_pyramid(n):
    """Pyramid using Pascal's triangle numbers"""
    output = []
    for i in range(n):
        spaces = " " * (n - i - 1) * 2
        row = ""
        num = 1
        for j in range(i + 1):
            row += str(num).center(4)
            num = num * (i - j) // (j + 1)
        output.append(spaces + row)
    return output

# Test
print(generate_pascal_pyramid(5))
# Output: 
# ['        1   ',
#  '      1   1  ',
#  '    1   2   1 ',
#  '  1   3   3   1',
#  '1   4   6   4   1']
```

## Hourglass Pyramid
```python
def generate_hourglass_pyramid(n):
    """Hourglass shape (inverted pyramid + pyramid)"""
    output = []
    # Top inverted pyramid
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    # Bottom pyramid (excluding the peak to avoid duplication)
    for i in range(2, n + 1):
        spaces = " " * (n - i)
        stars = "*" * (2 * i - 1)
        output.append(spaces + stars + spaces)
    return output

# Test
print(generate_hourglass_pyramid(4))
# Output: 
# ['*******',
#  ' ***** ',
#  '  ***  ',
#  '   *   ',
#  '  ***  ',
#  ' ***** ',
#  '*******']
```

## Checkerboard Pyramid
```python
def generate_checkerboard_pyramid(n):
    """Pyramid with checkerboard pattern"""
    output = []
    for i in range(1, n + 1):
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
print(generate_checkerboard_pyramid(5))
# Output: 
# ['    *    ',
#  '   * *   ',
#  '  * * *  ',
#  ' * * * * ',
#  '* * * * *']
```
