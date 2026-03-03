# Functions Examples: Practical Applications

## Example 1: Temperature Conversion

### 1. Definition and Concept Overview
The function `convert_temperature(temp, unit)` converts a temperature value between Celsius and Fahrenheit. It takes a numeric temperature and a unit specifier (`'C'` for Celsius input, `'F'` for Fahrenheit input) and returns the converted temperature in the opposite scale.

### 2. Core Principles and Internal Mechanics
- **Mathematical conversion**: Uses the standard formulas:
  - Celsius to Fahrenheit: `(temp * 9/5) + 32`
  - Fahrenheit to Celsius: `(temp - 32) * 5/9`
- **Conditional branching**: The `unit` parameter determines which formula to apply.
- **Return value**: Returns the converted temperature as a float, or `None` if an invalid unit is provided.

### 3. Step-by-Step Logical Breakdown
1. Receive `temp` (numeric) and `unit` (string).
2. If `unit == 'C'`, compute Fahrenheit using the Celsius-to-Fahrenheit formula and return result.
3. Else if `unit == 'F'`, compute Celsius using the Fahrenheit-to-Celsius formula and return result.
4. Else, return `None` (invalid unit).

### 4. Implementation (Python) with meaningful inline comments
```python
def convert_temperature(temp, unit):
    """Convert temperature between Celsius and Fahrenheit."""
    # Celsius to Fahrenheit conversion
    if unit == 'C':
        return temp * 9/5 + 32
    # Fahrenheit to Celsius conversion
    elif unit == 'F':
        return (temp - 32) * 5/9
    # Invalid unit handling
    else:
        return None

# Example calls
print(convert_temperature(25, 'C'))   # 77.0
print(convert_temperature(77, 'F'))   # 25.0
```

### 5. Time and Space Complexity Analysis
- **Time complexity**: O(1) – constant time arithmetic operations.
- **Space complexity**: O(1) – uses a fixed number of variables.

### 6. Edge Cases and Failure Scenarios
- **Invalid unit**: Any string other than `'C'` or `'F'` (case‑sensitive) returns `None`. The function does not handle lowercase `'c'` or `'f'`.
- **Non‑numeric temperature**: Passing a non‑numeric value (e.g., string) raises `TypeError` during arithmetic.
- **Extreme values**: Works for any float; precision is limited by floating‑point representation.

### 7. Practical Use Cases
- **User‑facing applications** that require temperature input in one scale and display in another.
- **Data preprocessing** when working with datasets containing mixed temperature units.

### 8. Limitations and Trade-offs
- **Case sensitivity**: Does not accept lowercase `'c'` or `'f'`; could be improved with `.upper()`.
- **No support for Kelvin** or other scales without modification.
- **Returns `None` for invalid input**, which may require caller to check for `None` explicitly.

---

## Example 2: Password Strength Checker

### 1. Definition and Concept Overview
The function `is_strong_password(password)` evaluates whether a given password meets common strength criteria: minimum length, inclusion of digits, lowercase letters, uppercase letters, and special characters. Returns `True` if all criteria are satisfied, otherwise `False`.

### 2. Core Principles and Internal Mechanics
- **Length check**: Verifies that the password has at least 8 characters.
- **Character class checks**: Uses `any()` with generator expressions to test for presence of at least one digit, one lowercase letter, one uppercase letter, and one special character from a predefined set.
- **Short-circuit evaluation**: The function returns `False` as soon as any condition fails, avoiding unnecessary checks.

### 3. Step-by-Step Logical Breakdown
1. If `len(password) < 8`, return `False`.
2. If no digit exists in password, return `False`.
3. If no lowercase letter exists, return `False`.
4. If no uppercase letter exists, return `False`.
5. If no special character from `'!@#$%^&*()_+'` exists, return `False`.
6. If all checks pass, return `True`.

