# Y9+ Language Features

Everything Y9+ can do as of the current implementation.

---

## Program Structure

* Every program requires an `entry main()` function.
* Returning an exit code (e.g. `return 0;`) is optional in `entry main()`.
* If `entry main()` finishes without reaching a return statement, execution automatically returns `0`.
* An explicit `return expr;` in `entry main()` must produce an integer-compatible `int`.
* An empty `return;` without a value is invalid inside `entry main()` and causes a runtime error.
* User-defined functions (`fn`) are declared above `entry main()`.
* `@bring` imports are parsed but not enforced (decorative only).

```y9
fn add(int a, int b) -> int
{
    return a + b;
}

entry main()
{
    int result = add(5, 3);
    display.show(result);
}
```

---

## Output

`display.show(expr)` prints a value. It accepts literals, variables, expressions, and arrays.

```y9
display.show("Hello");
display.show(42);
display.show(x + 1);
display.show([1, 2, 3]);
```

---

## Input

### Expression Form

Returns a value. A prompt is optional.

```y9
string name = input.read();
int age = input.read("Enter your age: ");
```

### Statement Form

Writes directly into an existing mutable variable. A prompt is optional.

```y9
change int x = 0;

input.read(x);
input.read("Enter value: ", x);
```

Supported input types:

* `int`
* `float`
* `deci`
* `bool`
* `chr`
* `string`

Invalid input causes a runtime error.

When no expected type is available, `input.read()` defaults to `string`.

---

## Variables

Six primitive types are supported:

* `int` — integer
* `float`
* `deci`
* `bool`
* `chr`
* `string`

Types must be explicitly declared.

Variables are immutable by default. Use `change` to allow reassignment.

Type checking is enforced at runtime.

```y9
int x = 10;
change float y = 3.14;
bool flag = true;
string s = "hello";
chr c = 'A';
```

---

## Arrays

Arrays are ordered, dynamically sized collections of elements.

### Declaration & Literals

Array types are declared using `type[]`.

Array literals use square brackets.

```y9
int[] numbers = [1, 2, 3, 4, 5];
change string[] names = ["Alice", "Bob"];
change int[] emptyList = [];
```

Numeric elements inside array literals are automatically widened when types are mixed.

For example, `[1, 2.5]` produces a `float[]`.

### Indexing & Assignment

Arrays use 0-based indexing for reading and writing.

```y9
int first = numbers[0];

numbers[0] = 99;
```

Accessing an out-of-bounds index produces a runtime error.

### Array Property

`.length` returns the number of elements in the array as an `int`.

```y9
int len = numbers.length;
```

### Array Methods

`.push(val)` appends a value to the end of the array and returns the new length.

`.pop()` removes and returns the last element.

`.remove(index)` removes and returns the element at the specified index.

```y9
change int[] items = [10, 20];

items.push(30);

int last = items.pop();

int item = items.remove(0);
```

After these operations, `items` is `[20]`.

---

## Functions

Custom functions are declared using `fn`.

### Syntax

```y9
fn doubleValue(int n) -> int
{
    return n * 2;
}
```

Functions may accept zero or more parameters.

Parameters are immutable by default. Use `change` before a parameter type to make it mutable within the function.

```y9
fn increment(change int val) -> int
{
    val += 1;
    return val;
}
```

Return type annotations use `-> Type`.

Functions that omit `-> Type` are treated as void functions.

```y9
fn sayHello()
{
    display.show("Hello");
}
```

Functions can be called from `entry main()` or from other functions.

```y9
int result = doubleValue(5);
```

---

## Comments

```y9
// Single-line comment
```

`///` documentation comments are recognized but currently have no special behavior.

They are parsed the same way as regular `//` comments and are currently decorative only.

---

## Arithmetic

Basic operators:

* `+`
* `-`
* `*`
* `/`

Multiplication and division have higher precedence than addition and subtraction.

Numeric type widening:

```text
int → float → deci
```

when mixing numeric types.

```y9
int a = 5 + 3 * 2;
float b = 10 / 3.0;
```

Unary negation works on numeric literals, variables, and expressions.

```y9
int c = -5;
int d = -a;
```

### String Operators

`+` concatenates strings:

```y9
string a = "Hello" + " World";
```

`-` removes all occurrences of a substring:

```y9
string a = "banana" - "a";
```

Result:

```text
bnn
```

`*` repeats a string by an integer:

```y9
string a = "ab" * 3;
```

Result:

```text
ababab
```

Division by zero causes a runtime error.

---

## Logical Operators

Supported operators:

* `&&` — Logical AND
* `||` — Logical OR
* `!` — Logical NOT

Logical operators operate on `bool` values.

`&&` and `||` use short-circuit evaluation.

```y9
bool valid = (x > 0) && (x < 100);
bool check = isReady || !hasFailed;
```

For example, the right-hand side of `||` is not evaluated when the left-hand side is already `true`.

```y9
if (a || (1 / 0 == 0)) do display.show("Short-circuit works!");
```

---

## Compound Assignment

Supported operators:

* `+=`
* `-=`
* `*=`
* `/=`

Numeric types (`int`, `float`, `deci`) support all four.

Strings support `+=` for concatenation.

