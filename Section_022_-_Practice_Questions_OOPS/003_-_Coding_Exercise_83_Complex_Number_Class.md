```python
# Quickcode
class ComplexNumber:
    def __init__(self, real, imaginary):
        self.real = real
        self.imaginary = imaginary

    def add(self, other):
        return ComplexNumber(self.real + other.real, self.imaginary + other.imaginary)

    def subtract(self, other):
        return ComplexNumber(self.real - other.real, self.imaginary - other.imaginary)

    def multiply(self, other):
        real = self.real * other.real - self.imaginary * other.imaginary
        imag = self.real * other.imaginary + self.imaginary * other.real
        return ComplexNumber(real, imag)

    def __eq__(self, other):
        return self.real == other.real and self.imaginary == other.imaginary

    def __str__(self):
        if self.imaginary >= 0:
            return f"{self.real} + {self.imaginary}i"
        else:
            return f"{self.real} - {abs(self.imaginary)}i"
```
## Python Implementation — `ComplexNumber` Class

### Mathematical Representation of Complex Numbers

A **complex number** is represented as:

```
a + bi
```

Where the following relationships apply.

| Component | Meaning                           |
| --------- | --------------------------------- |
| `a`       | Real part of the complex number   |
| `b`       | Imaginary coefficient             |
| `i`       | Imaginary unit where i² equals −1 |

Arithmetic operations follow standard algebraic rules.

| Operation      | Formula                                     |
| -------------- | ------------------------------------------- |
| Addition       | `(a + bi) + (c + di) = (a+c) + (b+d)i`      |
| Subtraction    | `(a + bi) - (c + di) = (a-c) + (b-d)i`      |
| Multiplication | `(a + bi)(c + di) = (ac - bd) + (ad + bc)i` |

---

# Python Class Implementation

```python
class ComplexNumber:
    """
    Custom implementation of a complex number class.

    The class stores real and imaginary components and
    supports addition, subtraction, multiplication,
    equality comparison, and string representation.
    """

    def __init__(self, real, imaginary):
        """
        Constructor initializes the real and imaginary parts.

        Parameters
        ----------
        real : int or float
            Real component of the complex number.

        imaginary : int or float
            Imaginary component of the complex number.
        """

        # Store real part of complex number
        self.real = real

        # Store imaginary part of complex number
        self.imaginary = imaginary


    def add(self, other):
        """
        Add another ComplexNumber object to the current object.

        Formula:
        (a + bi) + (c + di) = (a+c) + (b+d)i
        """

        # Calculate new real component
        new_real = self.real + other.real

        # Calculate new imaginary component
        new_imaginary = self.imaginary + other.imaginary

        # Return a new ComplexNumber object containing the result
        return ComplexNumber(new_real, new_imaginary)


    def subtract(self, other):
        """
        Subtract another ComplexNumber object from the current object.

        Formula:
        (a + bi) - (c + di) = (a-c) + (b-d)i
        """

        # Calculate resulting real component
        new_real = self.real - other.real

        # Calculate resulting imaginary component
        new_imaginary = self.imaginary - other.imaginary

        # Return a new ComplexNumber object containing the result
        return ComplexNumber(new_real, new_imaginary)


    def multiply(self, other):
        """
        Multiply two complex numbers.

        Formula:
        (a + bi)(c + di) = (ac - bd) + (ad + bc)i
        """

        # Compute the real component using ac - bd
        new_real = (self.real * other.real) - (self.imaginary * other.imaginary)

        # Compute the imaginary component using ad + bc
        new_imaginary = (self.real * other.imaginary) + (self.imaginary * other.real)

        # Return the resulting complex number object
        return ComplexNumber(new_real, new_imaginary)


    def __eq__(self, other):
        """
        Equality comparison between two complex numbers.

        Two complex numbers are equal when both real
        and imaginary components match exactly.
        """

        # Return True only when both components match
        return self.real == other.real and self.imaginary == other.imaginary


    def __str__(self):
        """
        Human readable representation of the complex number.

        Format example:
        "3 + 7i"
        "1 - 1i"
        """

        # Handle sign formatting for the imaginary component
        if self.imaginary >= 0:
            return f"{self.real} + {self.imaginary}i"
        else:
            return f"{self.real} - {abs(self.imaginary)}i"
```

---

# Example Usage

```python
# Creating instances of ComplexNumber class
c1 = ComplexNumber(2, 3)
c2 = ComplexNumber(1, 4)

# Testing addition
print("Addition Result:", c1.add(c2))

# Testing subtraction
print("Subtraction Result:", c1.subtract(c2))

# Testing multiplication
print("Multiplication Result:", c1.multiply(c2))

# Testing equality comparison
print("Equality Test:", c1 == ComplexNumber(2, 3))


# Comparison with Python built-in complex class
py_c1 = complex(2, 3)
py_c2 = complex(1, 4)

print("Python Addition Result:", py_c1 + py_c2)
print("Python Subtraction Result:", py_c1 - py_c2)
print("Python Multiplication Result:", py_c1 * py_c2)
```

---

# Expected Output

```
Addition Result: 3 + 7i
Subtraction Result: 1 - 1i
Multiplication Result: -10 + 11i
Equality Test: True
Python Addition Result: (3+7j)
Python Subtraction Result: (1-1j)
Python Multiplication Result: (-10+11j)
```