### 4. Implementation (Python) with meaningful inline comments
```python
def is_strong_password(password):
    """Return True if password meets strength criteria."""
    # Minimum length requirement
    if len(password) < 8:
        return False
    # At least one digit
    if not any(char.isdigit() for char in password):
        return False
    # At least one lowercase letter
    if not any(char.islower() for char in password):
        return False
    # At least one uppercase letter
    if not any(char.isupper() for char in password):
        return False
    # At least one special character from given set
    special_chars = '!@#$%^&*()_+'
    if not any(char in special_chars for char in password):
        return False
    return True

# Example calls
print(is_strong_password("WeakPwd"))        # False
print(is_strong_password("Str0ngPwd!"))     # True
```

### 5. Time and Space Complexity Analysis
- **Time complexity**: O(n) where n is the length of the password. Each `any()` call scans the string until a match is found; in the worst case the entire string is scanned for each check.
- **Space complexity**: O(1) – no additional data structures proportional to input size.

### 6. Edge Cases and Failure Scenarios
- **Empty password**: `len(password) == 0` fails length check, returns `False`.
- **Password containing only digits**: Fails lowercase/uppercase/special checks.
- **Unicode characters**: The methods `.isdigit()`, `.islower()`, `.isupper()` work with Unicode but may have surprising results for some scripts (e.g., certain numerals considered digits).
- **Special character set defined explicitly**: Characters like `<` or `}` are not considered special; function may reject passwords using other special characters.

### 7. Practical Use Cases
- **User registration systems** to enforce strong password policies.
- **Security audit tools** that check password strength offline.

### 8. Limitations and Trade-offs
- **Fixed criteria**: Does not allow configuration of length or character sets; hardcoded.
- **No dictionary checks**: Does not prevent common passwords or dictionary words.
- **Character class methods**: `.islower()` and `.isupper()` return `False` for non‑letter characters, but that’s correct for this logic.
- **Special character list may not match all systems’ requirements**.

---

## Example 3: Calculate Total Cost of Items in a Shopping Cart

### 1. Definition and Concept Overview
`calculate_total_cost(cart)` computes the sum of `price * quantity` for each item in a shopping cart represented as a list of dictionaries. Each dictionary contains keys `'price'` (float) and `'quantity'` (int or float). Returns the total cost as a float.

### 2. Core Principles and Internal Mechanics
- **Iteration**: Loops over each item in the cart list.
- **Accumulation**: Multiplies each item’s price by its quantity and adds to a running total.
- **Assumes correct structure**: Expects each item to be a dict with the required keys.

### 3. Step-by-Step Logical Breakdown
1. Initialize `total_cost = 0`.
2. For each `item` in `cart`:
   - Compute `item['price'] * item['quantity']`.
   - Add result to `total_cost`.
3. Return `total_cost`.

### 4. Implementation (Python) with meaningful inline comments
```python
def calculate_total_cost(cart):
    """Calculate total cost of items in cart."""
    total_cost = 0
    for item in cart:
        # Multiply price by quantity and accumulate
        total_cost += item['price'] * item['quantity']
    return total_cost

# Example cart data
cart = [
    {'name': 'Apple', 'price': 0.5, 'quantity': 4},
    {'name': 'Banana', 'price': 0.3, 'quantity': 6},
    {'name': 'Orange', 'price': 0.7, 'quantity': 3}
]

total = calculate_total_cost(cart)
print(total)  # 5.9
```

### 5. Time and Space Complexity Analysis
- **Time complexity**: O(n) where n is the number of items in the cart.
- **Space complexity**: O(1) – only a single accumulator variable.

### 6. Edge Cases and Failure Scenarios
- **Empty cart**: Returns 0 (loop runs zero times).
- **Missing keys**: If an item lacks `'price'` or `'quantity'`, raises `KeyError`.
- **Non‑numeric values**: If price or quantity are not numbers, may raise `TypeError` during multiplication.
- **Negative quantities**: Allowed; total cost may be negative (possible for returns/discounts).

