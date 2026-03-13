## 1. Introduction
Imagine you have a secret message where each letter is replaced by its position in the alphabet: a=1, b=2, …, z=26.  
If someone gives you a string like `"123"`, it could be decoded as:
- `1 2 3` → a b c → `"abc"`
- `1 23` → a w → `"aw"`
- `12 3` → l c → `"lc"`
So the answer should be `["abc", "aw", "lc"]`.  
The provided code generates **all possible decodings** using recursion.

## 2. Code Skeleton (High‑Level View)
```python
def get_char(value):                # helper: number -> letter (1->a, 2->b, ...)
    if value <= 0 or value > 26:
        return ""
    return chr(97 + value - 1)       # chr(97)='a', so 1->a, 2->b, etc.

def return_all_codes(input):         # input is a string of digits
    # Base cases: empty string or single digit
    # Recursive case:
    #   - take one digit, decode it, combine with decodings of the rest
    #   - if the first two digits form a valid number (10-26), also take them and combine
    #   return all combinations
```

## 3. Base Cases Explained
```python
if input == '':
    return ['']
```
- If there are no digits left, there is exactly one way to form a word: the empty word. This empty string is the seed for building longer words.

```python
if len(input) == 1:
    singleChar = get_char(int(input))
    return [singleChar]
```
- When only one digit remains, it must be decoded as a single letter (if valid). The helper `get_char` maps the digit to a letter.  
  (Note: For input like `"0"`, `get_char(0)` returns `""`, but that case shouldn't happen if input is well‑formed; we'll assume valid digits.)

These base cases stop the recursion.

## 4. Recursive Call (The Heart)
```python
singleDigit = int(input[0:1])          # first digit as integer
doubleDigit = int(input[0:2])          # first two digits as integer

ansWithoutFirstDigit = return_all_codes(input[1:])   # decodings for rest after 1 digit
```
- First, we compute all decodings for the string **without the first digit** (`input[1:]`).  
- Similarly, if valid, we later compute decodings for the string **without the first two digits** (`input[2:]`).

The recursion “trusts” that these calls return the correct lists of words for the shorter strings.

## 5. Building the Answer
```python
mainAns = []

# Case 1: use the first digit as a single letter
for eachAns in ansWithoutFirstDigit:
    mainAns.append(get_char(singleDigit) + eachAns)

# Case 2: if the first two digits form a valid number (10-26)
if doubleDigit >= 10 and doubleDigit <= 26:
    ansWithoutDoubleDigit = return_all_codes(input[2:])
    for eachAns in ansWithoutDoubleDigit:
        mainAns.append(get_char(doubleDigit) + eachAns)
```
- **Case 1**: We take the first digit, decode it to a letter, and prepend that letter to **every** word that can be formed from the remaining digits (`input[1:]`).  
- **Case 2**: If the first two digits are between 10 and 26, we also consider them as a single letter. We then prepend that letter to **every** word from the remaining digits (`input[2:]`).  
- Both sets of results are collected in `mainAns`.

## 6. Recursion Tree (Visualising for "123")
Let's draw the recursion tree for `"123"`.

```
return_all_codes("123")
├── singleDigit=1, doubleDigit=12 (valid)
├── ansWithoutFirstDigit = return_all_codes("23")
│   ├── singleDigit=2, doubleDigit=23 (valid)
│   ├── ansWithoutFirstDigit = return_all_codes("3")
│   │   ├── len==1 → ['c']   (3->c)
│   │   └── (no double because only one digit)
│   ├── case 1: prepend 'b' (2->b) to ['c'] → ["bc"]
│   ├── case 2: doubleDigit=23→'w', ansWithoutDoubleDigit = return_all_codes("") → ['']
│   │          prepend 'w' to [''] → ["w"]
│   └── return ["bc","w"]
│
├── case 1: prepend 'a' (1->a) to ["bc","w"] → ["abc","aw"]
│
├── case 2: doubleDigit=12→'l', ansWithoutDoubleDigit = return_all_codes("3")
│   │   (already computed above) returns ['c']
│   │   prepend 'l' to ['c'] → ["lc"]
│
└── return ["abc","aw","lc"]
```

Each box shows a call and its returned list. The arrows indicate how results flow upward.

## 7. Step‑by‑Step Working (Trace for "123")
Let's simulate the function calls manually:

1. **Call** `return_all_codes("123")`  
   - `singleDigit = 1`, `doubleDigit = 12` (valid).  
   - Need `ansWithoutFirstDigit` = result of `return_all_codes("23")`.

2. **Call** `return_all_codes("23")`  
   - `singleDigit = 2`, `doubleDigit = 23` (valid).  
   - Need `ansWithoutFirstDigit` = result of `return_all_codes("3")`.

3. **Call** `return_all_codes("3")`  
   - `len(input) == 1`, so base case: `get_char(3) = 'c'`. Return `["c"]`.

4. Back to `return_all_codes("23")`  
   - `ansWithoutFirstDigit = ["c"]`.  
   - Case 1: prepend `get_char(2)='b'` to each in `["c"]` → `["bc"]`.  
   - Case 2: `doubleDigit=23` valid → `get_char(23)='w'`. Need `ansWithoutDoubleDigit` = `return_all_codes("")`.

