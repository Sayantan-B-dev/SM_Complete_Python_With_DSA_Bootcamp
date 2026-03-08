```python
# Quickcode
class Calculator:
    def __init__(self, num1, num2):
        self.num1 = num1
        self.num2 = num2

    def add(self):
        return self.num1 + self.num2

    def subtract(self):
        return self.num1 - self.num2

    def multiply(self, factor):
        return self.num1 * factor

    def divide(self, divisor):
        if divisor == 0:
            print("Error: Cannot divide by zero")
            return None
        return self.num1 / divisor
```


## Python Class Implementation — `Calculator`

### Class Design Description

The **Calculator class** models a simple arithmetic computation unit containing two stored operands. The object maintains two attributes, `num1` and `num2`, initialized during object construction. Arithmetic operations are implemented as class methods that perform mathematical computations based on the stored attributes and provided parameters.

The implementation follows standard object-oriented design principles where the constructor initializes internal state and each method performs a well-defined arithmetic operation using that state.

### Class Attributes

| Attribute | Description                                                 |
| --------- | ----------------------------------------------------------- |
| `num1`    | Primary numeric operand used in operations                  |
| `num2`    | Secondary numeric operand used for addition and subtraction |

### Class Methods

| Method             | Parameters | Operation                                               |
| ------------------ | ---------- | ------------------------------------------------------- |
| `add()`            | None       | Returns sum of `num1` and `num2`                        |
| `subtract()`       | None       | Returns `num1 - num2`                                   |
| `multiply(factor)` | `factor`   | Returns `num1 * factor`                                 |
| `divide(divisor)`  | `divisor`  | Returns `num1 / divisor`, with zero division protection |

---

## Python Implementation

```python
class Calculator:
    """
    Calculator class implementing basic arithmetic operations.

    The class stores two numbers during initialization and
    exposes several methods that perform arithmetic operations.
    """

    def __init__(self, num1, num2):
        """
        Constructor initializes two numeric attributes.

        Parameters
        ----------
        num1 : int or float
            First numeric value stored in the calculator.

        num2 : int or float
            Second numeric value stored in the calculator.
        """

        # Store the first operand inside the object state
        self.num1 = num1

        # Store the second operand inside the object state
        self.num2 = num2


    def add(self):
        """
        Compute the sum of num1 and num2.

        Returns
        -------
        int or float
            Result of num1 + num2.
        """

        # Perform addition between the two stored attributes
        return self.num1 + self.num2


    def subtract(self):
        """
        Compute subtraction between num1 and num2.

        Returns
        -------
        int or float
            Result of num1 minus num2.
        """

        # Subtract num2 from num1
        return self.num1 - self.num2


    def multiply(self, factor):
        """
        Multiply num1 with a provided factor.

        Parameters
        ----------
        factor : int or float
            The multiplier applied to num1.

        Returns
        -------
        int or float
            Product of num1 and factor.
        """

        # Multiply num1 by the given factor
        return self.num1 * factor


    def divide(self, divisor):
        """
        Divide num1 by a given divisor.

        Parameters
        ----------
        divisor : int or float
            The number used to divide num1.

        Returns
        -------
        float or None
            Division result if divisor is valid.
            None returned when divisor equals zero.
        """

        # Prevent division by zero error
        if divisor == 0:

            # Print error message when invalid divisor detected
            print("Error: Cannot divide by zero")

            # Return None to indicate invalid operation
            return None

        # Perform division when divisor is valid
        return self.num1 / divisor
```

---

## Example Usage

```python
# Create an instance of the Calculator class
calculator = Calculator(10, 5)

# Test addition operation
print("Addition Result:", calculator.add())

# Test subtraction operation
print("Subtraction Result:", calculator.subtract())

# Test multiplication operation using factor 3
print("Multiplication Result:", calculator.multiply(3))

# Test division operation using divisor 2
print("Division Result:", calculator.divide(2))

# Test division operation using divisor 0
print("Division Result:", calculator.divide(0))
```

### Expected Output

```
Addition Result: 15
Subtraction Result: 5
Multiplication Result: 30
Division Result: 5.0
Error: Cannot divide by zero
Division Result: None
```