### 7. Practical Use Cases
- **E‑commerce checkout** to compute order total.
- **Inventory management** to calculate total value of stock.

### 8. Limitations and Trade-offs
- **No input validation**: Assumes well‑formed data; production code would need error handling.
- **No support for discounts or taxes**; a more sophisticated function would need additional parameters.
- **Floating‑point precision**: Summing many floating‑point numbers may lead to rounding errors; use `Decimal` for monetary values.

---

## Example 4: Check If a String Is Palindrome

### 1. Definition and Concept Overview
`is_palindrome(s)` determines whether a given string reads the same forwards and backwards, ignoring case and spaces. Returns `True` if it is a palindrome, otherwise `False`.

### 2. Core Principles and Internal Mechanics
- **Normalization**: Converts the string to lowercase and removes spaces.
- **Reversal comparison**: Uses slicing `[::-1]` to reverse the normalized string and compares with the original normalized string.
- **Efficiency**: Slicing creates a new reversed string; comparison is character‑by‑character.

### 3. Step-by-Step Logical Breakdown
1. Convert input string `s` to lowercase using `.lower()`.
2. Remove all spaces using `.replace(" ", "")`.
3. Compare the normalized string with its reverse (`normalized[::-1]`).
4. Return `True` if they are equal, else `False`.

### 4. Implementation (Python) with meaningful inline comments
```python
def is_palindrome(s):
    """Return True if s is a palindrome (case‑ and space‑insensitive)."""
    # Normalize: lowercase and remove spaces
    normalized = s.lower().replace(" ", "")
    # Compare with reversed version
    return normalized == normalized[::-1]

# Example calls
print(is_palindrome("A man a plan a canal Panama"))  # True
print(is_palindrome("Hello"))                         # False
```

### 5. Time and Space Complexity Analysis
- **Time complexity**: O(n) where n is the length of the original string. Lowercasing, replacing spaces, and slicing each take O(n).
- **Space complexity**: O(n) – creates a new normalized string and a reversed copy. Could be optimized to O(1) with two‑pointer technique, but this implementation prioritizes readability.

### 6. Edge Cases and Failure Scenarios
- **Empty string**: `""` after normalization remains empty, and empty string equals its reverse → returns `True` (considered palindrome by definition).
- **String with only spaces**: After replacing spaces, becomes empty → `True`.
- **Punctuation**: The function does not remove punctuation; `"race a car"` becomes `"raceacar"` which is not a palindrome. This may be desired or not depending on definition.
- **Unicode**: `.lower()` works for Unicode, but case folding may be needed for full correctness.

### 7. Practical Use Cases
- **Word games** (e.g., checking if a word is a palindrome).
- **Data validation** (e.g., verifying palindromic identifiers).

### 8. Limitations and Trade-offs
- **Ignores only spaces**: Does not ignore punctuation or other whitespace (tabs, newlines). Use regex for more comprehensive cleanup.
- **Space overhead**: Creates two copies; for very large strings, memory may be a concern. An iterative two‑pointer solution avoids this.

---

## Example 5: Calculate Factorial of a Number Using Recursion

### 1. Definition and Concept Overview
`factorial(n)` computes the factorial of a non‑negative integer `n` using recursion. The factorial of n (denoted n!) is the product of all positive integers less than or equal to n, with 0! defined as 1.

### 2. Core Principles and Internal Mechanics
- **Base case**: If `n == 0`, return 1 (terminates recursion).
- **Recursive case**: Return `n * factorial(n-1)`. This decomposes the problem into smaller subproblems.
- **Stack usage**: Each recursive call adds a frame to the call stack; the depth is proportional to n.

### 3. Step-by-Step Logical Breakdown
1. If `n == 0`, return 1.
2. Otherwise, compute `n * factorial(n-1)` and return the result.

