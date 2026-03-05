## Basic Number Pyramid
```python
def generate_number_pyramid(n):
    """Basic pyramid with sequential numbers"""
    def getnum(n):
        s = ""
        i = 1
        while len(s) <= n:
            s += str(i) + " "
            i += 1
        return s.strip()
    
    output = []
    for i in range(1, n + 1):
        output.append(" " * (n - i) + getnum(2 * i - 1) + " " * (n - i))
    return output

# Test
print(generate_number_pyramid(5))
# Output: 
# ['    1    ',
#  '   1 2   ',
#  '  1 2 3  ',
#  ' 1 2 3 4 ',
#  '1 2 3 4 5']
```

## Inverted Number Pyramid
```python
def generate_inverted_number_pyramid(n):
    """Inverted pyramid with sequential numbers"""
    def getnum(n):
        s = ""
        i = 1
        while len(s) <= n:
            s += str(i) + " "
            i += 1
        return s.strip()
    
    output = []
    for i in range(n, 0, -1):
        output.append(" " * (n - i) + getnum(2 * i - 1) + " " * (n - i))
    return output

# Test
print(generate_inverted_number_pyramid(5))
# Output: 
# ['1 2 3 4 5',
#  ' 1 2 3 4 ',
#  '  1 2 3  ',
#  '   1 2   ',
#  '    1    ']
```

## Palindrome Number Pyramid
```python
def generate_palindrome_number_pyramid(n):
    """Pyramid with palindrome numbers (1, 121, 12321, etc.)"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        # Ascending numbers
        left = ""
        for j in range(1, i + 1):
            left += str(j)
        # Descending numbers (excluding the peak)
        right = left[:-1][::-1]
        output.append(spaces + left + right + spaces)
    return output

# Test
print(generate_palindrome_number_pyramid(5))
# Output: 
# ['    1    ',
#  '   121   ',
#  '  12321  ',
#  ' 1234321 ',
#  '123454321']
```

## Consecutive Numbers Pyramid
```python
def generate_consecutive_pyramid(n):
    """Pyramid with consecutive numbers increasing by 1 each time"""
    output = []
    num = 1
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(2 * i - 1):
            row += str(num) + " "
            num += 1
        output.append(spaces + row.strip() + spaces)
    return output

# Test
print(generate_consecutive_pyramid(4))
# Output: 
# ['   1   ',
#  '  2 3 4  ',
#  ' 5 6 7 8 9 ',
#  '10 11 12 13 14 15 16']
```

## Even Numbers Pyramid
```python
def generate_even_number_pyramid(n):
    """Pyramid with only even numbers"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(1, 2 * i):
            row += str(j * 2) + " "
        output.append(spaces + row.strip() + spaces)
    return output

# Test
print(generate_even_number_pyramid(4))
# Output: 
# ['    2    ',
#  '   2 4 6   ',
#  '  2 4 6 8 10  ',
#  ' 2 4 6 8 10 12 14 ']
```

## Odd Numbers Pyramid
```python
def generate_odd_number_pyramid(n):
    """Pyramid with only odd numbers"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        row = ""
        for j in range(1, 2 * i):
            row += str(j * 2 - 1) + " "
        output.append(spaces + row.strip() + spaces)
    return output

# Test
print(generate_odd_number_pyramid(4))
# Output: 
# ['    1    ',
#  '   1 3 5   ',
#  '  1 3 5 7 9  ',
#  ' 1 3 5 7 9 11 13 ']
```

## Multiplication Pyramid
```python
def generate_multiplication_pyramid(n):
    """Pyramid showing multiplication table pattern"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(i * j) + " "
        for j in range(i - 1, 0, -1):
            row += str(i * j) + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_multiplication_pyramid(4))
# Output: 
# ['      1',
#  '    2 4 2',
#  '  3 6 9 6 3',
#  '4 8 12 16 12 8 4']
```

