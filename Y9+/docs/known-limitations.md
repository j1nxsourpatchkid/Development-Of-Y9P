# KNOWN LIMITATIONS

This is an honest list of what Y9+ does not support yet. If something isn't
listed here or in the syntax docs, assume it doesn't exist yet.

## @bring does nothing

`@bring /io.standard/` is parsed and stored, but it is never checked or
enforced. Removing it currently has no effect on whether the program runs.

Standard library files cannot currently be imported or dynamically loaded.

## Documentation comments are not yet functional

`///` is recognized and parsed the same as `//`, but there is no
documentation-generation tool or special handling for `///` comments yet.

They are purely decorative right now, identical to a regular `//` comment.

## Built-in APIs are limited

The only built-in APIs currently available are:

* `display.show()`
* `input.read()`

There are currently no built-in math utilities such as `abs()`, `sqrt()`,
`pow()`, `floor()`, `ceil()`, `min()`, or `max()`.

There are also no built-in string utility methods such as `split()`, `trim()`,
`substring()`, `indexOf()`, or case-conversion functions.

File system access, networking, environment variables, time/date utilities,
and process-control APIs are not currently implemented.

## No custom data types

Y9+ currently supports the primitive types:

* `int`
* `float`
* `deci`
* `bool`
* `chr`
* `string`

Arrays are also supported.

Custom types such as structs, classes, interfaces, enums, tuples, unions,
and type aliases are not implemented yet.

## No global variables

Variables must be declared inside functions or `entry main()`.

Top-level global variables are not currently supported.

## Strings have limited functionality

Strings do not currently support `.length`.

String indexing is also not supported. For example:

```y9
string text = "Hello";
chr first = text[0];
```

is invalid.

## Arrays have limited functionality

Arrays cannot currently be compared using `==` or `!=`.

Only these array methods are currently implemented:

* `.push()`
* `.pop()`
* `.remove()`

Methods such as `.map()`, `.filter()`, `.forEach()`, `.slice()`,
`.concat()`, and `.indexOf()` are not implemented.

## Missing operators

The modulo operator `%` is not implemented.

Increment and decrement operators (`++` and `--`) are not implemented.

Use compound assignment instead:

```y9
x += 1;
x -= 1;
```

Bitwise operators are not implemented:

* `&`
* `|`
* `^`
* `~`
* `<<`
* `>>`

The ternary operator is also not supported:

```text
condition ? expr1 : expr2
```

## Functions must be declared before main

User-defined functions (`fn`) must be declared above `entry main()`.

Functions cannot currently be declared below `entry main()`, inside another
function, or inside `entry main()`.

## No first-class functions

Functions cannot currently be:

* passed as arguments
* assigned to variables
* returned from other functions

Lambda functions, arrow functions, and closures are not implemented yet.

## Fixed entry main() signature

`entry main()` cannot currently accept parameters or command-line arguments.

The entry point must remain:

```y9
entry main()
```

## No language-level error handling

Y9+ does not currently have `try` / `catch` or another language-level
exception handling system.

When a runtime error occurs, execution immediately stops and the program exits
with code `1`.

## Runtime errors have limited context

Parse-time errors provide line and column information.

Runtime errors provide descriptive error messages, such as for:

* out-of-bounds array access
* type mismatches
* division by zero
* invalid operations

However, runtime errors do not currently include line numbers, column
numbers, or stack traces.

Majority if not all of these will be worked on through release: 6, and release: 7