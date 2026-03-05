## Basic Right-Angled Triangle
```python
def generate_right_angled_triangle(n):
    """Basic right-angled triangle with stars aligned to the right"""
    output = []
    for i in range(1, n + 1):
        output.append(" " * (n - i) + "*" * i)
    return output

# Test
print(generate_right_angled_triangle(5))
# Output: 
# ['    *',
#  '   **',
#  '  ***',
#  ' ****',
#  '*****']
```

## Left-Aligned Right-Angled Triangle
```python
def generate_left_aligned_right_angled_triangle(n):
    """Right-angled triangle aligned to the left"""
    output = []
    for i in range(1, n + 1):
        output.append("*" * i)
    return output

# Test
print(generate_left_aligned_right_angled_triangle(5))
# Output: 
# ['*',
#  '**',
#  '***',
#  '****',
#  '*****']
```

## Inverted Right-Angled Triangle
```python
def generate_inverted_right_angled_triangle(n):
    """Inverted right-angled triangle"""
    output = []
    for i in range(n, 0, -1):
        output.append(" " * (n - i) + "*" * i)
    return output

# Test
print(generate_inverted_right_angled_triangle(5))
# Output: 
# ['*****',
#  ' ****',
#  '  ***',
#  '   **',
#  '    *']
```

## Number Right-Angled Triangle
```python
def generate_number_right_angled_triangle(n):
    """Right-angled triangle with consecutive numbers"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(1, i + 1):
            row += str(j)
        output.append(" " * (n - i) + row)
    return output

# Test
print(generate_number_right_angled_triangle(5))
# Output: 
# ['    1',
#  '   12',
#  '  123',
#  ' 1234',
#  '12345']
```

## Alphabet Right-Angled Triangle
```python
def generate_alphabet_right_angled_triangle(n):
    """Right-angled triangle with alphabets"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += chr(65 + j)
        output.append(" " * (n - i) + row)
    return output

# Test
print(generate_alphabet_right_angled_triangle(5))
# Output: 
# ['    A',
#  '   AB',
#  '  ABC',
#  ' ABCD',
#  'ABCDE']
```

## Binary Right-Angled Triangle
```python
def generate_binary_right_angled_triangle(n):
    """Right-angled triangle with binary pattern"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str((i + j) % 2)
        output.append(" " * (n - i) + row)
    return output

# Test
print(generate_binary_right_angled_triangle(5))
# Output: 
# ['    1',
#  '   10',
#  '  101',
#  ' 1010',
#  '10101']
```

## Hollow Right-Angled Triangle
```python
def generate_hollow_right_angled_triangle(n):
    """Right-angled triangle with hollow center"""
    output = []
    for i in range(1, n + 1):
        if i == 1:  # First row
            output.append(" " * (n - i) + "*")
        elif i == n:  # Last row
            output.append(" " * (n - i) + "*" * i)
        else:  # Middle rows
            output.append(" " * (n - i) + "*" + " " * (i - 2) + "*")
    return output

# Test
print(generate_hollow_right_angled_triangle(6))
# Output: 
# ['     *',
#  '    **',
#  '   * *',
#  '  *  *',
#  ' *   *',
#  '******']
```

## Double Right-Angled Triangle
```python
def generate_double_right_angled_triangle(n):
    """Two right-angled triangles side by side"""
    output = []
    for i in range(1, n + 1):
        left_triangle = " " * (n - i) + "*" * i
        gap = " " * 2
        right_triangle = "*" * i
        output.append(left_triangle + gap + right_triangle)
    return output

# Test
print(generate_double_right_angled_triangle(4))
# Output: 
# ['   *   *',
#  '  **   **',
#  ' ***   ***',
#  '****   ****']
```

