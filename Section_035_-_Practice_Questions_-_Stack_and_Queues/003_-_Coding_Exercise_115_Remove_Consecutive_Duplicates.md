
### Remove Consecutive Duplicates

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for c in s:
            if stack and stack[-1] == c:
                stack.pop()
            else:
                stack.append(c)
        return "".join(stack)
```


## Removing Consecutive Adjacent Duplicates Using Stack

### Problem Mechanics

The objective is to repeatedly remove **two identical adjacent characters** from a string until no such pair remains.
Each removal may create new adjacent duplicates, therefore the process must continue until stabilization.

Example transformation sequence:

```
abbaca
→ remove "bb"
→ aaca
→ remove "aa"
→ ca
```

The final string is guaranteed to be **unique**, meaning that independent removal orders lead to the same result.

### Algorithmic Strategy

A **stack-based simulation** efficiently models the removal process.

Core idea:

* If the current character equals the **top element of the stack**, both represent adjacent duplicates and must be removed.
* If they differ, push the character onto the stack.
* After scanning the entire string, the stack contains the final reduced sequence.

### Why Stack Works

The stack always stores the **current valid prefix** of the processed string.

When a duplicate appears:

```
stack top == current character
```

both characters cancel out, exactly mimicking the removal rule.

### Complexity Analysis

| Metric           | Value                                      |
| ---------------- | ------------------------------------------ |
| Time Complexity  | `O(n)` each character processed once       |
| Space Complexity | `O(n)` worst case when no duplicates exist |

---

## Implementation

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        """
        Removes adjacent duplicate characters repeatedly
        until no consecutive duplicates remain.

        Parameters
        ----------
        s : str
            Input string consisting of lowercase English letters.

        Returns
        -------
        str
            Final string after all valid duplicate removals.
        """

        # Stack used to simulate the evolving string
        # Characters that survive removal remain here
        stack = []

        # Iterate through each character in the input string
        for current_character in s:

            # If stack is not empty and the top element equals
            # the current character, then we found adjacent duplicates
            if stack and stack[-1] == current_character:

                # Remove the previous occurrence because the pair cancels out
                stack.pop()

            else:
                # Otherwise this character becomes part of the valid sequence
                stack.append(current_character)

        # Convert the stack back into a string
        # because stack holds the final characters in order
        final_string = "".join(stack)

        return final_string
```

---

## Expected Output Demonstration

```python
solution = Solution()

print(solution.removeDuplicates("abbaca"))
print(solution.removeDuplicates("azxxzy"))
```

### Expected Output

```
ca
ay
```

### Stepwise Transformation Table

| Input  | Operation | Result |
| ------ | --------- | ------ |
| abbaca | remove bb | aaca   |
| aaca   | remove aa | ca     |
| azxxzy | remove xx | azzy   |
| azzy   | remove zz | ay     |
