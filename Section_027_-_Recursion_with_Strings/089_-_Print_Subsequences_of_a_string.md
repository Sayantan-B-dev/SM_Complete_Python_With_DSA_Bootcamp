Below is a step‑by‑step visualization of the recursive function `print_subsequences`. This function prints **all subsequences** of a given string by making two recursive calls at each step: one that **includes** the current character and one that **excludes** it. When the remaining string becomes empty, it prints the subsequence built so far.

## Function Recap

```python
def print_subsequences(s1, takenSoFar):
    # Base case: if the remaining string is empty, print the accumulated subsequence
    if s1 == '':
        print(takenSoFar)
        return

    currentChar = s1[0]          # first character of the remaining string
    smallInput = s1[1:]           # rest of the string (without the first char)

    # Recursive call 1: include the current character
    print_subsequences(smallInput, takenSoFar + currentChar)

    # Recursive call 2: exclude the current character
    print_subsequences(smallInput, takenSoFar)

    return
```

When we call `print_subsequences("abc", "")`, the function explores every combination of including or excluding each character. The result is all 2³ = 8 subsequences printed (including the empty string).

---

## Recursion Tree for `print_subsequences("abc", "")`

I’ll represent each call as a node with parameters `(s1, takenSoFar)`. The tree shows the order of calls (depth‑first) and the point where each subsequence is printed.

```
                            ( "abc", "" )
                           /             \
               include 'a' /               \ exclude 'a'
                         /                   \
              ( "bc", "a" )                 ( "bc", "" )
               /         \                   /         \
      inc 'b' /           \ exc 'b'  inc 'b' /           \ exc 'b'
            /               \               /               \
 ( "c", "ab" )          ( "c", "a" )  ( "c", "b" )        ( "c", "" )
    /      \              /      \       /      \          /      \
 inc/        \ exc    inc/        \ exc inc/        \ exc inc/        \ exc
  /          \        /          \     /          \    /          \
("","abc")  ("","ab") ("","ac")  ("","a") ("","bc") ("","b") ("","c")  ("","")
   |           |         |          |        |        |       |         |
 print       print     print      print    print    print   print     print
"abc"        "ab"      "ac"       "a"      "bc"     "b"     "c"       ""
```

### How the Tree is Traversed

The function uses **depth‑first recursion** – it fully explores the left subtree (include branch) before moving to the right subtree (exclude branch). The printed output order reflects this traversal:

1. `("abc")` from the path: include 'a', include 'b', include 'c' → print `"abc"`
2. Backtrack to `("c","ab")` and then take exclude 'c' → print `"ab"`
3. Backtrack to `("bc","a")` and take exclude 'b', then include 'c' → print `"ac"`
4. From same node, exclude 'b', exclude 'c' → print `"a"`
5. Backtrack to root, take exclude 'a', then from `("bc","")` include 'b', include 'c' → print `"bc"`
6. From `("c","b")` exclude 'c' → print `"b"`
7. From `("bc","")` exclude 'b', include 'c' → print `"c"`
8. Exclude 'b', exclude 'c' → print `""` (empty string)

Thus the output printed is:
```
abc
ab
ac
a
bc
b
c
(empty line)
```

> Note: The empty string prints as a blank line. In a console you might see an empty line or `''` depending on the print statement.

---

## Step‑by‑Step Walkthrough (with call stack)

Let’s trace the first few calls manually to see the recursion in action.

**Initial call**: `print_subsequences("abc", "")`

- `s1 = "abc"`, `takenSoFar = ""`
- `currentChar = 'a'`, `smallInput = "bc"`
- First recursive call (include): `print_subsequences("bc", "a")`  
  *This call will be explored completely before the second (exclude) call is made.*

**Call 2**: `print_subsequences("bc", "a")`

- `s1 = "bc"`, `takenSoFar = "a"`
- `currentChar = 'b'`, `smallInput = "c"`
- First recursive call (include): `print_subsequences("c", "ab")`

**Call 3**: `print_subsequences("c", "ab")`

- `s1 = "c"`, `takenSoFar = "ab"`
- `currentChar = 'c'`, `smallInput = ""`
- First recursive call (include): `print_subsequences("", "abc")`

**Call 4**: `print_subsequences("", "abc")`

- Base case: `s1 == ""` → print `"abc"` and return to Call 3.

**Back to Call 3**: now the second recursive call (exclude) is executed:  
`print_subsequences("", "ab")`

**Call 5**: `print_subsequences("", "ab")`

- Base case: print `"ab"` and return to Call 3.

**Call 3 finishes**, returns to Call 2.

**Call 2** now executes its second recursive call (exclude `'b'`):  
`print_subsequences("c", "a")`

**Call 6**: `print_subsequences("c", "a")`

- `s1 = "c"`, `takenSoFar = "a"`
- Include `'c'`: `print_subsequences("", "ac")` → prints `"ac"`
- Exclude `'c'`: `print_subsequences("", "a")` → prints `"a"`
- Returns to Call 2.

**Call 2 finishes**, returns to initial call.

**Initial call** now executes its second recursive call (exclude `'a'`):  
`print_subsequences("bc", "")`

... and so on, following the pattern until all 8 subsequences are printed.

---

## Why This Works

- **Base case** triggers when no characters are left to process – the `takenSoFar` string at that moment is a complete subsequence of the original string.
- **Include branch** builds subsequences that contain the current character.
- **Exclude branch** builds subsequences that do not contain the current character.
- By making both calls for every character, we enumerate **all** possibilities.

---

## Complexity

- **Time complexity**: O(2ⁿ) – each of the 2ⁿ subsequences is printed exactly once, and each recursive call does O(1) work (string concatenation in `takenSoFar + currentChar` is O(k) where k is the current length, but overall it sums to O(n·2ⁿ) if we consider copying; however, printing itself is also O(n) per subsequence, so total O(n·2ⁿ)).
- **Space complexity**: O(n) for the recursion stack (depth n) plus O(n) for storing `takenSoFar` along each path (implicitly on the stack).