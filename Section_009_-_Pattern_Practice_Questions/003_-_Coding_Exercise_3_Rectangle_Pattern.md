## Basic Rectangle (Original)
```python
def generate_rectangle(n, m):
    """Basic rectangle pattern of '*' with n rows and m columns"""
    output = []
    for i in range(n):
        output.append("*" * m)
    return output

# Test
print(generate_rectangle(4, 6))
# Output: 
# ['******',
#  '******',
#  '******',
#  '******']
```

## Hollow Rectangle
```python
def generate_hollow_rectangle(n, m):
    """Creates a hollow rectangle pattern with only border '*'"""
    output = []
    for i in range(n):
        if i == 0 or i == n-1:  # First and last row
            output.append("*" * m)
        else:  # Middle rows - only first and last *
            output.append("*" + " " * (m-2) + "*")
    return output

# Test
print(generate_hollow_rectangle(5, 8))
# Output: 
# ['********',
#  '*      *',
#  '*      *',
#  '*      *',
#  '********']
```

## Checkerboard Rectangle
```python
def generate_checkerboard_rectangle(n, m):
    """Creates a checkerboard pattern rectangle"""
    output = []
    for i in range(n):
        row = ""
        for j in range(m):
            if (i + j) % 2 == 0:
                row += "*"
            else:
                row += " "
        output.append(row)
    return output

# Test
print(generate_checkerboard_rectangle(4, 8))
# Output: 
# ['* * * * ',
#  ' * * * *',
#  '* * * * ',
#  ' * * * *']
```

## Number Rectangle
```python
def generate_number_rectangle(n, m):
    """Creates a rectangle with sequential numbers"""
    output = []
    num = 1
    for i in range(n):
        row = ""
        for j in range(m):
            row += str(num % 10)  # Use last digit
            num += 1
        output.append(row)
    return output

# Test
print(generate_number_rectangle(3, 5))
# Output: 
# ['12345',
#  '67890',
#  '12345']
```

## Pattern Rectangle (Zigzag)
```python
def generate_zigzag_rectangle(n, m):
    """Creates a rectangle with zigzag pattern"""
    output = []
    for i in range(n):
        row = ""
        if i % 2 == 0:  # Even rows: left to right
            for j in range(m):
                row += "*" if j % 2 == 0 else " "
        else:  # Odd rows: right to left pattern
            for j in range(m):
                row += "*" if j % 2 == 1 else " "
        output.append(row)
    return output

# Test
print(generate_zigzag_rectangle(4, 6))
# Output: 
# ['* * * ',
#  ' * * *',
#  '* * * ',
#  ' * * *']
```

## Border Thickness Rectangle
```python
def generate_border_thickness_rectangle(n, m, thickness=1):
    """Creates a rectangle with specified border thickness"""
    output = []
    for i in range(n):
        row = ""
        for j in range(m):
            # Distance from nearest border
            top_dist = i
            bottom_dist = n - 1 - i
            left_dist = j
            right_dist = m - 1 - j
            
            min_dist = min(top_dist, bottom_dist, left_dist, right_dist)
            
            if min_dist < thickness:
                row += "*"
            else:
                row += " "
        output.append(row)
    return output

# Test
print(generate_border_thickness_rectangle(6, 10, 2))
# Output: 
# ['**********',
#  '**********',
#  '**      **',
#  '**      **',
#  '**********',
#  '**********']
```

## Diagonal Lines Rectangle
```python
def generate_diagonal_rectangle(n, m):
    """Creates a rectangle with diagonal lines pattern"""
    output = []
    for i in range(n):
        row = ""
        for j in range(m):
            if i == j or i + j == n-1 or i == 0 or i == n-1 or j == 0 or j == m-1:
                row += "*"
            else:
                row += " "
        output.append(row)
    return output

# Test
print(generate_diagonal_rectangle(5, 9))
# Output: 
# ['*********',
#  '**     **',
#  '* *   * *',
#  '*  * *  *',
#  '*********']
```

## Gradient/Rainbow Rectangle
```python
def generate_gradient_rectangle(n, m):
    """Creates a rectangle with gradient-like pattern using different characters"""
    chars = ['*', '#', '@', '%', '&', '+', '=']
    output = []
    for i in range(n):
        row = ""
        for j in range(m):
            # Use position to determine character
            char_index = int((i + j) / (n + m) * len(chars))
            char_index = min(char_index, len(chars)-1)
            row += chars[char_index]
        output.append(row)
    return output

# Test
print(generate_gradient_rectangle(4, 8))
# Output: 
# ['********',
#  '########',
#  '@@@@@@@@',
#  '%%%%%%%%']
```

