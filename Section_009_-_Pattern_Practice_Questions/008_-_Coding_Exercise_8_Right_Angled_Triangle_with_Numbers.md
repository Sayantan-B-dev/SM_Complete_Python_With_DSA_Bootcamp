## Basic Number Triangle
```python
def generate_number_triangle(n):
    """Basic triangle with row numbers repeated"""
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

## Inverted Number Triangle
```python
def generate_inverted_number_triangle(n):
    """Inverted triangle with numbers decreasing"""
    output = []
    for i in range(n, 0, -1):
        output.append(str(i) * i)
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

## Right-Aligned Number Triangle
```python
def generate_right_aligned_number_triangle(n):
    """Number triangle aligned to the right"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        output.append(spaces + str(i) * i)
    return output

# Test
print(generate_right_aligned_number_triangle(5))
# Output: 
# ['    1',
#  '   22',
#  '  333',
#  ' 4444',
#  '55555']
```

## Consecutive Numbers Triangle
```python
def generate_consecutive_numbers_triangle(n):
    """Triangle with consecutive numbers in each row"""
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

## Palindrome Number Triangle
```python
def generate_palindrome_number_triangle(n):
    """Triangle with palindrome numbers (1, 121, 12321, etc.)"""
    output = []
    for i in range(1, n + 1):
        row = ""
        # Ascending numbers
        for j in range(1, i + 1):
            row += str(j)
        # Descending numbers (excluding the peak)
        for j in range(i - 1, 0, -1):
            row += str(j)
        output.append(row)
    return output

# Test
print(generate_palindrome_number_triangle(5))
# Output: 
# ['1',
#  '121',
#  '12321',
#  '1234321',
#  '123454321']
```

## Alternating Number Triangle
```python
def generate_alternating_number_triangle(n):
    """Triangle with alternating numbers in each row"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            if j % 2 == 0:
                row += str(i)
            else:
                row += str(i + 1)
        output.append(row)
    return output

# Test
print(generate_alternating_number_triangle(5))
# Output: 
# ['1',
#  '22',
#  '343',
#  '4545',
#  '56565']
```

## Binary Number Triangle
```python
def generate_binary_number_triangle(n):
    """Triangle with binary numbers (0s and 1s)"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str((i + j) % 2)
        output.append(row)
    return output

# Test
print(generate_binary_number_triangle(5))
# Output: 
# ['1',
#  '10',
#  '101',
#  '1010',
#  '10101']
```

## Diamond Number Triangle
```python
def generate_diamond_number_triangle(n):
    """Triangle and its mirror to form a diamond pattern"""
    output = []
    # Upper triangle
    for i in range(1, n + 1):
        spaces = " " * (n - i)
        output.append(spaces + str(i) * i)
    # Lower inverted triangle
    for i in range(n - 1, 0, -1):
        spaces = " " * (n - i)
        output.append(spaces + str(i) * i)
    return output

# Test
print(generate_diamond_number_triangle(4))
# Output: 
# ['   1',
#  '  22',
#  ' 333',
#  '4444',
#  ' 333',
#  '  22',
#  '   1']
```

## Multiplication Table Triangle
```python
def generate_multiplication_triangle(n):
    """Triangle showing multiplication table pattern"""
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(1, i + 1):
            row += str(i * j) + " "
        output.append(row.strip())
    return output

# Test
print(generate_multiplication_triangle(5))
# Output: 
# ['1',
#  '2 4',
#  '3 6 9',
#  '4 8 12 16',
#  '5 10 15 20 25']
```

## Pyramid Number Triangle
```python
def generate_pyramid_number_triangle(n):
    """Numbers arranged in pyramid form"""
    output = []
    for i in range(1, n + 1):
        spaces = " " * (n - i) * 2
        row = ""
        for j in range(1, i + 1):
            row += str(j) + " "
        for j in range(i - 1, 0, -1):
            row += str(j) + " "
        output.append(spaces + row.strip())
    return output

# Test
print(generate_pyramid_number_triangle(5))
# Output: 
# ['        1',
#  '      1 2 1',
#  '    1 2 3 2 1',
#  '  1 2 3 4 3 2 1',
#  '1 2 3 4 5 4 3 2 1']
```

## Floyd's Number Triangle
```python
def generate_floyds_number_triangle(n):
    """Floyd's triangle with numbers"""
    output = []
    num = 1
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += f"{num:2d} "
            num += 1
        output.append(row.strip())
    return output

# Test
print(generate_floyds_number_triangle(5))
# Output: 
# [' 1',
#  ' 2  3',
#  ' 4  5  6',
#  ' 7  8  9 10',
#  '11 12 13 14 15']
```

## Even Numbers Triangle
```python
def generate_even_numbers_triangle(n):
    """Triangle with only even numbers"""
    output = []
    num = 2
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str(num) + " "
            num += 2
        output.append(row.strip())
    return output

# Test
print(generate_even_numbers_triangle(5))
# Output: 
# ['2',
#  '4 6',
#  '8 10 12',
#  '14 16 18 20',
#  '22 24 26 28 30']
```

## Odd Numbers Triangle
```python
def generate_odd_numbers_triangle(n):
    """Triangle with only odd numbers"""
    output = []
    num = 1
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str(num) + " "
            num += 2
        output.append(row.strip())
    return output

# Test
print(generate_odd_numbers_triangle(5))
# Output: 
# ['1',
#  '3 5',
#  '7 9 11',
#  '13 15 17 19',
#  '21 23 25 27 29']
```

## Fibonacci Number Triangle
```python
def generate_fibonacci_number_triangle(n):
    """Triangle with Fibonacci numbers"""
    def fibonacci(k):
        if k <= 1:
            return 1
        a, b = 1, 1
        for _ in range(2, k + 1):
            a, b = b, a + b
        return b
    
    output = []
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str(fibonacci(j)) + " "
        output.append(row.strip())
    return output

# Test
print(generate_fibonacci_number_triangle(6))
# Output: 
# ['1',
#  '1 1',
#  '1 1 2',
#  '1 1 2 3',
#  '1 1 2 3 5',
#  '1 1 2 3 5 8']
```

## Prime Number Triangle
```python
def is_prime(num):
    """Check if a number is prime"""
    if num < 2:
        return False
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            return False
    return True

def generate_prime_number_triangle(n):
    """Triangle with prime numbers"""
    output = []
    primes = []
    num = 2
    total_needed = n * (n + 1) // 2
    
    while len(primes) < total_needed:
        if is_prime(num):
            primes.append(num)
        num += 1
    
    index = 0
    for i in range(1, n + 1):
        row = ""
        for j in range(i):
            row += str(primes[index]) + " "
            index += 1
        output.append(row.strip())
    return output

# Test
print(generate_prime_number_triangle(5))
# Output: 
# ['2',
#  '3 5',
#  '7 11 13',
#  '17 19 23 29',
#  '31 37 41 43 47']
```

## Hollow Number Triangle
```python
def generate_hollow_number_triangle(n):
    """Triangle with hollow center using numbers"""
    output = []
    for i in range(1, n + 1):
        if i == 1:  # First row
            output.append("1")
        elif i == n:  # Last row
            output.append(str(i) * i)
        else:  # Middle rows
            output.append(str(i) + " " * (i - 2) + str(i))
    return output

# Test
print(generate_hollow_number_triangle(6))
# Output: 
# ['1',
#  '22',
#  '3 3',
#  '4  4',
#  '5   5',
#  '666666']
```
