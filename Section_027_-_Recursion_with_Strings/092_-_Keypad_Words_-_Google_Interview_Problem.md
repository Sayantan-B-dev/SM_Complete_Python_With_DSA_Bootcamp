## 1. Introduction to the Problem
On old mobile phones, each digit (2‑9) maps to a set of letters:  
- 2 → abc, 3 → def, 4 → ghi, 5 → jkl, 6 → mno, 7 → pqrs, 8 → tuv, 9 → wxyz  
(Digit 1 maps to nothing, but here we ignore it.)  
Given a string of digits, e.g., `"234"`, we want **all possible letter sequences** that can be formed by picking one letter from each digit's group.  
Example: `"23"` → `["ad","ae","af","bd","be","bf","cd","ce","cf"]`.

The provided Python code solves exactly this problem using recursion.

## 2. Code Skeleton (High‑Level View)
```python
keys = {'1':'','2':'abc','3':'def', ...}   # mapping from digit to letters

def return_all_words(input):
    # Base case: if input is empty, return a list with one empty string
    # Recursive case:
    #   1. Get all combinations for the rest of the digits (smaller input)
    #   2. For each letter of the first digit, combine it with every word from step 1
    #   3. Return the collected results
```

## 3. Base Case Explained
```python
if input == '':
    return ['']
```
Why?  
- If we have no more digits to process, there is exactly one way to form a word: the empty word.  
- This empty string acts as a **building block** – later we will prefix letters to it.  
- Without this base case, recursion would never stop.

## 4. Recursive Call (The Heart)
```python
smallInput = input[1:]                 # remove the first digit
smallInputWords = return_all_words(smallInput)   # get words for the rest
```
- `smallInput` is the original string without its first character.  
- `return_all_words(smallInput)` is a **recursive call** – the function calls itself with a shorter string.  
- It trusts that this call will correctly return **all letter combinations for the shorter digit string**.

## 5. Building the Answer
```python
keyLetter = keys[input[0]]   # e.g., for '2' → "abc"

for myChar in keyLetter:          # for each letter of the current digit
    for word in smallInputWords:  # for each word from the recursive result
        ans.append(myChar + word) # combine and add to answer
```
- We take the first digit, get its possible letters.  
- We take every word that can be formed from the **remaining digits** (`smallInputWords`).  
- By prefixing the current letter to each of those words, we build all combinations that start with that letter.  
- Repeating for every letter of the current digit gives us **all combinations** for the whole digit string.

## 6. Recursion Tree (Visualising for "23")
Let's see how the recursion unfolds for `input = "23"`.

```
return_all_words("23")
├── smallInput = "3"
├── smallInputWords = return_all_words("3")
│   ├── smallInput = ""
│   ├── smallInputWords = return_all_words("") → [""]
│   ├── keyLetter = keys['3'] = "def"
│   ├── for 'd': word in [""] → "d" + "" = "d"
│   ├── for 'e': word in [""] → "e"
│   ├── for 'f': word in [""] → "f"
│   └── return ["d","e","f"]
│
├── keyLetter = keys['2'] = "abc"
├── for 'a': word in ["d","e","f"] → "ad","ae","af"
├── for 'b': word in ["d","e","f"] → "bd","be","bf"
├── for 'c': word in ["d","e","f"] → "cd","ce","cf"
└── return ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

Each box is a function call, and the arrows show how results flow upwards.

## 7. Step‑by‑Step Working (Trace for "23")
Let's simulate `return_all_words('23')` manually.

1. **Call** `return_all_words('23')`  
   - `input` = `'23'`, not empty → go to recursive case.  
   - `smallInput` = `'23'[1:]` = `'3'`.  
   - Call `return_all_words('3')` and wait for its result.

2. **Call** `return_all_words('3')`  
   - `input` = `'3'`, not empty.  
   - `smallInput` = `'3'[1:]` = `''`.  
   - Call `return_all_words('')`.

3. **Call** `return_all_words('')`  
   - `input` = `''`, so base case returns `['']`.  

4. Back to `return_all_words('3')`  
   - `smallInputWords` = `['']`.  
   - `keyLetter` = `keys['3']` = `'def'`.  
   - `ans` starts as empty list `[]`.  
   - Outer loop `myChar` in `'def'`:  
     - `myChar = 'd'`: inner loop over `['']` → append `'d' + ''` = `'d'`.  
     - `myChar = 'e'`: append `'e'`.  
     - `myChar = 'f'`: append `'f'`.  
   - Return `['d','e','f']`.

5. Back to `return_all_words('23')`  
   - `smallInputWords` = `['d','e','f']`.  
   - `keyLetter` = `keys['2']` = `'abc'`.  
   - `ans` starts empty.  
   - Outer loop `myChar` in `'abc'`:  
     - `myChar = 'a'`: inner loop over `['d','e','f']` → append `'ad','ae','af'`.  
     - `myChar = 'b'`: append `'bd','be','bf'`.  
     - `myChar = 'c'`: append `'cd','ce','cf'`.  
   - Return `['ad','ae','af','bd','be','bf','cd','ce','cf']`.

Done. The result is exactly the 9 combinations we expect.

## 8. The Algorithm
This is a classic **recursive backtracking** (or combination generation) algorithm.  
- It breaks the problem into a smaller version of itself (the remaining digits).  
- It combines the solutions of the smaller problem with each possible choice for the current digit.  
- The base case handles the empty input, providing the starting point (the empty string).

Time complexity: O(4ⁿ) in worst case (because digit 7 and 9 have 4 letters), where n is the number of digits.

## 9. Role of Loops
- **Outer loop** (`for myChar in keyLetter`): iterates over all letters the current digit can be.  
- **Inner loop** (`for word in smallInputWords`): iterates over all words that can be formed from the remaining digits.  
- Together they create every combination by attaching the current letter to the front of every suffix word.

## 10. Full Code with Detailed Comments
```python
# Mapping from digit to possible letters (as strings)
keys = {
    '1': '',      # digit 1 has no letters (ignored)
    '2': 'abc',
    '3': 'def',
    '4': 'ghi',
    '5': 'jkl',
    '6': 'mno',   # Note: original code had '6':'ghi' – that's a typo! Should be 'mno'.
    '7': 'pqrs',
    '8': 'tuv',
    '9': 'wxyz'
}

