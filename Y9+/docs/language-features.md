# Y9+ Language Features

Everything Y9+ can do as of the current implementation.

---

## Program Structure

* Every program requires an `entry main()` function.
* `entry main()` can take no arguments or accept command-line arguments as a `string[] args`.
* Returning an exit code (e.g. `return 0;`) is optional in `entry main()`.
* If `entry main()` finishes without reaching a return statement, execution automatically returns `0`.
* An explicit `return expr;` in `entry main()` must produce an integer-compatible `int`.
* An empty `return;` without a value is invalid inside `entry main()` and causes a runtime error.
* User-defined functions (`fn`) are declared above `entry main()`.
* Struct declarations may appear at the top level.
* Global variable declarations may appear at the top level.
* `export` can be used on top-level functions, structs, and global variables.
* `@bring` imports can import standard library modules or source files.

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

`entry main()` may also receive command-line arguments:

```y9
entry main(string[] args)
{
    if (args.length > 0) do
    {
        display.show(args[0]);
    }

    return 0;
}
```

---

## Modules & Imports

Y9+ uses `@bring` to import modules.

Standard library modules can be imported using a standard library path:

```y9
@bring /std/math;
@bring /std/io;
@bring /std/fs;
```

Source files can also be imported using a relative path:

```y9
@bring "./utils.y9";
```

Circular module dependencies are detected and reported as import errors.

Top-level declarations can be exported using `export`:

```y9
export fn add(int a, int b) -> int
{
    return a + b;
}
```

Structs and global variables can also be exported:

```y9
export struct Player
{
    string name;
    change int score;
}

export change int callCount = 0;
```

Members exported by an imported module can be accessed using the module filename as a namespace:

```y9
@bring "./math_utils.y9";

entry main()
{
    math_utils.Vector2 v = math_utils.Vector2(1.0, 2.0);
    display.show(v);
}
```

---

## Output

`display.show(expr)` prints a value. It accepts literals, variables, expressions, arrays, and structs.

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

Global variables may also be declared at the top level:

```y9
change int callCount = 0;
```

Top-level global variables can be exported from modules using `export`.

---

## Structs

Structs are user-defined composite data types with named fields.

### Declaration

Fields are immutable by default. Use `change` to make a field mutable.

```y9
struct Player
{
    string name;
    change int score;
    change float health;
}
```

### Instantiation

Structs are instantiated by calling the struct name with field values in declaration order.

```y9
Player p = Player("Alice", 100, 98.5);
```

### Field Access

Fields are accessed using dot notation.

```y9
display.show(p.name);

p.score += 50;
p.health = 100.0;
```

Attempting to modify an immutable field produces a runtime error.

```y9
// p.name = "Bob"; // Invalid
```

### Structural Comparison

Struct instances support deep equality comparison with `==` and `!=`.

```y9
Player p1 = Player("Alice", 100, 100.0);
Player p2 = Player("Alice", 100, 100.0);

bool identical = p1 == p2;
```

Structs can be exported from modules using `export`.

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

`.indexOf(val)` returns the index of a value, or `-1` when the value is not found.

`.slice(start, end)` returns a sub-array from `start` up to, but not including, `end`.

`.concat(other)` returns a new array containing the elements of both arrays.

```y9
change int[] items = [10, 20, 30];

items.push(40);

int last = items.pop();

int item = items.remove(0);

int index = items.indexOf(30);

int[] sub = items.slice(0, 1);

int[] merged = items.concat([50, 60]);
```

### Array Deep Equality

Arrays support deep structural comparison using `==` and `!=`.

```y9
bool same = ([1, 2, 3] == [1, 2, 3]);
```

---

## Strings

Strings are immutable sequences of characters.

Direct index assignment is not supported:

```y9
string text = "Hello";

// text[0] = 'a'; // Invalid
```

### String Indexing & Property

Strings support 0-based indexing.

Indexing a string returns a `chr`.

```y9
string text = "Hello";

chr first = text[0];
int len = text.length;
```

### String Methods

`.indexOf(search)` finds the index of a substring or character.

`.substring(start, end)` returns the characters from `start` up to, but not including, `end`.

`.split(sep)` splits a string into a `string[]`.

`.trim()` removes leading and trailing whitespace.

`.replace(old, new)` replaces all occurrences of a substring.

`.toUpper()` converts a string to uppercase.

`.toLower()` converts a string to lowercase.