Compound assignment requires the target variable to be mutable.

```y9
change int x = 10;

x += 5;
x -= 3;
x *= 2;
x /= 2;
```

String example:

```y9
change string s = "Hello";
s += " World";
```

---

## Comparisons

Supported operators:

* `==`
* `!=`
* `<`
* `>`
* `<=`
* `>=`

All comparisons produce a `bool`.

Comparisons work across numeric types using numeric widening.

Strings and chars support lexicographic comparison.

Bools support equality comparisons (`==`, `!=`) only.

```y9
bool b1 = 5 < 10;
bool b2 = "abc" == "abc";
bool b3 = true != false;
```

Arithmetic and logical precedence rules apply as standard.

---

## Control Flow

### if / elif / else

```y9
if (condition) do
{
    // block
}
elif (condition) do
{
    // block
}
else do
{
    // block
}
```

The condition must evaluate to `bool`.

Both block and single-statement forms are supported.

Single statement:

```y9
if (x > 5) do display.show("big");
```

Block:

```y9
if (x > 5) do
{
    display.show("big");
    display.show(x);
}
```

The same body rule applies to `if`, `elif`, `else`, `while`, and `for`:

* Multiple statements require `{ }`.
* Exactly one statement may omit `{ }` by sitting on the same line to the right of `do`.

---

## Loops

### while

```y9
while (condition) do
{
    // body
}
```

Repeats while the condition evaluates to `true`.

Single-statement bodies are also supported:

```y9
while (x < 5) do display.show(x);
```

The condition must evaluate to `bool`.

---

### for — Range-Based

```y9
for i in 0..10 do
{
    display.show(i);
}
```

The end value is exclusive.

This iterates over:

```text
0, 1, 2, 3, 4, 5, 6, 7, 8, 9
```

An optional `step` is supported:

```y9
for i in 0..10 step 2 do
{
    display.show(i);
}
```

Negative steps are supported:

```y9
for i in 10..0 step -1 do
{
    display.show(i);
}
```

If `step` is omitted, it defaults to `1`.

A `step` of `0` causes a runtime error.

Start, end, and step can be numeric expressions. They are evaluated once when the loop starts.

The range variable is automatically an `int`, is scoped to the loop, and cannot be reassigned inside the loop.

---

### for — C-Style

```y9
for (change int i = 0; i < 10; i += 1) do
{
    display.show(i);
}
```

The three sections are:

```text
initialization ; condition ; update
```

The initialization can declare a variable:

```y9
for (change int i = 0; i < 10; i += 1) do display.show(i);
```

Or use an existing variable:

```y9
change int i = 0;

for (i = 0; i < 10; i += 1) do display.show(i);
```

The update supports:

* `=`
* `+=`
* `-=`
* `*=`
* `/=`

The update target must be mutable.

The condition must evaluate to `bool`.

A variable declared in the `for` initializer is scoped to the loop.

---

## Loop Control

### break

```y9
break;
```

Immediately exits the nearest enclosing loop.

`break` is valid inside:

* `while`
* range-based `for`
* C-style `for`
* nested control-flow blocks inside loops

Using `break` outside a loop produces a parse-time error.

Example:

```y9
for i in 0..10 do
{
    if (i == 5) do break;

    display.show(i);
}
```

---

### next

```y9
next;
```

Skips the remainder of the current iteration and proceeds to the next iteration.

`next` is valid inside:

* `while`
* range-based `for`
* C-style `for`

For C-style `for` loops, the update expression still executes before the next condition check.

Using `next` outside a loop produces a parse-time error.

Example:

```y9
for i in 0..10 do
{
    if (i == 5) do next;

    display.show(i);
}
```

---

## Return

`return expr;` immediately exits a function with the evaluated expression result.

`entry main()` may explicitly return an integer-compatible `int`, but an explicit return is optional.

If `entry main()` finishes without reaching a return statement, it automatically returns `0`.

An empty `return;` without a value is invalid inside `entry main()` and causes a runtime error.

```y9
return 0;
return x + 2;
```

Void functions can use `return;` without an expression or omit `return` at the end of the function body.

```y9
fn sayHello()
{
    display.show("Hello");
    return;
}
```

Return statements work inside nested blocks and loops.

---

## Scoping

Y9+ uses lexical block scoping.

Variables declared inside `{ }` are not visible outside that block.

Loop variables declared by `for` are scoped to the loop.

A variable declared outside a loop remains accessible after the loop.

Function parameters and local variables are scoped to their function.

---

## Full Example

```y9
@bring /io.standard/

fn isEven(int val) -> bool
{
    return (val / 2 * 2) == val;
}

fn filterEvens(int[] nums) -> int[]
{
    change int[] evens = [];

    for (change int i = 0; i < nums.length; i += 1) do
    {
        if (isEven(nums[i]) && nums[i] > 0) do evens.push(nums[i]);
    }

    return evens;
}

entry main()
{
    int[] numbers = [1, 2, 3, 4, 5, 6, 7, 8];
    int[] evensOnly = filterEvens(numbers);

    display.show("Filtered evens:");
    display.show(evensOnly);
}
```

Output:

```text
Filtered evens:
[2, 4, 6, 8]
```

---