def return_all_words(input):
    """
    Returns a list of all possible letter combinations for the given digit string.
    For example, input='23' → ['ad','ae','af','bd','be','bf','cd','ce','cf'].
    """
    # Base case: if there are no digits left, the only combination is an empty string.
    if input == '':
        return ['']

    # Recursive case:
    # 1. Get all words for the remaining digits (everything except the first).
    smallInput = input[1:]                     # e.g., '23' → '3'
    smallInputWords = return_all_words(smallInput)  # Recursive call

    # 2. Get the letters corresponding to the first digit.
    firstDigit = input[0]                       # e.g., '2'
    possibleLetters = keys[firstDigit]           # e.g., 'abc'

    # 3. Build the answer by combining each letter of the first digit
    #    with each word from the recursive result.
    ans = []                                     # will hold all combinations
    for letter in possibleLetters:               # try each letter for first digit
        for word in smallInputWords:              # for each suffix word
            ans.append(letter + word)             # combine and add to answer

    # 4. Return the complete list of combinations.
    return ans


# Example usage:
result = return_all_words('2345')
print(result)   # prints all possible letter combinations for 2345
```

**Note**: The original `keys` dictionary had `'6':'ghi'` – that's incorrect (digit 6 should map to "mno"). I've corrected it in the comments. If you keep the original, digit 6 would give the same letters as digit 4, which is not the intended phone mapping.

## 11. Explain Like I'm 5 (ELI5)
Imagine you have a row of toy boxes.  
- The first box has an `'a'`, a `'b'`, and a `'c'`.  
- The second box has a `'d'`, an `'e'`, and an `'f'`.  
- The third box has a `'g'`, an `'h'`, and an `'i'`.  

You want to make **all possible words** by taking one toy from each box, in order.  
The function works like this:  
- First, it asks: *If there were no boxes, how many words could we make?* Answer: one word – the empty word.  
- Then it says: *If we have one box, we can take each toy from that box and attach it to the empty word.* So we get `'a'`, `'b'`, `'c'`.  
- For two boxes, it first finds all words from the **second box onwards** (the ones that come from the remaining boxes). That gives `'d'`, `'e'`, `'f'`. Then it takes each toy from the first box (`'a'`, `'b'`, `'c'`) and puts it in front of **each** of those words. So `'a'` + `'d'` = `'ad'`, `'a'` + `'e'` = `'ae'`, `'a'` + `'f'` = `'af'`, and so on.  

That's exactly what the recursion does – it keeps asking "what are the words for the rest of the boxes?" and then builds the final words by adding the first box's toys at the front.

---

