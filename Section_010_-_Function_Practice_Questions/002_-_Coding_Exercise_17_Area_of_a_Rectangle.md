Basic Implementation
```python

def area_of_rectangle(length, breadth):
    """
    Function to calculate the area of a rectangle.
    
    Parameters:
    length (float): The length of the rectangle.
    breadth (float): The breadth of the rectangle.
    
    Returns:
    float: The area of the rectangle.
    """
    return length*breadth

print(area_of_rectangle(5, 3))
print(area_of_rectangle(7, 2))
print(area_of_rectangle(10, 10))
```
Output:
```
15
14
100
```

With input validation
```python

def area_of_rectangle(length, breadth):
    """
    Function to calculate the area of a rectangle.
    
    Parameters:
    length (float): The length of the rectangle.
    breadth (float): The breadth of the rectangle.
    
    Returns:
    float: The area of the rectangle.
    """
    if length <= 0 or breadth <= 0:
        raise ValueError("Length and breadth must be positive numbers")
    return length*breadth

print(area_of_rectangle(4, 6))
print(area_of_rectangle(3.5, 2))
```
Output:
```
24
7.0
```

Using type hints and round
```python

def area_of_rectangle(length: float, breadth: float) -> float:
    """
    Function to calculate the area of a rectangle.
    
    Parameters:
    length (float): The length of the rectangle.
    breadth (float): The breadth of the rectangle.
    
    Returns:
    float: The area of the rectangle rounded to 2 decimals.
    """
    return round(length * breadth, 2)

print(area_of_rectangle(3.333, 4.444))
print(area_of_rectangle(5.5, 2.2))
```
Output:
```
14.81
12.1
```

Lambda function version
```python

area_of_rectangle = lambda length, breadth: length * breadth

print(area_of_rectangle(8, 5))
print(area_of_rectangle(12, 3))
print(area_of_rectangle(6, 7))
```
Output:
```
40
36
42
```

With default parameters
```python

def area_of_rectangle(length=1, breadth=1):
    """
    Function to calculate the area of a rectangle.
    
    Parameters:
    length (float): The length of the rectangle. Default is 1.
    breadth (float): The breadth of the rectangle. Default is 1.
    
    Returns:
    float: The area of the rectangle.
    """
    return length*breadth

print(area_of_rectangle())
print(area_of_rectangle(5))
print(area_of_rectangle(5, 4))
```
Output:
```
1
5
20
```

Using f-string for formatted output
```python

def area_of_rectangle(length, breadth):
    """
    Function to calculate the area of a rectangle.
    
    Parameters:
    length (float): The length of the rectangle.
    breadth (float): The breadth of the rectangle.
    
    Returns:
    float: The area of the rectangle.
    """
    return length*breadth

l, b = 7, 3
area = area_of_rectangle(l, b)
print(f"Rectangle with length {l} and breadth {b} has area = {area}")
```
Output:
```
Rectangle with length 7 and breadth 3 has area = 21
```

With multiple return statements
```python

def area_of_rectangle(length, breadth):
    """
    Function to calculate the area of a rectangle.
    
    Parameters:
    length (float): The length of the rectangle.
    breadth (float): The breadth of the rectangle.
    
    Returns:
    float: The area of the rectangle.
    """
    if length == breadth:
        return f"Square area: {length*breadth}"
    else:
        return length*breadth

print(area_of_rectangle(4, 4))
print(area_of_rectangle(5, 3))
```
Output:
```
Square area: 16
15
```

Using different arithmetic approach
```python

def area_of_rectangle(length, breadth):
    """
    Function to calculate the area of a rectangle.
    
    Parameters:
    length (float): The length of the rectangle.
    breadth (float): The breadth of the rectangle.
    
    Returns:
    float: The area of the rectangle.
    """
    return length * breadth  # Explicit multiplication

print(area_of_rectangle(9, 2))
print(area_of_rectangle(4.5, 3))
```
Output:
```
18
13.5
```

With docstring and parameter checking
```python

def area_of_rectangle(length, breadth):
    """
    Function to calculate the area of a rectangle.
    
    Parameters:
    length (float): The length of the rectangle.
    breadth (float): The breadth of the rectangle.
    
    Returns:
    float: The area of the rectangle.
    """
    if not isinstance(length, (int, float)) or not isinstance(breadth, (int, float)):
        raise TypeError("Length and breadth must be numbers")
    return length*breadth

print(area_of_rectangle(10, 6))
print(area_of_rectangle(2.5, 4))
```
Output:
```
60
10.0
```

One-liner with print
```python

def area_of_rectangle(length, breadth): return length*breadth

print(area_of_rectangle(15, 2))
print(area_of_rectangle(8, 8))
print(area_of_rectangle(3, 7))
```
Output:
```
30
64
21
```