# Y9+ Language Features

Everything Y9+ can do as of the current implementation.

---

## Program Structure

- Every program requires an `entry main()` function.
- `@bring` imports are parsed but not enforced (decorative only).

```y9
entry main()
{
    // code here
    return 0;
}
Output
display.show(expr) prints a value (literal, variable, or expression result).

y9
display.show("Hello");
display.show(42);
display.show(x + 1);
Input
Expression form – returns a value (prompt is optional):

y9
string name = input.read();
num age = input.read("Enter your age: ");
Statement form – writes directly into a mutable variable (prompt is optional):

y9
mut num x = 0;
input.read(x);
input.read("Enter value: ", x);
Supported input types: num, float, deci, bool, chr, string.
Invalid input causes a runtime error.
When no expected type is available, input defaults to string.

Variables
Six types: num (integer), float, deci, bool, chr, string

Type must be explicitly declared.

Immutable by default – use mut to allow reassignment.

Type checking is enforced at runtime.

y9
num x = 10;          // immutable
mut float y = 3.14;  // mutable
bool flag = true;
string s = "hello";
chr c = 'A';
Comments
// for single-line comments.

/// is recognised but has no special behaviour yet.

Arithmetic
Basic operators: +, -, *, / between numbers.
Precedence: multiplication/division before addition/subtraction.
Type widening: num → float → deci when mixing types.

y9
num a = 5 + 3 * 2;   // 11
float b = 10 / 3.0;  // 3.333...
Unary negation:

y9
num c = -5;
num d = -x;
String operators:

+ concatenation: "Hello" + " World" → "Hello World"

- removes all occurrences: "banana" - "a" → "bnn"

* repetition with a number: "ab" * 3 → "ababab"

Division by zero throws a runtime error.

Compound Assignment
+=, -=, *=, /= work on num, float, deci.

+= also works on string (concatenation).

Requires the variable to be mut.

y9
mut num x = 10;
x += 5;   // 15
x -= 3;   // 12
x *= 2;   // 24

mut string s = "Hello";
s += " World";  // "Hello World"
Comparisons
Operators: ==, !=, <, >, <=, >=

Produce a bool.

Work across numeric types (with widening), strings/chars (lexicographic), and bools (equality only).

y9
bool b1 = 5 < 10;       // true
bool b2 = "abc" == "abc"; // true
bool b3 = true != false;  // true
Arithmetic evaluates before comparisons.

Control Flow
if / elif / else
y9
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
Condition must evaluate to bool; runtime error otherwise.

Both single‑statement (no braces) and block forms are allowed.

y9
if (x > 5) display.show("big");
elif (x > 0) display.show("small");
else display.show("zero");
Loops
while
y9
while (condition) do
{
    // body
}
Repeats while condition is true.

Supports single‑statement form without braces.

for (range-based)
y9
for variable in startExpr..endExpr do
{
    // body
}
End is exclusive.

Optional step clause: for i in 0..10 step 2 do

Loop variable is automatically num, scoped to loop body, immutable.

Start, end, and step can be expressions, evaluated once at loop start.

for (C‑style)
y9
for (init; condition; update) do
{
    // body
}
init can be a variable declaration (mut num i = 0) or assignment (i = 0).

update supports =, +=, -=, *=, /= on a mutable variable.

Condition must be bool.

Variable declared in init is scoped to the loop body; if a variable is reused, it must already exist.

Loop Control
break
y9
break;
Immediately exits the innermost enclosing loop.

Only valid inside a loop (parse‑time error otherwise).

next
y9
next;
Skips the remainder of the current iteration, proceeds to next iteration.

In C‑style for, the update expression still executes before the next condition check.

Only valid inside a loop (parse‑time error otherwise).

return
return expr; immediately exits main() with the given integer value.

expr can be any expression producing an integer‑compatible num.

Works anywhere, including inside loops and nested blocks.

y9
return 0;
return x + 2;
return a * b;
Scoping
Lexical block scoping: variables declared inside {} are not visible outside.

Loop variables declared in for are scoped to the loop body.

Full Example
y9
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
Output:

text
0
1
2
0
2
0
2
Done
