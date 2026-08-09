# KNOWN LIMITATIONS

This is an honest list of what Y9+ does not support yet. If something isn't
listed here or in the syntax docs, assume it doesn't exist yet.

## Documentation comments are not yet functional

`///` is recognized and parsed the same as `//`, but there is no
documentation-generation tool or special handling for `///` comments yet.

They are purely decorative right now, identical to a regular `//` comment.

## Built-in APIs are still limited

Y9+ now has standard library support through `@bring`, including math, IO,
and file-system functionality.

However, many system-level APIs are not implemented yet.

There is currently no built-in support for:

* Networking
* HTTP
* WebSockets
* Environment variables
* Time/date APIs
* Process management
* Operating-system APIs
* Hardware/sensor APIs
* Serial communication
* GPU computing

## No first-class functions

Functions cannot currently be:

* passed as arguments
* assigned to variables
* returned from other functions

Lambda functions, arrow functions, and closures are not implemented yet.

This also means higher-order collection operations such as `.map()` and
`.filter()` are not currently available.

## Missing operators

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

## Struct methods / full OOP

Y9+ supports user-defined `struct` types, fields, construction, property
access, mutability, arrays of structs, and deep equality.

However, methods cannot currently be declared directly inside a `struct`.

Full class-based OOP features such as:

* inheritance
* interfaces
* method overriding
* access modifiers
* abstract classes

are not implemented yet.

## Runtime diagnostics are still limited

Runtime errors now include source line and column information.

For example:

```text
[12:5] Runtime Error: Division by zero
```

However, Y9+ does not yet provide full runtime stack traces or advanced
debugger information.

## Developer tooling is not yet implemented

Y9+ does not currently include:

* VS Code extension
* Language Server Protocol (LSP)
* Code formatter
* Linter
* Documentation generator
* Package manager
* Dependency manager
