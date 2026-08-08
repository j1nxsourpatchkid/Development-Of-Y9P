# Release Number: 2026.08.08 - 4

## Program structure

Every program requires an `entry main()` function and a `return` statement.

`@bring` imports are parsed and stored but are not currently enforced or used at runtime.

## Printing

`display.show(...)` prints a string literal, variable, or expression result.

## Input

**Expression form** (returns a value):
string name = input.read();
num age = input.read("Enter your age: ");

**Statement form** (writes into a mutable variable, optional prompt):
mut num x = 0;
input.read(x);
input.read("Enter value: ", x);

Input is type‑aware when an expected type is available.
Supported types: num, float, deci, bool, chr, string.

Invalid input produces a runtime error.
When no expected type exists, input defaults to string.

## Variables

Six types: num, float, deci, bool, chr, string.
Type is required, never inferred.
Immutable by default; mut allows reassignment.

## Type checking

Declared types are enforced. Assigning a mismatched type throws a runtime error on both declaration and reassignment.

## Comments

// for single‑line comments. /// is recognised but has no special behaviour yet.

## Expressions

Arithmetic: +, -, *, / on numbers, with correct precedence and numeric type widening (num → float → deci).

Strings support:
- + (concatenation)
- - (removes all occurrences of a substring)
- * (repetition with a number)

Division by zero throws an error.
Unary negation works on literals and variables (-5, -x).

## Compound assignment

+=, -=, *=, /= for numeric types (num, float, deci).
+= for string (concatenation).
All compound operators require the target variable to be mutable.

## Comparisons

==, !=, <, >, <=, >= produce a bool.
Works across numeric types (with widening), strings/chars (lexicographic), and bools (equality only).

## Control flow

if / elif / else – condition must be bool.
Both single‑statement and block forms are supported.

while (condition) do { ... } – loops while condition is true.

for loops:
- Range‑based: for i in startExpr..endExpr do { ... }  (optional step clause)
- C‑style:    for (init; condition; update) do { ... }

break;  – exits the innermost loop.
next;   – skips to the next iteration of the innermost loop.
return; – now accepts any integer‑compatible expression.

All loop variables declared inside for are scoped to the loop body and are immutable.
break / next / return work consistently inside while and for loops.
