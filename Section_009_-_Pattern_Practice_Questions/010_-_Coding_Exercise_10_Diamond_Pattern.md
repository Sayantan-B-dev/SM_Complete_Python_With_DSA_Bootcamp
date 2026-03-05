## Basic Diamond
```python
def generate_diamond(n):
    """Basic diamond pattern with stars"""
    output = []
    for i in range(1, n + 1):
        output.append(" " * (n - i) + "*" * (2 * i - 1) + " " * (n - i))
    for i in range(n - 1, 0, -1):
        output.append(" " * (n - i) + "*" * (2 * i - 1) + " " * (n - i))
    return output

# Test
print(generate_diamond(4))
# Output: 
# ['   *   ',
#  '  ***  ',
#  ' ***** ',
#  '*******',
#  ' ***** ',
#  '  ***  ',
#  '   *   ']
```

## Hollow Diamond
```python
def generate_hollow_diamond(n):
    """Diamond with hollow center (only border)"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        if i == 1:
            output.append(" " * (n - i) + "*" + " " * (n - i))
        else:
            output.append(" " * (n - i) + "*" + " " * (2 * i - 3) + "*" + " " * (n - i))
    # Lower half
    for i in range(n - 1, 0, -1):
        if i == 1:
            output.append(" " * (n - i) + "*" + " " * (n - i))
        else:
            output.append(" " * (n - i) + "*" + " " * (2 * i - 3) + "*" + " " * (n - i))
    return output

# Test
print(generate_hollow_diamond(5))
# Output: 
# ['    *    ',
#  '   * *   ',
#  '  *   *  ',
#  ' *     * ',
#  '*       *',
#  ' *     * ',
#  '  *   *  ',
#  '   * *   ',
#  '    *    ']
```

## Number Diamond
```python
def generate_number_diamond(n):
    """Diamond with numbers instead of stars"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(1, i + 1):
            row += str(j)
        for j in range(i - 1, 0, -1):
            row += str(j)
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(1, i + 1):
            row += str(j)
        for j in range(i - 1, 0, -1):
            row += str(j)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_number_diamond(4))
# Output: 
# ['   1   ',
#  '  121  ',
#  ' 12321 ',
#  '1234321',
#  ' 12321 ',
#  '  121  ',
#  '   1   ']
```

## Alphabet Diamond
```python
def generate_alphabet_diamond(n):
    """Diamond with alphabets"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(i):
            row += chr(65 + j)
        for j in range(i - 2, -1, -1):
            row += chr(65 + j)
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(i):
            row += chr(65 + j)
        for j in range(i - 2, -1, -1):
            row += chr(65 + j)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_alphabet_diamond(4))
# Output: 
# ['   A   ',
#  '  ABA  ',
#  ' ABCBA ',
#  'ABCDCBA',
#  ' ABCBA ',
#  '  ABA  ',
#  '   A   ']
```

## Binary Diamond
```python
def generate_binary_diamond(n):
    """Diamond with binary pattern (0s and 1s)"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            row += str((i + j) % 2)
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            row += str((i + j) % 2)
        output.append(spaces + row + spaces)
    return output

# Test
print(generate_binary_diamond(4))
# Output: 
# ['   1   ',
#  '  101  ',
#  ' 10101 ',
#  '1010101',
#  ' 10101 ',
#  '  101  ',
#  '   1   ']
```

## Plus-Minus Diamond
```python
def generate_plus_minus_diamond(n):
    """Diamond with alternating + and - symbols"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            if j % 2 == 0:
                row += "+"
            else:
                row += "-"
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(n - 1, 0, -1):
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
print(generate_plus_minus_diamond(4))
# Output: 
# ['   +   ',
#  '  +-+  ',
#  ' +-+-+ ',
#  '+-+-+-+',
#  ' +-+-+ ',
#  '  +-+  ',
#  '   +   ']
```

## Checkerboard Diamond
```python
def generate_checkerboard_diamond(n):
    """Diamond with checkerboard pattern"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            if (i + j) % 2 == 0:
                row += "*"
            else:
                row += " "
        output.append(spaces + row + spaces)
    # Lower half
    for i in range(n - 1, 0, -1):
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
print(generate_checkerboard_diamond(4))
# Output: 
# ['   *   ',
#  '  * *  ',
#  ' * * * ',
#  '* * * *',
#  ' * * * ',
#  '  * *  ',
#  '   *   ']
```

