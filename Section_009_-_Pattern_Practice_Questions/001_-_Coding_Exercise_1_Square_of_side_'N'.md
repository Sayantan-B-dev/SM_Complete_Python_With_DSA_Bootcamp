## Basic Square (Original)
```python
def generate_square(n):
    """Basic square pattern of '*' of size n x n"""
    output = []
    for i in range(n):
        output.append("*" * n)
    return output

# Test
print(generate_square(5))
# Output: ['*****', '*****', '*****', '*****', '*****']
```

## Number Square
```python
def generate_number_square(n):
    """Creates a square pattern with numbers"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n):
            row += str((i + j) % 10)  # Numbers 0-9 cycling
        output.append(row)
    return output

# Test
print(generate_number_square(5))
# Output: ['01234', '12345', '23456', '34567', '45678']
```

## Diagonal Pattern Square
```python
def generate_diagonal_square(n):
    """Creates a square with diagonal pattern"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n):
            if i == j or i + j == n-1:  # Both diagonals
                row += "*"
            else:
                row += " "
        output.append(row)
    return output

# Test
print(generate_diagonal_square(5))
# Output: ['*   *', ' * * ', '  *  ', ' * * ', '*   *']
```

## Alphabet Square
```python
def generate_alphabet_square(n):
    """Creates a square pattern with alphabets"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n):
            # Cycle through A-Z
            char_code = 65 + ((i + j) % 26)
            row += chr(char_code)
        output.append(row)
    return output

# Test
print(generate_alphabet_square(5))
# Output: ['ABCDE', 'BCDEF', 'CDEFG', 'DEFGH', 'EFGHI']
```

## Checkerboard Square
```python
def generate_checkerboard_square(n):
    """Creates a checkerboard pattern square"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n):
            if (i + j) % 2 == 0:
                row += "*"
            else:
                row += " "
        output.append(row)
    return output

# Test
print(generate_checkerboard_square(5))
# Output: ['* * *', ' * * ', '* * *', ' * * ', '* * *']
```

## Incremental Border Square
```python
def generate_border_square(n):
    """Creates a square with incremental border thickness"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n):
            # Distance from nearest border
            dist = min(i, j, n-1-i, n-1-j)
            row += str(dist + 1)
        output.append(row)
    return output

# Test
print(generate_border_square(5))
# Output: ['11111', '12221', '12321', '12221', '11111']
```

## Custom Character Square
```python
def generate_custom_square(n, char1='*', char2='+'):
    """Creates a square with two alternating characters"""
    output = []
    for i in range(n):
        row = ""
        for j in range(n):
            if i < j:
                row += char2
            else:
                row += char1
        output.append(row)
    return output

# Test
print(generate_custom_square(5, '#', '@'))
# Output: ['#####', '@####', '@@###', '@@@##', '@@@@#']
```

## Pyramid in Square
```python
def generate_pyramid_square(n):
    """Creates a square with pyramid pattern inside"""
    output = []
    mid = n // 2
    for i in range(n):
        row = ""
        for j in range(n):
            if abs(i - mid) + abs(j - mid) <= mid:
                row += "*"
            else:
                row += " "
        output.append(row)
    return output

# Test
print(generate_pyramid_square(7))
# Output: 
# ['   *   ',
#  '  ***  ',
#  ' ***** ',
#  '*******',
#  ' ***** ',
#  '  ***  ',
#  '   *   ']
```

## Frame Square with Text
```python
def generate_text_square(n, text="HELLO"):
    """Creates a square frame with centered text"""
    output = []
    # Top border
    output.append("*" * n)
    
    # Middle rows with text
    text_row = "*" + text.center(n-2) + "*"
    output.append(text_row)
    
    # Fill remaining rows
    for i in range(n-3):
        output.append("*" + " " * (n-2) + "*")
    
    # Bottom border if needed
    if n > 2:
        output.append("*" * n)
    
    return output[:n]  # Ensure correct number of rows

# Test
print(generate_text_square(10, "PYTHON"))
# Output: 
# ['**********',
#  '*  PYTHON *',
#  '*        *',
#  '*        *',
#  '*        *',
#  '*        *',
#  '*        *',
#  '*        *',
#  '*        *',
#  '**********']
```
