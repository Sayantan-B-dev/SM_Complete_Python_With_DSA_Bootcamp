### Valid Parenthesis

```python
def is_balanced(S):
    stack = []
    m = {')': '(', ']': '[', '}': '{'}
    for c in S:
        if c in "({[":
            stack.append(c)
        else:
            if not stack or stack.pop() != m[c]:
                return False
    return len(stack) == 0
```


## Balanced Parentheses Validation Using Stack

### Conceptual Logic

A string containing only **`{ } ( ) [ ]`** characters is considered **balanced** when the following structural constraints are satisfied:

1. Every **opening bracket** must have a corresponding **closing bracket** of the same type.
2. Brackets must be **properly nested** according to stack discipline.
3. The **closing order must be the reverse order** of the opening sequence.
4. No closing bracket may appear without a previously encountered matching opening bracket.

The correct computational structure for this validation is a **stack**, because the most recent opening bracket must be closed first.
This behavior follows the **Last-In-First-Out (LIFO)** principle.

### Algorithmic Procedure

1. Initialize an empty stack.
2. Define a mapping between closing and opening brackets.
3. Iterate through each character in the string.
4. If the character is an opening bracket, push it into the stack.
5. If the character is a closing bracket:

   * If the stack is empty, the sequence is invalid.
   * Pop the top element and verify that it matches the expected opening bracket.
6. After processing all characters, the stack must be empty for the string to be balanced.

### Time and Space Complexity

| Metric           | Value                                                      |
| ---------------- | ---------------------------------------------------------- |
| Time Complexity  | `O(n)` where `n` equals string length                      |
| Space Complexity | `O(n)` worst case when all characters are opening brackets |

---

## Implementation

```python
def is_balanced(S):
    """
    Determines whether the given string containing parentheses
    and brackets is balanced according to proper nesting rules.

    Parameters
    ----------
    S : str
        Input string containing characters from the set
        '{', '}', '(', ')', '[' and ']'.

    Returns
    -------
    bool
        True if the parentheses structure is balanced.
        False if the structure violates nesting or pairing rules.
    """

    # Stack used to store opening brackets encountered during traversal
    stack = []

    # Dictionary mapping closing brackets to their corresponding opening bracket
    bracket_map = {
        ')': '(',
        ']': '[',
        '}': '{'
    }

    # Traverse every character in the string
    for character in S:

        # If the character is an opening bracket
        # push it onto the stack for later matching
        if character in "({[":
            stack.append(character)

        # If the character is a closing bracket
        elif character in ")}]":

            # If stack is empty, a closing bracket appeared without a matching opening bracket
            if not stack:
                return False

            # Pop the most recent opening bracket
            top_element = stack.pop()

            # Validate that the popped bracket corresponds to the correct opening type
            if bracket_map[character] != top_element:
                return False

    # After processing all characters, stack must be empty for balanced structure
    return len(stack) == 0
```

---

## Expected Output Demonstration

```python
print(is_balanced("{}()"))
print(is_balanced("{[()]}"))
print(is_balanced("{[(])}"))
print(is_balanced("((()))"))
```

### Expected Output

```
True
True
False
True
```

### Structural Interpretation

| Input String  | Result | Reason                               |
| ------------- | ------ | ------------------------------------ |
| `{ } ( )`     | True   | All brackets correctly paired        |
| `{ [ ( ) ] }` | True   | Proper nesting structure maintained  |
| `{ [ ( ] ) }` | False  | Incorrect closing order detected     |
| `( ( ( ) ) )` | True   | Symmetric nesting pattern maintained |