### 4. Implementation (Python) with meaningful inline comments
```python
def factorial(n):
    """Return n! using recursion."""
    # Base case: 0! = 1
    if n == 0:
        return 1
    # Recursive case: n! = n * (n-1)!
    else:
        return n * factorial(n - 1)

print(factorial(6))  # 720
```

### 5. Time and Space Complexity Analysis
- **Time complexity**: O(n) – there are n recursive calls, each doing O(1) work.
- **Space complexity**: O(n) – due to the call stack depth. Each call consumes stack space.

### 6. Edge Cases and Failure Scenarios
- **Negative input**: The function does not handle negative numbers; calling with negative n leads to infinite recursion (no base case) and eventually `RecursionError`.
- **Large n**: Python’s recursion limit (default ~1000) will be exceeded for n > ~1000, raising `RecursionError`. Factorials for such large n produce enormous integers (Python supports big integers) but recursion depth becomes the limiting factor.
- **Non‑integer input**: Passing a float (e.g., 5.5) will cause recursion with fractional values, never reaching base case; may lead to infinite recursion or `RecursionError` after many steps (since subtraction continues indefinitely). The function should validate input type.

### 7. Practical Use Cases
- **Mathematical computations** (combinations, permutations).
- **Educational examples** for teaching recursion.

### 8. Limitations and Trade-offs
- **Recursion depth limit**: Not suitable for large n; iterative solution avoids stack overflow and uses O(1) space.
- **No input validation**: Should include type and range checks for robustness.
- **Performance**: Recursive calls have overhead compared to iterative loops.

---

## Example 6: Read a File and Count Word Frequency

### 1. Definition and Concept Overview
`count_word_frequency(file_path)` reads a text file, splits it into words, normalizes each word (lowercase, strips punctuation), and counts occurrences. Returns a dictionary mapping each word to its frequency.

### 2. Core Principles and Internal Mechanics
- **File I/O**: Opens the file using a context manager (`with` statement) ensuring proper closure.
- **Line‑by‑line reading**: Iterates over lines to handle large files without loading entire content into memory.
- **Word splitting**: Splits each line on whitespace using `.split()`.
- **Normalization**: Converts each word to lowercase and strips common punctuation characters.
- **Counting**: Uses dictionary `get(word, 0) + 1` idiom to increment counts.

### 3. Step-by-Step Logical Breakdown
1. Initialize empty dictionary `word_count`.
2. Open file at `file_path` in read mode.
3. For each line in file:
   - Split line into words (list).
   - For each word:
     - Convert to lowercase.
     - Strip punctuation characters `.,!?;:"'`.
     - Update count: `word_count[word] = word_count.get(word, 0) + 1`.
4. Return `word_count`.

### 4. Implementation (Python) with meaningful inline comments
```python
def count_word_frequency(file_path):
    """Count frequency of each word in the specified file."""
    word_count = {}
    # Open file safely
    with open(file_path, 'r') as file:
        for line in file:
            # Split line into words
            words = line.split()
            for word in words:
                # Normalize: lowercase and strip punctuation
                word = word.lower().strip('.,!?;:"\'')
                # Update count
                word_count[word] = word_count.get(word, 0) + 1
    return word_count

# Example usage (assumes sample.txt exists)
filepath = 'sample.txt'
word_frequency = count_word_frequency(filepath)
print(word_frequency)
```

### 5. Time and Space Complexity Analysis
- **Time complexity**: O(T) where T is total number of words in the file. Each word is processed once.
- **Space complexity**: O(U) where U is the number of unique words. The dictionary stores each unique word and its count.

### 6. Edge Cases and Failure Scenarios
- **File not found**: `open()` raises `FileNotFoundError`. The function does not handle it; caller must manage exceptions.
- **Empty file**: Returns empty dictionary.
- **Words with hyphen or apostrophe**: The simple strip may remove intended punctuation; e.g., `"can't"` becomes `"can't"` if apostrophe not stripped, but the strip set includes `'` so it would become `"cant"`. This may be undesirable.
- **Punctuation attached to words**: Words like `"hello,"` become `"hello"` after stripping trailing comma – correct. But words like `"well-being"` have hyphen inside; stripping hyphens would break it.
- **Very large files**: Line‑by‑line reading prevents memory overload, but the dictionary may become large.