5. **Call** `return_all_codes("")`  
   - Base case returns `[""]`.

6. Back to case 2 of `return_all_codes("23")`  
   - `ansWithoutDoubleDigit = [""]`.  
   - Prepend `'w'` → `["w"]`.  
   - Combine both cases: `["bc"] + ["w"]` = `["bc","w"]`. Return this list.

7. Back to `return_all_codes("123")`  
   - `ansWithoutFirstDigit = ["bc","w"]`.  
   - Case 1: prepend `'a'` → `["abc","aw"]`.  
   - Case 2: `doubleDigit=12` valid → `'l'`. Need `ansWithoutDoubleDigit` = `return_all_codes("3")`. We already know that call returns `["c"]`.  
   - Prepend `'l'` → `["lc"]`.  
   - Combine: `["abc","aw"] + ["lc"]` = `["abc","aw","lc"]`.

Result: exactly the three expected decodings.

## 8. The Algorithm
This is a **recursive depth‑first search** (or backtracking) that explores all valid groupings of digits. At each step:
- It considers taking one digit (always valid if digit is 1‑9).
- It optionally considers taking two digits if they form a number between 10 and 26.
- It recursively solves the remaining substring and combines the results.

**Time complexity**: In the worst case (e.g., all digits are 1 or 2, allowing many splits), the number of decodings grows like the Fibonacci sequence, roughly O(1.6ⁿ). For a string of length n, each recursion can branch twice, but not all branches are valid. The total number of leaf nodes equals the number of decodings.

## 9. Use of Loops
- The loops `for eachAns in ansWithoutFirstDigit` and `for eachAns in ansWithoutDoubleDigit` iterate over all possible suffix decodings returned from the recursive calls.  
- They simply prepend the current letter to each suffix word and collect the results. This is the **combining step** that builds the full words.

## 10. Full Code with Detailed Comments
```python
def get_char(value):
    """
    Convert a number (1-26) to the corresponding lowercase letter.
    Returns empty string if value is out of range.
    """
    if value <= 0 or value > 26:
        return ""
    # chr(97) = 'a', so value 1 -> 'a', 2 -> 'b', ..., 26 -> 'z'
    return chr(97 + value - 1)


def return_all_codes(input):
    """
    Return all possible letter decodings of the digit string `input`.
    Each digit (1-9) maps to a-i, and each two-digit number (10-26) maps to j-z.
    """
    # Base case: empty string → one possibility: the empty string
    if input == '':
        return ['']

    # Base case: single digit → decode it directly
    if len(input) == 1:
        singleChar = get_char(int(input))
        return [singleChar]

    # First digit and first two digits as integers
    singleDigit = int(input[0:1])       # e.g., "123" → 1
    doubleDigit = int(input[0:2])       # e.g., "123" → 12

    # This list will hold all decodings for the current prefix
    mainAns = []

    # --- Case 1: Use only the first digit ---
    # Get all decodings for the string after removing the first digit
    ansWithoutFirstDigit = return_all_codes(input[1:])
    # For each suffix decoding, prepend the letter of the first digit
    for eachAns in ansWithoutFirstDigit:
        mainAns.append(get_char(singleDigit) + eachAns)

    # --- Case 2: Use the first two digits, if they form a valid number (10-26) ---
    if doubleDigit >= 10 and doubleDigit <= 26:
        # Get all decodings for the string after removing the first two digits
        ansWithoutDoubleDigit = return_all_codes(input[2:])
        # For each suffix decoding, prepend the letter of the two-digit number
        for eachAns in ansWithoutDoubleDigit:
            mainAns.append(get_char(doubleDigit) + eachAns)

    # Return all combinations found
    return mainAns


# Example usage
result = return_all_codes('123')
print(result)   # Output: ['abc', 'aw', 'lc']
```

## 11. Explain Like I'm 5 (ELI5)
Imagine you have a secret code where numbers stand for letters:  
- 1 = a, 2 = b, 3 = c, … 9 = i  
- 10 = j, 11 = k, … 26 = z  

Now you get a string of numbers, like "123".  
How many different secret words could it be?  
- You can read it as **one number at a time**: 1, 2, 3 → a, b, c → "abc".  
- Or you can sometimes read **two numbers together** if they make a number from 10 to 26: 12, 3 → l, c → "lc".  
- Or 1, 23 → a, w → "aw".  

The computer solves it by thinking:  
- "If I had no numbers, the only word is an empty word."  
- "If I have one number, I just write its letter."  
- "If I have many numbers, I look at the first one. I ask the computer: *What are all the words for the rest of the numbers?* Then I put the first number's letter in front of each of those words.  
- "I also check if the first two numbers together make a valid letter (10‑26). If yes, I ask the computer: *What are all the words for the numbers after these two?* Then I put that two‑number letter in front of each of those words."  

By doing this again and again, the computer collects **every possible secret word** for the whole string. It’s like building with LEGO: first you build all the smaller pieces (the endings), then you attach the front piece to each of them.

---

