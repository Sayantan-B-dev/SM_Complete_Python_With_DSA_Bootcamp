### Evaluate Postfix

```python
def evaluate_postfix(expression):
    stack = []
    for token in expression:
        if token.lstrip('-').isdigit():
            stack.append(int(token))
        else:
            b = stack.pop()
            a = stack.pop()
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            elif token == '/':
                stack.append(int(a / b))
    return stack.pop()
```


## Postfix Expression Evaluation Using Stack

### Conceptual Framework

A **postfix expression** (Reverse Polish Notation) places operators **after their operands**.
This notation eliminates the need for parentheses because evaluation order becomes unambiguous.

Example postfix expression:

```
2 3 1 * + 9 -
```

Equivalent infix expression:

```
2 + (3 * 1) - 9
```

Evaluation proceeds strictly from **left to right**, using a stack to temporarily store operands.

### Operational Rules

| Token Type           | Action                                                |
| -------------------- | ----------------------------------------------------- |
| Operand (number)     | Push onto stack                                       |
| Operator (`+ - * /`) | Pop two operands, perform operation, push result back |

Important ordering rule:

```
operand1 operator operand2
```

When popping:

```
operand2 = stack.pop()
operand1 = stack.pop()
result = operand1 operator operand2
```

This order preserves correct arithmetic evaluation.

### Complexity Characteristics

| Metric           | Value                                    |
| ---------------- | ---------------------------------------- |
| Time Complexity  | `O(n)` where `n` equals number of tokens |
| Space Complexity | `O(n)` stack storage in worst case       |

---

## Implementation

```python
def evaluate_postfix(expression):
    """
    Evaluates a postfix expression using a stack-based approach.

    Parameters
    ----------
    expression : List[str]
        A list representing the postfix expression where each
        element is either an operand or an arithmetic operator.

    Returns
    -------
    int
        The computed result of the postfix expression.
    """

    # Stack used to store operands during evaluation
    stack = []

    # Traverse every token in the postfix expression
    for token in expression:

        # If token represents a number, convert to integer and push onto stack
        if token.lstrip('-').isdigit():
            stack.append(int(token))

        else:
            # Pop the two most recent operands from stack
            operand2 = stack.pop()
            operand1 = stack.pop()

            # Perform operation depending on the operator token
            if token == '+':
                result = operand1 + operand2

            elif token == '-':
                result = operand1 - operand2

            elif token == '*':
                result = operand1 * operand2

            elif token == '/':
                # Integer division used to maintain typical postfix problem conventions
                result = int(operand1 / operand2)

            # Push computed result back onto the stack
            stack.append(result)

    # Final result remains as the only element in the stack
    return stack.pop()
```

---

## Expected Output Demonstration

```python
expression = ["2", "3", "1", "*", "+", "9", "-"]

print(evaluate_postfix(expression))
```

### Expected Output

```
-4
```

### Stepwise Evaluation

| Step | Token | Stack State         |
| ---- | ----- | ------------------- |
| 1    | 2     | `[2]`               |
| 2    | 3     | `[2, 3]`            |
| 3    | 1     | `[2, 3, 1]`         |
| 4    | *     | `[2, 3]` → push `3` |
| 5    | +     | `[5]`               |
| 6    | 9     | `[5, 9]`            |
| 7    | -     | `[-4]`              |

Final stack contains the computed result.
