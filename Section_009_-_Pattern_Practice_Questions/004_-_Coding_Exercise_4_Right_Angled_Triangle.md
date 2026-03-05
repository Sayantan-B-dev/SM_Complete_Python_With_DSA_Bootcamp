## Basic Right-Angled Triangle (Original)
```python
def generate_triangle(n):
    """Basic right-angled triangle with stars increasing from 1 to n"""
    output = []
    for i in range(1, n + 1):
        output.append("*" * i)
    return output

# Test
print(generate_triangle(5))
# Output: 
# ['*',
#  '**',
#  '***',
#  '****',
#  '*****']
```

## Inverted Right-Angled Triangle
```python
def generate_inverted_triangle(n):
    """Inverted right-angled triangle (stars decreasing)"""
    output = []
    for i in range(n, 0, -1):
        output.append("*" * i)
    return output

# Test
print(generate_inverted_triangle(5))
# Output: 
# ['*****',
#  '****',
#  '***',
#  '**',
#  '*']
```

## Right-Aligned Triangle
```python
def generate_right_aligned_triangle(n):
    """Right-angled triangle aligned to the right side"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        output.append(spaces + "*" * i)
    return output

# Test
print(generate_right_aligned_triangle(5))
# Output: 
# ['    *',
#  '   **',
#  '  ***',
#  ' ****',
#  '*****']
```

## Inverted Right-Aligned Triangle
```python
def generate_inverted_right_aligned_triangle(n):
    """Inverted right-angled triangle aligned to the right"""
    output = []
    for i in range(n, 0, -1):
        spaces = " " * (n - i)
        output.append(spaces + "*" * i)
    return output

# Test
print(generate_inverted_right_aligned_triangle(5))
# Output: 
# ['*****',
#  ' ****',
#  '  ***',
#  '   **',
#  '    *']
```

## Number Triangle
```python
def generate_number_triangle(n):
    """Right-angled triangle with row numbers"""
    output = []
    for i in range(1, n + 1):
        output.append(str(i) * i)
    return output

# Test
print(generate_number_triangle(5))
# Output: 
# ['1',
#  '22',
#  '333',
#  '4444',
#  '55555']
```

## Consecutive Numbers Triangle
```python
def generate_consecutive_numbers_triangle(n):
    """Right-angled triangle with consecutive numbers"""
    output = []
    num = 1
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str(num) + " "
            num += 1
        output.append(row.strip())
    return output

# Test
print(generate_consecutive_numbers_triangle(5))
# Output: 
# ['1',
#  '2 3',
#  '4 5 6',
#  '7 8 9 10',
#  '11 12 13 14 15']
```

## Alphabet Triangle
```python
def generate_alphabet_triangle(n):
    """Right-angled triangle with alphabets"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += chr(65 + j)  # A, B, C, ...
        output.append(row)
    return output

# Test
print(generate_alphabet_triangle(5))
# Output: 
# ['A',
#  'AB',
#  'ABC',
#  'ABCD',
#  'ABCDE']
```

## Hollow Right-Angled Triangle
```python
def generate_hollow_triangle(n):
    """Hollow right-angled triangle (only border stars)"""
    output = []
    for i in range(1, n + 1):
        if i == 1 or i == n:  # First or last row
            output.append("*" * i)
        else:  # Middle rows
            output.append("*" + " " * (i - 2) + "*")
    return output

# Test
print(generate_hollow_triangle(6))
# Output: 
# ['*',
#  '**',
#  '* *',
#  '*  *',
#  '*   *',
#  '******']
```

## Mirror Triangle (Diamond Half)
```python
def generate_mirror_triangle(n):
    """Creates a triangle and its mirror to form a pattern"""
    output = []
    # Upper triangle
    for i in range(1, n + 1):
        output.append("*" * i)
    # Lower inverted triangle
    for i in range(n - 1, 0, -1):
        output.append("*" * i)
    return output

# Test
print(generate_mirror_triangle(5))
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

## Alternating Character Triangle
```python
def generate_alternating_triangle(n):
    """Triangle with alternating characters (* and #)"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            if j % 2 == 0:
                row += "*"
            else:
                row += "#"
        output.append(row)
    return output

# Test
print(generate_alternating_triangle(6))
# Output: 
# ['*',
#  '*#',
#  '*#*',
#  '*#*#',
#  '*#*#*',
#  '*#*#*#']
```

## Binary Triangle
```python
def generate_binary_triangle(n):
    """Triangle with binary pattern (0s and 1s)"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str((i + j) % 2)
        output.append(row)
    return output

# Test
print(generate_binary_triangle(5))
# Output: 
# ['1',
#  '10',
#  '101',
#  '1010',
#  '10101']
```

## Floyd's Triangle Style
```python
def generate_floyd_style_triangle(n):
    """Triangle with numbers in Floyd's triangle pattern"""
    output = []
    num = 1
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += f"{num:2d} "  # Format with 2 spaces
            num += 1
        output.append(row.strip())
    return output

# Test
print(generate_floyd_style_triangle(5))
# Output: 
# [' 1',
#  ' 2  3',
#  ' 4  5  6',
#  ' 7  8  9 10',
#  '11 12 13 14 15']
```

## Pascal's Triangle (Simplified)
```python
def generate_pascal_triangle(n):
    """Triangle using Pascal's triangle numbers"""
    output = []
    for i in range(n):
        row = ""
        num = 1
        for j in range(i + 1):
            row += str(num) + " "
            num = num * (i - j) // (j + 1)
        output.append(row.strip())
    return output

# Test
print(generate_pascal_triangle(6))
# Output: 
# ['1',
#  '1 1',
#  '1 2 1',
#  '1 3 3 1',
#  '1 4 6 4 1',
#  '1 5 10 10 5 1']
```

## Pyramid Half (Double Sided)
```python
def generate_pyramid_half(n):
    """Creates half of a pyramid with spaces in middle"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        stars = "*" * i
        output.append(spaces + stars + " " + stars[::-1])
    return output

# Test
print(generate_pyramid_half(5))
# Output: 
# ['    * *',
#  '   ** **',
#  '  *** ***',
#  ' **** ****',
#  '***********']
```

## Custom Character Triangle
```python
def generate_custom_triangle(n, char_pattern=None):
    """Triangle with custom character pattern"""
    if char_pattern is None:
        char_pattern = ['*', '#', '@', '%', '&']
    
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += char_pattern[j % len(char_pattern)]
        output.append(row)
    return output

# Test
print(generate_custom_triangle(7, ['★', '♠', '♥', '♦']))
# Output: 
# ['★',
#  '★♠',
#  '★♠♥',
#  '★♠♥♦',
#  '★♠♥♦★',
#  '★♠♥♦★♠',
#  '★♠♥♦★♠♥']
```
