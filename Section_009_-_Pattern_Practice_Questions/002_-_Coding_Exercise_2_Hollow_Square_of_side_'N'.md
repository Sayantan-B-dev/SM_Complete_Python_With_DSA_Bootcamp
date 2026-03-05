
## Hollow Square
```python
def generate_hollow_square(n):
    """Creates a hollow square pattern with only border '*'"""
    output = []
    for i in range(n):
        if i == 0 or i == n-1:  # First and last row
            output.append("*" * n)
        else:  # Middle rows - only first and last *
            output.append("*" + " " * (n-2) + "*")
    return output

# Test
print(generate_hollow_square(5))
# Output: ['*****', '*   *', '*   *', '*   *', '*****']
```
