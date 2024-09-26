# PHP Type Juggling

#### **What is PHP Type Juggling?**

> &#x20;**is the automatic conversion between different data types by PHP when performing operations or comparisons. This behaviour is rooted in PHP's nature as a loosely-typed language, designed to make development easier but often leading to unexpected results and potential security vulnerabilities if not managed properly.**

### Key Concepts:

* Comparisons:
  * Loose Comparison(<mark style="color:red;">**`==`**</mark>):

```php
"123" == 123   // true
"0" == false   // true
null == ""     // true
var_dump(0 == 'test'); // true, because 'test' is converted to 0
var_dump('0' == 0);    // true, both are treated as integers
```

* Arithmetic operations:
  * performing math operations between strings numbers causes strings to be converted to numbers

```php
"10" + 5    // 15
"10abc" + 5 // 15
```

* Boolean Contexts:
  * Non-boolean values are converted to <mark style="color:red;">**true**</mark> or <mark style="color:red;">**false**</mark> based on their value.

```python
if ("0") {.....}    // Evaluates to false
if (0) {......}     // Evaluates to false
if ("foo") {.....}  // Evaluates to true
if (bool(False)) {} // Evaluates to false
if (bool(None)) {}  // Evaluates to false
if (bool(0))  {}    // Evaluates to false
if (bool("")) {}    // Evaluates to false
if (bool(())) {}    // Evaluates to false
if (bool([])) {}    // Evaluates to false
if (bool({})) {}    // Evaluates to false
```

* **String Operations**:
  * Concatenating numbers with strings converts numbers to strings.

```php
"Number: " . 10 // "Number: 10"
var_dump(0 === '0');   // false, different types (int and string)
```

### How Type Juggling Works

1. **String to Number**:
   * Non-numeric strings are partially converted to numbers until an invalid character is encountered.

```python
(int) "123abc" // 123
(int) "abc123" // 0
```

Boolean to Integer:

```python
(int) true // 1
(int) false // 0
```

3. **Loose Comparison**:
   * PHP converts both operands to a common type for comparison.

```python
"10" == 10     // true
0 == false     // true
0 == bool({})  // true
```

4. **Strict Comparison**:
   * Compares both value and type, avoiding type juggling.

```python
"10" === 10 // false
0 === false // false
```

### Identifying Type Juggling

Source Code Review & Analysis

1. **Code Review**:
   * Look for operations between different types, especially comparisons.
   * Identify any loose comparisons (<mark style="color:red;">**`==`**</mark>, <mark style="color:red;">**`!=`**</mark>) that could cause unexpected results.
   * Examine the use of user inputs in operations and comparisons.
   * Review functions or methods where types are not explicitly handled.
2. **Static Analysis Tools**:
   * Use tools like **PHPStan** or **Psalm** to identify potential type juggling issues by analyzing code for type inconsistencies.
3. **Explicit Type Declarations**:
   * Ensure proper type hints and declarations (e.g., function parameters, return types) are used where possible.
4. **Testing and Debugging**:
   * Test with varied inputs to observe behaviour. Use <mark style="color:red;">**`var_dump()`**</mark> similar functions to print variable types and values before and after operations.

```php
$input = "123";
var_dump($input);  // string(3) "123"
$input += 1;
var_dump($input);  // int(124)
```

### Why Did This Happen?

PHP automatically tries to convert types based on context:

* PHP converts the string to a number when you try to perform an arithmetic operation on a string containing numeric characters.
* <mark style="color:red;">**`"123"`**</mark> is a valid numeric string, so PHP treats it as an integer <mark style="color:red;">**`123`**</mark> and adds <mark style="color:red;">**`1`**</mark>, resulting in <mark style="color:red;">**`124`**</mark>.

#### External Testing and Observation

* Fuzzing Testing:

```sh
curl -X POST -d "input=123" <http://example.com/test>
curl -X POST -d "input=abc" <http://example.com/test>
```

* Behavioural Analysis:
  * Compare the application's responses to different types of input. Check for discrepancies in behaviour when inputs are of different types but logically similar (e.g., "0" vs. 0).

```sh
# Test numeric vs. string input
curl -X POST -d "amount=10" <http://example.com/process>
curl -X POST -d "amount=10abc" <http://example.com/process>
```

* **Boundary Testing**:
  * Test edge cases where type boundaries might be crossed, such as large numbers, special characters, or unexpected boolean values.

```sh
curl -X POST -d "input=" <http://example.com/test>  # Empty input
curl -X POST -d "input=9999999999" <http://example.com/test>  # Large number
curl -X POST -d "input=0" <http://example.com/test>  # Zero
```

### Exploiting Type Juggling

**Authentication Bypass:**

```php
// Vulnerable code
if ($_POST['password'] == $storedPassword) {
    // Grant access
}
```

**Weak Hash Comparisons:**

Hash comparisons using <mark style="color:red;">**==**</mark> can be bypassed by crafting inputs that evaluate the same numeric value.

```php
// Vulnerable hash comparison
$hash = "0e123456789"; // MD5 or SHA1 hash with leading 0e\
if ($userInput == $hash) {
    echo "Hash match!";
}
// Attacker can use "0e123456780" or similar crafted string
```

```php
$hash1 = '0e12345'; // Starts with '0e', PHP interprets this as scientific notation (0 * 10^12345)
$hash2 = '0e54321'; // Same result (0)

if ($hash1 == $hash2) {
    // Vulnerable to type juggling attack
}
```

**Array Key Manipulation:**

```php
// Vulnerable array access
$arr = ["foo" => "bar", 0 => "zero"];
echo $arr["0"]; // Attacker might access $arr[0] using "0"
```

**Unexpected Value Substitution:**

```php
// Vulnerable arithmetic operation
$amount = "10abc";
$total = $amount + 5; // Results in 15 due to type juggling
```

### **Mitigation Strategies**

1. **Use Strict Comparisons (**<mark style="color:red;">**`===`**</mark>**)**:
   * Always use strict comparisons to avoid type juggling.

```php
if ($input === "password123") {
    // Secure check
}
```

2. Explicit Type Casting:

```php
$amount = (int) $_POST['amount'];
```

3. **Input Validation and Sanitization**:
   * Validate and sanitize input to ensure it matches the expected type.

```php
$input = filter_var($_POST['input'], FILTER_VALIDATE_INT);
```

4. **Type Declarations and Hints**:
   * Use type hints in function parameters and return types to enforce correct types.

```php
function calculateTotal(int $amount): int {
    return $amount * 1.2;
}
```

5. **Avoid Implicit Conversions**:
   * Write code that avoids relying on PHP's implicit type conversions.

```php
$result = (int) $input + (int) $number;
```