```y9
string greeting = "  Hello, World!  ";

string clean = greeting.trim();
string upper = clean.toUpper();
string sub = clean.substring(0, 5);
string[] parts = clean.split(",");
string replaced = clean.replace("World", "Y9+");
```

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
* `%`

Multiplication and division have higher precedence than addition and subtraction.

Modulo (`%`) returns the remainder of a numeric division.

```y9
int remainder = 10 % 3;
```

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

Division or modulo by zero causes a runtime error.

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

String concatenation also supports combining a string with other values:

```y9
string message = "Score: " + 100;
```

String repetition supports either operand order:

```y9
string a = "ab" * 3;
string b = 3 * "ab";
```

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
* `%=`

Numeric types (`int`, `float`, `deci`) support all five.

Strings support `+=` for concatenation.

Compound assignment requires the target variable or mutable struct field to be mutable.

```y9
change int x = 10;

x += 5;
x -= 3;
x *= 2;
x /= 2;
x %= 3;
```

String example:

```y9
change string s = "Hello";
s += " World";
```

Mutable struct fields can also use compound assignment:

```y9
struct Player
{
    change int score;
}

Player p = Player(10);

p.score += 5;
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

Arrays and structs support deep equality comparisons using `==` and `!=`.

```y9
bool b1 = 5 < 10;
bool b2 = "abc" == "abc";
bool b3 = true != false;

bool b4 = ([1, 2] == [1, 2]);
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

### Ternary Expression

Y9+ supports conditional ternary expressions:

```y9
int maxVal = (a > b) ? a : b;
```

The condition must evaluate to `bool`.

The expression before `:` is selected when the condition is `true`; otherwise the expression after `:` is selected.

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

### for — Iterable

The `for` loop can iterate directly over arrays and strings.

When iterating over an array, the loop variable contains each element:

```y9
int[] nums = [10, 20, 30];

for item in nums do
{
    display.show(item);
}
```

When iterating over a string, the loop variable is a `chr` containing each character:

```y9
for ch in "Y9+" do
{
    display.show(ch);
}
```

The iterable expression is evaluated when the loop starts.

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
* `%=`

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
* iterable `for`
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
* iterable `for`
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

## Error Handling

### try / catch

`try` / `catch` blocks catch runtime errors occurring inside the `try` block.

The `catch` parameter must be explicitly typed as `string`.

```y9
try do
{
    int zero = 0;
    int result = 10 / zero;
}
catch (string err) do
{
    display.show("Caught error: " + err);
}
```

Runtime errors that occur inside the `try` block are caught and provided to the `catch` block as a `string`.

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

Struct fields are scoped to their containing struct and are accessed through a struct instance.

Top-level global variables have module-level scope.

---

## Standard Library

Y9+ provides standard modules that can be imported using `@bring`.

### /std/math

The math module provides mathematical helper functions:

```text
abs
sqrt
floor
ceil
round
pow
min
max
sin
cos
tan
```

Example:

```y9
@bring /std/math;

float value = sqrt(25.0);
display.show(value);
```

### /std/io

The IO module provides wrappers around the built-in input and output functions.

```text
print
read
```

Example:

```y9
@bring /std/io;

io.print("Hello");
```

### /std/fs

The filesystem module provides file operations:

```text
readFile
writeFile
exists
```

Example:

```y9
@bring /std/fs;

string content = fs.readFile("test.txt");
```

---

## Escape Sequences

Strings and character literals support escape sequences.

Supported sequences include:

```text
\n  Newline
\t  Horizontal Tab
\r  Carriage Return
\\  Backslash
\"  Double Quote
\'  Single Quote
```

Example:

```y9
string msg = "Hello\n\tWorld!";
chr newline = '\n';
```

---

## Full Example

```y9
@bring /std/math;
@bring /std/io;

struct Player
{
    string name;
    change int score;
}

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

entry main(string[] args)
{
    Player player = Player("Alice", 0);

    int[] numbers = [1, 2, 3, 4, 5, 6, 7, 8];
    int[] evensOnly = filterEvens(numbers);

    player.score = evensOnly.length;

    display.show("Player: " + player.name);
    display.show("Score: " + player.score);
    display.show("Filtered evens:");
    display.show(evensOnly);

    for item in evensOnly do
    {
        display.show(item);
    }

    try do
    {
        int zero = 0;
        int result = 10 / zero;
        display.show(result);
    }
    catch (string err) do
    {
        display.show("Caught error: " + err);
    }
}
```

Output:

```text
Player: Alice
Score: 4
Filtered evens:
[2, 4, 6, 8]
2
4
6
8
Caught error: Division by zero
```