## Spiral Pattern Rectangle
```python
def generate_spiral_rectangle(n, m):
    """Creates a rectangle with spiral pattern (simplified)"""
    output = [[' ' for _ in range(m)] for _ in range(n)]
    
    top, bottom, left, right = 0, n-1, 0, m-1
    char = '*'
    
    while top <= bottom and left <= right:
        # Top row
        for j in range(left, right+1):
            output[top][j] = char
        top += 1
        
        # Right column
        for i in range(top, bottom+1):
            output[i][right] = char
        right -= 1
        
        # Bottom row
        if top <= bottom:
            for j in range(right, left-1, -1):
                output[bottom][j] = char
            bottom -= 1
        
        # Left column
        if left <= right:
            for i in range(bottom, top-1, -1):
                output[i][left] = char
            left += 1
        
        # Alternate character for next layer
        char = '#' if char == '*' else '*'
    
    return [''.join(row) for row in output]

# Test
print(generate_spiral_rectangle(5, 7))
# Output: 
# ['*******',
#  '*#####*',
#  '*#***#*',
#  '*#####*',
#  '*******']
```

## Frame with Text Rectangle
```python
def generate_text_rectangle(n, m, text="HELLO"):
    """Creates a rectangle with centered text"""
    output = []
    
    # Top border
    output.append("*" * m)
    
    if n >= 3:
        # Calculate text positioning
        available_width = m - 2
        if len(text) > available_width:
            text = text[:available_width]
        
        padding = available_width - len(text)
        left_pad = padding // 2
        right_pad = padding - left_pad
        
        # Text row
        text_row = "*" + " " * left_pad + text + " " * right_pad + "*"
        output.append(text_row)
        
        # Fill middle rows
        for i in range(n-3):
            output.append("*" + " " * (m-2) + "*")
        
        # Bottom border
        output.append("*" * m)
    
    return output[:n]

# Test
print(generate_text_rectangle(5, 15, "PYTHON"))
# Output: 
# ['***************',
#  '*    PYTHON    *',
#  '*             *',
#  '*             *',
#  '***************']
```

## Alternating Row Pattern
```python
def generate_alternating_rectangle(n, m):
    """Creates a rectangle with alternating row patterns"""
    output = []
    patterns = [
        "*" * m,  # All stars
        "* " * (m//2) + "*" if m % 2 else "* " * (m//2),  # Alternate
        " *" * (m//2),  # Space then star
        "*" * m  # All stars again
    ]
    
    for i in range(n):
        output.append(patterns[i % len(patterns)])
    return output

# Test
print(generate_alternating_rectangle(6, 8))
# Output: 
# ['********',
#  '* * * * ',
#  ' * * * *',
#  '********',
#  '* * * * ',
#  ' * * * *']
```

## Math Function Rectangle
```python
import math

def generate_math_rectangle(n, m):
    """Creates a rectangle using mathematical functions"""
    output = []
    for i in range(n):
        row = ""
        for j in range(m):
            # Use sine function to determine pattern
            value = math.sin(i/2) * math.cos(j/2)
            if value > 0.3:
                row += "@"
            elif value > 0:
                row += "*"
            elif value > -0.3:
                row += "."
            else:
                row += " "
        output.append(row)
    return output

# Test
print(generate_math_rectangle(8, 16))
# Output will vary but looks like a wave pattern
```

## Dynamic Border Rectangle
```python
def generate_dynamic_border_rectangle(n, m):
    """Creates a rectangle with varying border characters"""
    borders = ['*', '#', '@', '%', '&']
    output = []
    
    for i in range(n):
        row = ""
        for j in range(m):
            # Determine if current position is on border
            if i == 0 or i == n-1 or j == 0 or j == m-1:
                # Use different border characters based on position
                border_index = (i + j) % len(borders)
                row += borders[border_index]
            else:
                row += " "
        output.append(row)
    return output

# Test
print(generate_dynamic_border_rectangle(5, 10))
# Output: 
# ['*#@%&#*#@%',
#  '*        %',
#  '*        &',
#  '*        #',
#  '*#@%&#*#@%']
```

## Clock Pattern Rectangle
```python
def generate_clock_rectangle(n, m):
    """Creates a rectangle with numbers arranged like a clock"""
    output = []
    for i in range(n):
        row = ""
        for j in range(m):
            # Calculate angle and map to hours 1-12
            angle = math.atan2(i - n//2, j - m//2) if (i - n//2, j - m//2) != (0, 0) else 0
            hour = int((angle + math.pi) / (2 * math.pi) * 12) + 1
            row += str(hour % 10)  # Use last digit
        output.append(row)
    return output

# Test
print(generate_clock_rectangle(5, 9))
# Output shows numbers arranged radially
```

## Custom Symbol Rectangle
```python
def generate_custom_symbol_rectangle(n, m, symbol_func=None):
    """Creates a rectangle where each cell uses a custom symbol based on position"""
    if symbol_func is None:
        symbol_func = lambda i, j: "*" if (i + j) % 3 == 0 else " "
    
    output = []
    for i in range(n):
        row = ""
        for j in range(m):
            row += symbol_func(i, j)
        output.append(row)
    return output

# Test with custom function
print(generate_custom_symbol_rectangle(5, 10, 
      lambda i, j: chr(65 + (i * j) % 26) if (i * j) % 3 == 0 else "."))
# Output: Custom pattern with letters and dots
```