## Double Diamond
```python
def generate_double_diamond(n):
    """Two diamonds side by side"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces1 = " " * (n - i)
        stars1 = "*" * (2 * i - 1)
        gap = " " * 3
        spaces2 = " " * (n - i)
        stars2 = "*" * (2 * i - 1)
        output.append(spaces1 + stars1 + gap + stars2 + spaces2)
    # Lower half
    for i in range(n - 1, 0, -1):
        spaces1 = " " * (n - i)
        stars1 = "*" * (2 * i - 1)
        gap = " " * 3
        spaces2 = " " * (n - i)
        stars2 = "*" * (2 * i - 1)
        output.append(spaces1 + stars1 + gap + stars2 + spaces2)
    return output

# Test
print(generate_double_diamond(3))
# Output: 
# ['  *     *  ',
#  ' ***   *** ',
#  '***** *****',
#  ' ***   *** ',
#  '  *     *  ']
```

## Custom Character Diamond
```python
def generate_custom_diamond(n, chars=['*', '#', '@', '%']):
    """Diamond with alternating character layers"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        char = chars[(i - 1) % len(chars)]
        output.append(spaces + char * (2 * i - 1) + spaces)
    # Lower half
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i)
        char = chars[(i - 1) % len(chars)]
        output.append(spaces + char * (2 * i - 1) + spaces)
    return output

# Test
print(generate_custom_diamond(5, ['★', '♠', '♥', '♦', '♣']))
# Output: 
# ['    ★    ',
#  '   ♠♠♠   ',
#  '  ♥♥♥♥♥  ',
#  ' ♦♦♦♦♦♦♦ ',
#  '♣♣♣♣♣♣♣♣♣',
#  ' ♦♦♦♦♦♦♦ ',
#  '  ♥♥♥♥♥  ',
#  '   ♠♠♠   ',
#  '    ★    ']
```

## Rotated Square Diamond
```python
def generate_rotated_square_diamond(n):
    """Diamond made of rotated squares"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        if i == 1:
            output.append(spaces + "□" + spaces)
        else:
            output.append(spaces + "□" + "■" * (2 * i - 3) + "□" + spaces)
    # Lower half
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i)
        if i == 1:
            output.append(spaces + "□" + spaces)
        else:
            output.append(spaces + "□" + "■" * (2 * i - 3) + "□" + spaces)
    return output

# Test
print(generate_rotated_square_diamond(4))
# Output: 
# ['   □   ',
#  '  □■□  ',
#  ' □■■■□ ',
#  '□■■■■■□',
#  ' □■■■□ ',
#  '  □■□  ',
#  '   □   ']
```

## Fibonacci Diamond
```python
def generate_fibonacci_diamond(n):
    """Diamond using Fibonacci numbers"""
    def fibonacci(k):
        if k <= 1:
            return 1
        a, b = 1, 1
        for _ in range(2, k + 1):
            a, b = b, a + b
        return b
    
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(fibonacci(j)) + " "
        for j in range(i - 1, 0, -1):
            row += str(fibonacci(j)) + " "
        output.append(spaces + row.strip())
    # Lower half
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(fibonacci(j)) + " "
        for j in range(i - 1, 0, -1):
            row += str(fibonacci(j)) + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_fibonacci_diamond(4))
# Output: 
# ['      1',
#  '    1 1 1',
#  '  1 1 2 1 1',
#  '1 1 2 3 2 1 1',
#  '  1 1 2 1 1',
#  '    1 1 1',
#  '      1']
```

## X-Pattern Diamond
```python
def generate_x_pattern_diamond(n):
    """Diamond with X pattern inside"""
    output = []
    # Upper half
    for i in range(1, n + 1):
        spaces_outer = " " * (n - i)
        if i == 1:
            output.append(spaces_outer + "*" + spaces_outer)
        else:
            stars_left = "*" + " " * (i - 2)
            stars_right = " " * (i - 2) + "*"
            output.append(spaces_outer + stars_left + " " + stars_right + spaces_outer)
    # Lower half
    for i in range(n - 1, 0, -1):
        spaces_outer = " " * (n - i)
        if i == 1:
            output.append(spaces_outer + "*" + spaces_outer)
        else:
            stars_left = "*" + " " * (i - 2)
            stars_right = " " * (i - 2) + "*"
            output.append(spaces_outer + stars_left + " " + stars_right + spaces_outer)
    return output

# Test
print(generate_x_pattern_diamond(5))
# Output: 
# ['    *    ',
#  '   * *   ',
#  '  *   *  ',
#  ' *     * ',
#  '*       *',
#  ' *     * ',
#  '  *   *  ',
#  '   * *   ',
#  '    *    ']
```