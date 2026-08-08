# Y9+ Language Features

Everything Y9+ can do as of the current implementation.

---

## Program Structure

* Every program requires an `entry main()` function.
* `@bring` imports are parsed but not enforced (decorative only).

```y9
entry main()
{
    // code here
    return 0;
}
```

---

## Output

`display.show(expr)` prints a value. It accepts literals, variables, and expressions.

```y9
display.show("Hello");
display.show(42);
display.show(x + 1);
```

---

## Input

### Expression Form

Returns a value. A prompt is optional.

```y9
string name = input.read();
num age = input.read("Enter your age: ");
```

### Statement Form

Writes directly into an existing mutable variable. A prompt is optional.

```y9
mut num x = 0;
input.read(x);
input.read("Enter value: ", x);
```

Supported input types:

* `num`
* `float`
* `deci`
* `bool`
* `chr`
* `string`

Invalid input causes a runtime error.

When no expected type is available, `input.read()` defaults to `string`.

---

## Variables

Six types are supported:

* `num` — integer
* `float`
* `deci`
* `bool`
* `chr`
* `string`

Types must be explicitly declared.

Variables are immutable by default. Use `mut` to allow reassignment.

Type checking is enforced at runtime.

```y9
num x = 10;
mut float y = 3.14;
bool flag = true;
string s = "hello";
chr c = 'A';
```

---

## Comments

```y9
// Single-line comment
```

`///` documentation comments are recognized but currently have no special behavior.

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
num → float → deci
```

when mixing numeric types.

```y9
num a = 5 + 3 * 2;
float b = 10 / 3.0;
```

Unary negation works on literals and variables.

```y9
num c = -5;
num d = -x;
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

`*` repeats a string by a number:

```y9
string a = "ab" * 3;
```

Result:

```text
ababab
```

Division by zero causes a runtime error.

---

## Compound Assignment

Supported operators:

* `+=`
* `-=`
* `*=`
* `/=`

Numeric types (`num`, `float`, `deci`) support all four.

Strings support `+=` for concatenation.

Compound assignment requires the variable to be mutable.

```y9
mut num x = 10;

x += 5;
x -= 3;
x *= 2;
x /= 2;
```

String example:

```y9
mut string s = "Hello";
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

Bools support equality comparisons only.

```y9
bool b1 = 5 < 10;
bool b2 = "abc" == "abc";
bool b3 = true != false;
```

Arithmetic is evaluated before comparisons.

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

The condition must evaluate to `bool`. Invalid condition types produce a runtime error.

Both block and single-statement forms are supported.

Single statement:

```y9
if (x > 5) do
    display.show("big");
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
* Exactly one statement may omit `{ }` after `do`.

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
while (x < 5) do
    display.show(x);
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

The range variable is automatically a `num`, is scoped to the loop, and cannot be reassigned inside the loop.

---

### for — C-Style

```y9
for (mut num i = 0; i < 10; i += 1) do
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
for (mut num i = 0; i < 10; i += 1) do
{
    display.show(i);
}
```

Or use an existing variable:

```y9
mut num i = 0;

for (i = 0; i < 10; i += 1) do
{
    display.show(i);
}
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
    if (i == 5) do
        break;

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
    if (i == 5) do
        next;

    display.show(i);
}
```

---

## Return

`return expr;` immediately exits `main()` with the given integer-compatible value.

The expression can be any expression that produces an integer-compatible `num`.

```y9
return 0;
return x + 2;
return a * b;
```

`return` works inside nested blocks and loops.

---

## Scoping

Y9+ uses lexical block scoping.

Variables declared inside `{ }` are not visible outside that block.

Loop variables declared by `for` are scoped to the loop.

A variable declared outside a loop remains accessible after the loop.

---

## Full Example

```y9
@bring /io.standard/

entry main()
{
    mut num counter = 0;

    while (counter < 3) do
    {
        display.show(counter);
        counter += 1;
    }

    for i in 0..5 step 2 do
    {
        if (i == 4) do
            break;

        display.show(i);
    }

    for (mut num j = 0; j < 3; j += 1) do
    {
        if (j == 1) do
            next;

        display.show(j);
    }

    display.show("Done");

    return 0;
}
```

Output:

```text
0
1
2
0
2
0
2
Done
```
