Basic Implementation
```python

def celsius_to_fahrenheit(C):
    """
    Function to convert temperature from Celsius to Fahrenheit.
    
    Parameters:
    C (float): The temperature in Celsius.
    
    Returns:
    float: The temperature in Fahrenheit.
    """
    return ((9/5 * C) + 32)

print(celsius_to_fahrenheit(0))
print(celsius_to_fahrenheit(100))
print(celsius_to_fahrenheit(-40))
```
Output:
```
32.0
212.0
-40.0
```

Using round for cleaner output
```python

def celsius_to_fahrenheit(C):
    """
    Function to convert temperature from Celsius to Fahrenheit.
    
    Parameters:
    C (float): The temperature in Celsius.
    
    Returns:
    float: The temperature in Fahrenheit rounded to 2 decimal places.
    """
    return round(((9/5 * C) + 32), 2)

print(celsius_to_fahrenheit(36.6))
print(celsius_to_fahrenheit(37))
print(celsius_to_fahrenheit(0.5))
```
Output:
```
97.88
98.6
32.9
```

With input validation
```python

def celsius_to_fahrenheit(C):
    """
    Function to convert temperature from Celsius to Fahrenheit.
    
    Parameters:
    C (float): The temperature in Celsius.
    
    Returns:
    float: The temperature in Fahrenheit.
    """
    if not isinstance(C, (int, float)):
        raise TypeError("Temperature must be a number")
    return ((9/5 * C) + 32)

print(celsius_to_fahrenheit(25))
print(celsius_to_fahrenheit(-10))
```
Output:
```
77.0
14.0
```

Lambda function version
```python

celsius_to_fahrenheit = lambda C: ((9/5 * C) + 32)

print(celsius_to_fahrenheit(20))
print(celsius_to_fahrenheit(30))
print(celsius_to_fahrenheit(40))
```
Output:
```
68.0
86.0
104.0
```

With docstring and type hints
```python

def celsius_to_fahrenheit(C: float) -> float:
    """
    Function to convert temperature from Celsius to Fahrenheit.
    
    Parameters:
    C (float): The temperature in Celsius.
    
    Returns:
    float: The temperature in Fahrenheit.
    """
    return ((9/5 * C) + 32)

print(celsius_to_fahrenheit(15))
print(celsius_to_fahrenheit(22.5))
```
Output:
```
59.0
72.5
```

Using f-string for formatted output
```python

def celsius_to_fahrenheit(C):
    """
    Function to convert temperature from Celsius to Fahrenheit.
    
    Parameters:
    C (float): The temperature in Celsius.
    
    Returns:
    float: The temperature in Fahrenheit.
    """
    return ((9/5 * C) + 32)

c = 24
f = celsius_to_fahrenheit(c)
print(f"{c}°C is equal to {f}°F")
```
Output:
```
24°C is equal to 75.2°F
```

With multiple return statements
```python

def celsius_to_fahrenheit(C):
    """
    Function to convert temperature from Celsius to Fahrenheit.
    
    Parameters:
    C (float): The temperature in Celsius.
    
    Returns:
    float: The temperature in Fahrenheit.
    """
    if C == 0:
        return 32
    elif C == 100:
        return 212
    else:
        return ((9/5 * C) + 32)

print(celsius_to_fahrenheit(0))
print(celsius_to_fahrenheit(100))
print(celsius_to_fahrenheit(50))
```
Output:
```
32
212
122.0
```

Using integer division alternative
```python

def celsius_to_fahrenheit(C):
    """
    Function to convert temperature from Celsius to Fahrenheit.
    
    Parameters:
    C (float): The temperature in Celsius.
    
    Returns:
    float: The temperature in Fahrenheit.
    """
    return ((9 * C / 5) + 32)

print(celsius_to_fahrenheit(10))
print(celsius_to_fahrenheit(35))
```
Output:
```
50.0
95.0
```

With default parameter
```python

def celsius_to_fahrenheit(C=0):
    """
    Function to convert temperature from Celsius to Fahrenheit.
    
    Parameters:
    C (float): The temperature in Celsius. Default is 0.
    
    Returns:
    float: The temperature in Fahrenheit.
    """
    return ((9/5 * C) + 32)

print(celsius_to_fahrenheit())
print(celsius_to_fahrenheit(25))
```
Output:
```
32.0
77.0
```

One-liner with print
```python

def celsius_to_fahrenheit(C): return ((9/5 * C) + 32)

print(celsius_to_fahrenheit(18))
print(celsius_to_fahrenheit(27))
```
Output:
```
64.4
80.6
```