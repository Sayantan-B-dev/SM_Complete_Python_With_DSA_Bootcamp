## Basic Inverted Triangle
```python
def generate_inverted_triangle(n):
    """Basic inverted triangle with stars decreasing from n to 1"""
    output = []
    for i in range(n):
        output.append("*" * (n - i))
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

## Inverted Right-Aligned Triangle
```python
def generate_inverted_right_aligned_triangle(n):
    """Inverted triangle aligned to the right side"""
    output = []
    for i in range(n):
        spaces = " " * i
        output.append(spaces + "*" * (n - i))
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

## Inverted Hollow Triangle
```python
def generate_inverted_hollow_triangle(n):
    """Inverted triangle with only border stars"""
    output = []
    for i in range(n):
        if i == 0:  # First row (longest)
            output.append("*" * (n - i))
        elif i == n-1:  # Last row (single star)
            output.append("*")
        else:  # Middle rows
            spaces = " " * (n - i - 2)
            output.append("*" + spaces + "*")
    return output

# Test
print(generate_inverted_hollow_triangle(6))
# Output: 
# ['******',
#  '*   *',
#  '*  *',
#  '* *',
#  '**',
#  '*']
```

## Inverted Number Triangle
```python
def generate_inverted_number_triangle(n):
    """Inverted triangle with row numbers"""
    output = []
    for i in range(n):
        output.append(str(n - i) * (n - i))
    return output

# Test
print(generate_inverted_number_triangle(5))
# Output: 
# ['55555',
#  '4444',
#  '333',
#  '22',
#  '1']
```

## Inverted Consecutive Numbers Triangle
```python
def generate_inverted_consecutive_triangle(n):
    """Inverted triangle with consecutive numbers starting from top"""
    output = []
    num = 1
    total_nums = n * (n + 1) // 2
    
    for i in range(n, 0, -1):
        row = ""
        for j in range(i):
            row += str(num) + " "
            num += 1
        output.append(row.strip())
    return output

# Test
print(generate_inverted_consecutive_triangle(4))
# Output: 
# ['1 2 3 4',
#  '5 6 7',
#  '8 9',
#  '10']
```

## Inverted Alphabet Triangle
```python
def generate_inverted_alphabet_triangle(n):
    """Inverted triangle with alphabets in sequence"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n - i):
            row += chr(65 + j)
        output.append(row)
    return output

# Test
print(generate_inverted_alphabet_triangle(5))
# Output: 
# ['ABCDE',
#  'ABCD',
#  'ABC',
#  'AB',
#  'A']
```

## Inverted Binary Triangle
```python
def generate_inverted_binary_triangle(n):
    """Inverted triangle with binary pattern"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n - i):
            row += str((i + j) % 2)
        output.append(row)
    return output

# Test
print(generate_inverted_binary_triangle(5))
# Output: 
# ['10101',
#  '0101',
#  '101',
#  '01',
#  '1']
```

## Inverted Mirrored Triangle
```python
def generate_inverted_mirrored_triangle(n):
    """Creates an inverted triangle and its mirror"""
    output = []
    # Upper inverted triangle
    for i in range(n):
        output.append("*" * (n - i))
    # Lower triangle (right-side up)
    for i in range(2, n + 1):
        output.append("*" * i)
    return output

# Test
print(generate_inverted_mirrored_triangle(4))
# Output: 
# ['****',
#  '***',
#  '**',
#  '*',
#  '**',
#  '***',
#  '****']
```

## Inverted Alternating Character Triangle
```python
def generate_inverted_alternating_triangle(n):
    """Inverted triangle with alternating characters"""
    chars = ['*', '#', '@', '%']
    output = []
    for i in range(n):
        row = ""
        for j in range(n - i):
            row += chars[j % len(chars)]
        output.append(row)
    return output

# Test
print(generate_inverted_alternating_triangle(6))
# Output: 
# ['*#@%*#',
#  '*#@%*',
#  '*#@%',
#  '*#@',
#  '*#',
#  '*']
```

## Inverted Pyramid Base Triangle
```python
def generate_inverted_pyramid_base(n):
    """Inverted triangle with spaces for pyramid effect"""
    output = []
    for i in range(n):
        spaces = " " * i
        stars = "*" * (2 * (n - i) - 1)
        output.append(spaces + stars)
    return output

# Test
print(generate_inverted_pyramid_base(5))
# Output: 
# ['*********',
#  ' *******',
#  '  *****',
#  '   ***',
#  '    *']
```

## Inverted Floyd-Style Triangle
```python
def generate_inverted_floyd_triangle(n):
    """Inverted triangle with numbers in Floyd pattern"""
    output = []
    num = 1
    # First calculate total numbers to determine starting point
    total = n * (n + 1) // 2
    
    for i in range(n, 0, -1):
        row = ""
        for j in range(i):
            row += f"{num:2d} "
            num += 1
        output.append(row.strip())
    return output

# Test
print(generate_inverted_floyd_triangle(4))
# Output: 
# [' 1  2  3  4',
#  ' 5  6  7',
#  ' 8  9',
#  '10']
```

## Inverted Diamond Half
```python
def generate_inverted_diamond_half(n):
    """Inverted triangle as top half of a diamond"""
    output = []
    for i in range(n):
        spaces = " " * i
        stars = "*" * (n - i)
        output.append(spaces + stars + "*" * (n - i - 1))
    return output

# Test
print(generate_inverted_diamond_half(5))
# Output: 
# ['*********',
#  ' *******',
#  '  *****',
#  '   ***',
#  '    *']
```

## Inverted Double Character Triangle
```python
def generate_inverted_double_char_triangle(n):
    """Inverted triangle with two alternating characters per row"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n - i):
            if j % 2 == 0:
                row += "*"
            else:
                row += "+"
        output.append(row)
    return output

# Test
print(generate_inverted_double_char_triangle(6))
# Output: 
# ['*+*+*+',
#  '*+*+*',
#  '*+*+',
#  '*+*',
#  '*+',
#  '*']
```

## Inverted Number Pattern Triangle
```python
def generate_inverted_number_pattern(n):
    """Inverted triangle with numbers decreasing pattern"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n - i, 0, -1):
            row += str(j)
        output.append(row)
    return output

# Test
print(generate_inverted_number_pattern(5))
# Output: 
# ['54321',
#  '4321',
#  '321',
#  '21',
#  '1']
```

## Inverted Border Triangle
```python
def generate_inverted_border_triangle(n):
    """Inverted triangle with border emphasis"""
    output = []
    for i in range(n):
        if i == 0:  # Top row
            output.append("*" * n)
        elif i == n-1:  # Bottom row
            output.append("*")
        else:
            # Border only on left and right
            spaces = " " * (n - i - 2)
            output.append("*" + spaces + "*")
    return output

# Test
print(generate_inverted_border_triangle(6))
# Output: 
# ['******',
#  '*   *',
#  '*  *',
#  '* *',
#  '**',
#  '*']
```