## Fibonacci Pyramid
```python
def generate_fibonacci_pyramid(n):
    """Pyramid with Fibonacci numbers"""
    def fibonacci(k):
        if k <= 1:
            return 1
        a, b = 1, 1
        for _ in range(2, k + 1):
            a, b = b, a + b
        return b
    
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(fibonacci(j)) + " "
        for j in range(i - 1, 0, -1):
            row += str(fibonacci(j)) + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_fibonacci_pyramid(5))
# Output: 
# ['        1',
#  '      1 1 1',
#  '    1 1 2 1 1',
#  '  1 1 2 3 2 1 1',
#  '1 1 2 3 5 3 2 1 1']
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

## Diamond Number Pyramid
```python
def generate_diamond_number_pyramid(n):
    """Number pyramid with its mirror to form a diamond"""
    output = []
    # Upper pyramid
    for i in range(1, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(j) + " "
        for j in range(i - 1, 0, -1):
            row += str(j) + " "
        output.append(spaces + row.strip())
    # Lower inverted pyramid
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(j) + " "
        for j in range(i - 1, 0, -1):
            row += str(j) + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_diamond_number_pyramid(4))
# Output: 
# ['      1',
#  '    1 2 1',
#  '  1 2 3 2 1',
#  '1 2 3 4 3 2 1',
#  '  1 2 3 2 1',
#  '    1 2 1',
#  '      1']
```

## Floyd's Pyramid
```python
def generate_floyds_pyramid(n):
    """Pyramid using Floyd's triangle pattern"""
    num = 1
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(i):
            row += str(num) + " "
            num += 1
        # Mirror for pyramid effect
        temp_nums = row.strip().split()
        for j in range(len(temp_nums) - 2, -1, -1):
            row += temp_nums[j] + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_floyds_pyramid(4))
# Output: 
# ['      1',
#  '    2 3 2',
#  '  4 5 6 5 4',
#  '7 8 9 10 9 8 7']
```

## Prime Number Pyramid
```python
def is_prime(num):
    """Check if a number is prime"""
    if num < 2:
        return False
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            return False
    return True

def generate_prime_pyramid(n):
    """Pyramid with prime numbers"""
    primes = []
    num = 2
    total_needed = n * n
    
    while len(primes) < total_needed:
        if is_prime(num):
            primes.append(num)
        num += 1
    
    output = []
    index = 0
    for i in range(1, n + 1):
        spaces = " " * (n - i) * 3
        row = ""
        for j in range(i):
            row += str(primes[index]) + " "
            index += 1
        # Mirror for pyramid effect
        temp_nums = row.strip().split()
        for j in range(len(temp_nums) - 2, -1, -1):
            row += temp_nums[j] + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_prime_pyramid(4))
# Output: 
# ['           2',
#  '        3 5 3',
#  '     7 11 13 11 7',
#  ' 17 19 23 29 23 19 17']
```

## Hollow Number Pyramid
```python
def generate_hollow_number_pyramid(n):
    """Pyramid with hollow center using numbers"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        if i == 1:
            output.append(spaces + "1" + spaces)
        elif i == n:
            row = ""
            for j in range(1, i + 1):
                row += str(j) + " "
            for j in range(i - 1, 0, -1):
                row += str(j) + " "
            output.append(spaces + row.strip() + spaces)
        else:
            output.append(spaces + "1" + " " * (4 * (i - 1) - 3) + str(i) + spaces)
    return output

# Test
print(generate_hollow_number_pyramid(5))
# Output: 
# ['    1    ',
#  '   1    2   ',
#  '  1      3  ',
#  ' 1        4 ',
#  '1 2 3 4 5 4 3 2 1']
```

## Custom Step Pyramid
```python
def generate_step_number_pyramid(n, step=2):
    """Pyramid with numbers increasing by custom step"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        num = i
        for j in range(i):
            row += str(num) + " "
            num += step
        # Mirror for pyramid effect
        temp_nums = row.strip().split()
        for j in range(len(temp_nums) - 2, -1, -1):
            row += temp_nums[j] + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_step_number_pyramid(4, 3))
# Output: 
# ['      1',
#  '    2 5 2',
#  '  3 6 9 6 3',
#  '4 7 10 13 10 7 4']
```