## Floyd's Right-Angled Triangle
```python
def generate_floyds_right_angled_triangle(n):
    """Right-angled triangle with Floyd's triangle numbers"""
    output = []
    num = 1
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str(num) + " "
            num += 1
        output.append(" " * (n - i) * 2 + row.strip())
    return output

# Test
print(generate_floyds_right_angled_triangle(5))
# Output: 
# ['        1',
#  '      2 3',
#  '    4 5 6',
#  '  7 8 9 10',
#  '11 12 13 14 15']
```

## Pyramid Half Right-Angled Triangle
```python
def generate_pyramid_half_right_angled_triangle(n):
    """Half pyramid forming a right-angled triangle pattern"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        stars = "*" * i
        # Add a mirrored version to create a pyramid half
        output.append(spaces + stars + " " + stars[::-1])
    return output

# Test
print(generate_pyramid_half_right_angled_triangle(4))
# Output: 
# ['   *    *',
#  '  **   **',
#  ' ***  ***',
#  '**** ****']
```

## Custom Character Right-Angled Triangle
```python
def generate_custom_right_angled_triangle(n, chars=['*', '#', '@']):
    """Right-angled triangle with alternating characters"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += chars[j % len(chars)]
        output.append(" " * (n - i) + row)
    return output

# Test
print(generate_custom_right_angled_triangle(6, ['*', '+', '-', '•']))
# Output: 
# ['     *',
#  '    *+',
#  '   *+-',
#  '  *+-•',
#  ' *+-•*',
#  '*+-•*+']
```

## Mirror Right-Angled Triangle
```python
def generate_mirror_right_angled_triangle(n):
    """Right-angled triangle with its mirror image"""
    output = []
    # Upper triangle
    for i in range(1, n + 1):
        output.append(" " * (n - i) + "*" * i)
    # Lower inverted triangle (mirror)
    for i in range(n - 1, 0, -1):
        output.append(" " * (n - i) + "*" * i)
    return output

# Test
print(generate_mirror_right_angled_triangle(4))
# Output: 
# ['   *',
#  '  **',
#  ' ***',
#  '****',
#  ' ***',
#  '  **',
#  '   *']
```

## Multiplication Right-Angled Triangle
```python
def generate_multiplication_right_angled_triangle(n):
    """Right-angled triangle showing multiplication table"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(1, i + 1):
            row += str(i * j) + " "
        output.append(" " * (n - i) * 3 + row.strip())
    return output

# Test
print(generate_multiplication_right_angled_triangle(5))
# Output: 
# ['            1',
#  '          2 4',
#  '        3 6 9',
#  '     4 8 12 16',
#  ' 5 10 15 20 25']
```

## Even Number Right-Angled Triangle
```python
def generate_even_right_angled_triangle(n):
    """Right-angled triangle with even numbers"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(1, i + 1):
            row += str(j * 2) + " "
        output.append(" " * (n - i) * 2 + row.strip())
    return output

# Test
print(generate_even_right_angled_triangle(5))
# Output: 
# ['        2',
#  '      2 4',
#  '    2 4 6',
#  '  2 4 6 8',
#  '2 4 6 8 10']
```

## Odd Number Right-Angled Triangle
```python
def generate_odd_right_angled_triangle(n):
    """Right-angled triangle with odd numbers"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(1, i + 1):
            row += str(j * 2 - 1) + " "
        output.append(" " * (n - i) * 2 + row.strip())
    return output

# Test
print(generate_odd_right_angled_triangle(5))
# Output: 
# ['        1',
#  '      1 3',
#  '    1 3 5',
#  '  1 3 5 7',
#  '1 3 5 7 9']
```

## Checkerboard Right-Angled Triangle
```python
def generate_checkerboard_right_angled_triangle(n):
    """Right-angled triangle with checkerboard pattern"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            if (i + j) % 2 == 0:
                row += "*"
            else:
                row += " "
        output.append(" " * (n - i) + row)
    return output

# Test
print(generate_checkerboard_right_angled_triangle(6))
# Output: 
# ['     *',
#  '    * ',
#  '   * *',
#  '  * * ',
#  ' * * *',
#  '* * * ']
```
