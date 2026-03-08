```python
# Quickcode
class Fraction:
    def __init__(self, numerator, denominator):
        if denominator == 0:
            raise ValueError("Denominator cannot be zero")
        self.numerator = numerator
        self.denominator = denominator
        self._simplify()

    def _find_gcd(self, a, b):
        a = abs(a)
        b = abs(b)
        while b != 0:
            a, b = b, a % b
        return a

    def _simplify(self):
        gcd = self._find_gcd(self.numerator, self.denominator)
        self.numerator //= gcd
        self.denominator //= gcd
        if self.denominator < 0:
            self.numerator = -self.numerator
            self.denominator = -self.denominator

    def add(self, other):
        n = self.numerator * other.denominator + other.numerator * self.denominator
        d = self.denominator * other.denominator
        return Fraction(n, d)

    def subtract(self, other):
        n = self.numerator * other.denominator - other.numerator * self.denominator
        d = self.denominator * other.denominator
        return Fraction(n, d)

    def multiply(self, other):
        n = self.numerator * other.numerator
        d = self.denominator * other.denominator
        return Fraction(n, d)

    def divide(self, other):
        if other.numerator == 0:
            raise ValueError("Error: Cannot divide by zero")
        n = self.numerator * other.denominator
        d = self.denominator * other.numerator
        return Fraction(n, d)

    def __eq__(self, other):
        return self.numerator == other.numerator and self.denominator == other.denominator

    def __str__(self):
        return f"{self.numerator}/{self.denominator}"

    def __repr__(self):
        return f"Fraction(numerator={self.numerator}, denominator={self.denominator})"
```

## Python Implementation — `Fraction` Class

### Mathematical Representation of Fractions

A **fraction** represents a rational number expressed as a ratio between two integers.

```
numerator / denominator
```

| Component     | Meaning                                         |
| ------------- | ----------------------------------------------- |
| `numerator`   | Top part of the fraction                        |
| `denominator` | Bottom part of the fraction                     |
| `GCD`         | Greatest common divisor used for simplification |

Fractions must always remain in **simplified form**. Simplification occurs by dividing both numerator and denominator by their **greatest common divisor**.

---

# Python Class Implementation

```python
class Fraction:
    """
    Class representing mathematical fractions with arithmetic operations.
    Fractions are always stored in their simplified form.
    """

    def __init__(self, numerator, denominator):
        """
        Constructor initializes numerator and denominator.

        Denominator must never be zero because division by zero
        is mathematically undefined.
        """

        # Validate denominator before storing fraction
        if denominator == 0:
            raise ValueError("Denominator cannot be zero")

        # Store numerator value
        self.numerator = numerator

        # Store denominator value
        self.denominator = denominator

        # Automatically simplify fraction when created
        self._simplify()


    def _find_gcd(self, a, b):
        """
        Compute greatest common divisor using iterative Euclidean algorithm.

        Recursion and imported functions are intentionally avoided.
        """

        # Convert numbers to positive values for safe computation
        a = abs(a)
        b = abs(b)

        # Iteratively compute GCD using remainder operation
        while b != 0:
            temp = b
            b = a % b
            a = temp

        # Final value of 'a' becomes the GCD
        return a


    def _simplify(self):
        """
        Reduce fraction to its simplest form using GCD.
        """

        # Find greatest common divisor of numerator and denominator
        gcd_value = self._find_gcd(self.numerator, self.denominator)

        # Divide numerator and denominator by the GCD
        self.numerator = self.numerator // gcd_value
        self.denominator = self.denominator // gcd_value

        # Normalize negative sign so denominator stays positive
        if self.denominator < 0:
            self.numerator = -self.numerator
            self.denominator = -self.denominator


    def add(self, other):
        """
        Add two fractions using common denominator formula.

        (a/b) + (c/d) = (ad + bc) / bd
        """

        # Compute new numerator using cross multiplication
        new_numerator = (self.numerator * other.denominator) + (other.numerator * self.denominator)

        # Compute new denominator
        new_denominator = self.denominator * other.denominator

        # Return simplified Fraction object
        return Fraction(new_numerator, new_denominator)


    def subtract(self, other):
        """
        Subtract two fractions using common denominator formula.

        (a/b) - (c/d) = (ad - bc) / bd
        """

        # Compute numerator difference using cross multiplication
        new_numerator = (self.numerator * other.denominator) - (other.numerator * self.denominator)

        # Compute denominator
        new_denominator = self.denominator * other.denominator

        # Return simplified Fraction object
        return Fraction(new_numerator, new_denominator)


    def multiply(self, other):
        """
        Multiply two fractions.

        (a/b) * (c/d) = (ac) / (bd)
        """

        # Multiply numerators together
        new_numerator = self.numerator * other.numerator

        # Multiply denominators together
        new_denominator = self.denominator * other.denominator

        # Return simplified Fraction object
        return Fraction(new_numerator, new_denominator)


    def divide(self, other):
        """
        Divide one fraction by another.

        (a/b) ÷ (c/d) = (a/b) * (d/c)
        """

        # Division by zero occurs when other numerator equals zero
        if other.numerator == 0:
            raise ValueError("Error: Cannot divide by zero")

        # Multiply by reciprocal of second fraction
        new_numerator = self.numerator * other.denominator
        new_denominator = self.denominator * other.numerator

        # Return simplified Fraction object
        return Fraction(new_numerator, new_denominator)


    def __eq__(self, other):
        """
        Compare two fractions for equality.

        Since fractions are always simplified, equality becomes
        a direct comparison of numerator and denominator.
        """

        return (self.numerator == other.numerator) and (self.denominator == other.denominator)


    def __str__(self):
        """
        Human readable string representation.
        """

        return f"{self.numerator}/{self.denominator}"


    def __repr__(self):
        """
        Developer friendly representation useful for debugging.
        """

        return f"Fraction(numerator={self.numerator}, denominator={self.denominator})"
```

---

# Example Usage

```python
# Creating instances of the Fraction class
frac1 = Fraction(1, 2)
frac2 = Fraction(3, 4)

# Addition
print(frac1.add(frac2))

# Subtraction
print(frac1.subtract(frac2))

# Multiplication
print(frac1.multiply(frac2))

# Division
print(frac1.divide(frac2))

# Division by zero example
try:
    print(frac1.divide(Fraction(0, 1)))
except ValueError as error:
    print(error)

# Equality comparison
print(frac1 == Fraction(2, 4))

# String representation
print(frac1)
```

---

# Expected Output

```
5/4
-1/4
3/8
2/3
Error: Cannot divide by zero
True
1/2
```
