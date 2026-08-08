CHANGELOG

This file reflects the current state of Y9+, not a dated history.
It is updated in place as the language changes.

## Program structure

Every file requires @bring, an entry main() function, and a return statement.

## Printing

display.show(...) prints a string literal, variable, or expression result.

## Variables

Six types: num, float, deci, bool, chr, string. Type is required, never
inferred. Variables are immutable by default; mut allows reassignment.

## Type checking

Declared types are enforced. Assigning a mismatched type throws a runtime
error, both on declaration and reassignment.

## Comments

// for single-line comments, /// for documentation comments. Both are
currently skipped identically by the lexer with no functional difference.

## Expressions

Full arithmetic: +, -, *, / between numbers, with correct operator
precedence and numeric type widening (num -> float -> deci). Strings
support + (concatenation), - (removes all occurrences of a substring),
and * (repetition with a number). Division by zero throws an error.
Unary negation works on both literals and variables (-5, -x).

## Comparisons

==, !=, <, >, <=, >= all produce a bool. Works across numeric types
(with widening), strings and chars (lexicographic ordering), and bools
(equality only, no ordering). Standard precedence applies: arithmetic
evaluates before comparison.
