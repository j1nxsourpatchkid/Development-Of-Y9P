# KNOWN LIMITATIONS

This is an honest list of what Y9+ does not support yet. If something isn't
listed here or in the syntax docs, assume it doesn't exist yet.

## Documentation comments are not yet functional

`///` is recognized and parsed the same as `//`, but there is no
documentation-generation tool or special handling for `///` comments yet.

They are purely decorative right now, identical to a regular `//` comment.

## Missing Operators

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

The following operators are already supported:

* `%`
* `%=`
* `? :`

## Struct Methods / Full OOP

Y9+ supports user-defined `struct` types, fields, construction, property
access, mutability, arrays of structs, and deep equality.

However, methods cannot currently be declared directly inside a `struct`
definition.

Full class-based OOP features such as:

* inheritance
* interfaces
* method overriding
* access modifiers
* abstract classes

are not implemented yet.

## User-Defined Exception Throwing

Y9+ supports `try / catch (string err) do ...` blocks to handle runtime
errors.

However, user code cannot yet throw custom exceptions (for example,
`throw "error"`).

`try / catch` currently catches system and native runtime errors.

## Documentation Generation

Y9+ supports documentation comments using `///`, but there is currently no
documentation-generation tool that converts these comments into generated
API documentation.

## Remaining System-Level APIs

Y9+ now provides a substantially developed standard library through `@bring`,
including functionality for mathematics, IO, filesystems, networking, HTTP,
cryptography, machine learning utilities, neural networks, and other
higher-level capabilities.

The standard library test suite is currently passing completely.

However, some lower-level system and hardware functionality is still not
implemented, including:

* Environment variables
* Advanced time/date APIs
* Process management
* Operating-system APIs
* Hardware and sensor APIs
* Serial communication
* Microcontroller control
* GPU computing
* Image processing
* Audio processing
* Geolocation

## Runtime Diagnostics Are Still Limited

Runtime errors include source line and column information.

For example:

```text
[12:5] Runtime Error: Division by zero
```

However, Y9+ does not yet provide full runtime stack traces or advanced
debugger information.

## Developer Tooling Is Not Yet Fully Implemented

Y9+ includes development tooling and a full IDE, but several ecosystem and
editor integrations are still missing.

Y9+ does not currently include:

* VS Code extension
* Language Server Protocol (LSP)
* Code formatter
* Linter
* Documentation generator
* Package manager
* Dependency manager
