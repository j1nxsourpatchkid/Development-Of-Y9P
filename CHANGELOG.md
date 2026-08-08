# CHANGELOG

This file reflects the current state of Y9+, not a dated history.
It is updated in place as the language changes.

## Program structure

Every program requires an `entry main()` function and a `return` statement.

`@bring` imports are supported syntactically but are not currently enforced.

## Printing

`display.show(...)` prints a string literal, variable, or expression result.

## Input

`input.read()` reads user input from the terminal.

Input can be used directly as an expression:

```y9
string name = input.read();
num age = input.read("Enter your age: ");
```

It can also write directly into an existing mutable variable:

```y9
mut string name = "";
input.read(name);
```

Prompts are optional:

```y9
string name = input.read("Enter your name: ");
```

Input is type-aware when an expected type is available.

Supported input types include:

* `num`
* `float`
* `deci`
* `bool`
* `chr`
* `string`

Numeric, boolean, and character input is parsed according to the expected type.
String input is preserved exactly as entered.

Invalid input produces a runtime error.

When `input.read()` has no expected type, such as when used directly inside
`display.show(...)`, it defaults to `string`.

Direct input into an existing variable requires that variable to be declared
with `mut`.

## Variables

Six types: `num`, `float`, `deci`, `bool`, `chr`, `string`. Type is required,
never inferred. Variables are immutable by default; `mut` allows reassignment.

## Type checking

Declared types are enforced. Assigning a mismatched type throws a runtime
error, both on declaration and reassignment.

## Comments

`//` for single-line comments, `///` for documentation comments. Both are
currently skipped identically by the lexer with no functional difference.

## Expressions

Full arithmetic: `+`, `-`, `*`, `/` between numbers, with correct operator
precedence and numeric type widening (`num -> float -> deci`).

Strings support `+` (concatenation), `-` (removes all occurrences of a
substring), and `*` (repetition with a number).

Division by zero throws an error.

Unary negation works on both literals and variables (`-5`, `-x`).

## Comparisons

`==`, `!=`, `<`, `>`, `<=`, `>=` all produce a `bool`.

Works across numeric types (with widening), strings and chars
(lexicographic ordering), and bools (equality only, no ordering).

Standard precedence applies: arithmetic evaluates before comparison.
