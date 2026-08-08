##Everything Possible In Y9P As Of The Latest Implementation

Program Structure

entry main() function is required.

@bring imports are parsed but not enforced.

Output

display.show(expr) prints a value (literal, variable, or expression result).

Input

Expression form: input.read() returns a value, can include an optional prompt.

Statement form: input.read(variable) or input.read("prompt", variable) writes into a mutable variable.

Supported input types: num, float, deci, bool, chr, string.

Variables

Six types: num (integer), float, deci, bool, chr, string.

Type must be explicitly declared.

Immutable by default; mut keyword allows reassignment.

Type checking is enforced at runtime.

Comments

// for single-line comments.

Arithmetic

Operators: +, -, *, / between numbers.

Precedence: multiplication/division before addition/subtraction.

Numeric widening: num → float → deci when mixing types.

Unary negation (-x, -5).

String operators:

+ concatenation

- removes all occurrences of a substring

* repetition with a number

Division by zero throws a runtime error.

Compound Assignment

+=, -=, *=, /= work on num, float, deci.

+= also works on string (concatenation).

Requires the variable to be mutable.

Comparisons

==, !=, <, >, <=, >= produce a bool.

Work across numeric types (with widening), strings/chars (lexicographic), and bools (equality only).

Control Flow

if (condition) do { ... }

elif (condition) do { ... }

else do { ... }

Conditions must evaluate to bool; runtime error otherwise.

Both single‑statement (no braces) and block forms are allowed.

Loops

while (condition) do { ... } – repeats while condition is true.

Range‑based for:

for varName in startExpr..endExpr do { ... }

Optional step clause: for i in 0..10 step 2 do

Loop variable is automatically num, scoped to the loop body, immutable.

C‑style for:

for (init; condition; update) do { ... }

init can be a variable declaration (mut num i = 0) or assignment (i = 0).

update supports =, +=, -=, *=, /= on a mutable variable.

Condition must be bool.

Variable declared in init is scoped to the loop body.

Loop Control

break; – exits the innermost enclosing loop immediately.

next; – skips the remainder of the current loop iteration and proceeds to the next iteration (update expression executes first in C‑style for).

Both break and next are only valid inside a loop (parse‑time error otherwise).

return exits main() immediately, now accepts any integer‑compatible expression (not just literals).

Scoping

Lexical block scoping: variables declared inside a block (if, while, for) are invisible outside.

Loop variables in range‑based for are block‑scoped to the loop body.

Return

return expr; where expr must produce an integer num.

Allowed anywhere inside main(), including deeply nested loops/ifs.