### 7. Practical Use Cases
- **Text analysis** (e.g., word clouds, frequency distributions).
- **Search engine indexing** (building inverted indices).
- **Plagiarism detection** (comparing word frequency profiles).

### 8. Limitations and Trade-offs
- **Punctuation handling**: Naive stripping may not handle all cases (e.g., contractions, hyphenated words). A more robust solution might use regular expressions or tokenization libraries.
- **Case normalization**: Lowercasing loses case information; sometimes case is significant.
- **No stop‑word removal**: Common words like "the" will dominate counts; may need filtering.
- **Encoding issues**: Assumes file is UTF‑8 or platform default; may fail with other encodings.

---

## Example 7: Validate Email Address

### 1. Definition and Concept Overview
`is_valid_email(email)` checks whether a given string conforms to a basic email address pattern using regular expressions. Returns `True` if the pattern matches, otherwise `False`.

### 2. Core Principles and Internal Mechanics
- **Regular expression pattern**: `r'^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$'`
  - Local part: one or more alphanumeric, underscore, dot, plus, or hyphen.
  - Domain: one or more alphanumeric or hyphen.
  - TLD: dot followed by one or more alphanumeric, dot, or hyphen (allowing subdomains and multi‑part TLDs like co.uk).
- **`re.match`**: Attempts to match the pattern from the start of the string; returns a match object if successful, otherwise `None`.
- **Boolean return**: Returns `True` if match object is not `None`.

### 3. Step-by-Step Logical Breakdown
1. Compile pattern (or use raw string directly in `re.match`).
2. Call `re.match(pattern, email)`.
3. If result is not `None`, return `True`; otherwise return `False`.

### 4. Implementation (Python) with meaningful inline comments
```python
import re

def is_valid_email(email):
    """Validate email address using regex."""
    # Pattern: local@domain.tld (supports subdomains and some special chars)
    pattern = r'^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$'
    # Return True if pattern matches entire string
    return re.match(pattern, email) is not None

# Example calls
print(is_valid_email("test@example.com"))   # True
print(is_valid_email("invalid-email"))       # False
```

### 5. Time and Space Complexity Analysis
- **Time complexity**: O(n) where n is length of email string, as regex engine processes characters.
- **Space complexity**: O(1) for the pattern; match object overhead negligible.

### 6. Edge Cases and Failure Scenarios
- **Empty string**: Fails because pattern requires at least one character before @.
- **Missing @**: Fails.
- **Multiple @ symbols**: Fails (pattern expects exactly one @).
- **Trailing spaces**: Not stripped; " test@example.com " would fail because pattern expects start/end anchors. Should strip whitespace first.
- **Internationalized domain names (IDN)**: Pattern only accepts ASCII; fails for emails with Unicode characters.
- **Valid but complex emails**: Some valid emails (e.g., with quoted strings, comments) are not covered by this simplified pattern.

### 7. Practical Use Cases
- **Form validation** in web applications.
- **Client‑side or server‑side email format checking before sending verification emails.

### 8. Limitations and Trade-offs
- **Over‑simplification**: Does not fully comply with RFC 5322; may reject some valid emails and accept some invalid ones (e.g., `a@b.c` is accepted but `a@b.c.` is also accepted due to trailing dot in TLD part).
- **No DNS validation**: Only checks format, not whether the domain actually exists or accepts mail.
- **Unicode support**: Lacks; for modern applications, consider using `email.utils.parseaddr` or specialized libraries.
- **Performance**: Regex compilation on each call; can be pre‑compiled with `re.compile` for repeated use.

---

*These examples illustrate common real‑world uses of functions in Python, each with specific design considerations, trade‑offs, and edge cases.*