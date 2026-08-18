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

- `&`
- `|`
- `^`
- `~`
- `<<`
- `>>`

The following operators are already supported:

- `%`
- `%=`
- `? :`

## Traditional Class-Based OOP / Inheritance

Y9+ supports user-defined struct types, generic structs such as
`Box<T>` and `Pair<T, U>`, fields, construction, property access,
mutability, arrays of structs, and deep equality. Y9+ also now supports
inherent methods via `impl`, static methods, traits, default trait methods,
generic bounds, and trait-based polymorphism.

However, traditional class-based OOP features such as:

- class inheritance (`extends` / `super`)
- abstract classes
- access modifiers (`private` / `protected`)
- virtual method tables with dynamic subclass dispatch

are not part of the language design. Composition and traits are used instead.

## Generic Constraints / Trait Bounds

Y9+ supports full parametric generics for functions, structs, enums,
lambdas, and higher-order functions, and now supports generic bounds via
traits (see above).

For example, syntax such as:

```y9
fn add<T: Numeric>(T a, T b) -> T
{
    // ...
}
```

is now supported for trait-based bounds. Generic type parameters can still
represent any valid Y9+ type when no constraint is specified.

## Documentation Generation

Y9+ supports documentation comments using `///`, but there is currently no
documentation-generation tool that converts these comments into generated
API documentation.

## Remaining System-Level APIs

Y9+ now provides a substantially developed standard library through `@bring`,
including functionality for mathematics, IO, filesystems, networking, HTTP,
cryptography, machine learning utilities, neural networks, operating-system
APIs, process management, environment variables, time/date APIs, JSON,
databases, compression, data analysis, and other higher-level capabilities.

The standard library test suite is currently passing completely.

However, some lower-level system and hardware functionality is still not
implemented, including:

- Advanced hardware and sensor APIs
- Serial communication
- Microcontroller control
- GPU computing
- SIMD utilities
- Image processing
- Audio processing
- Geolocation
- Embedded programming
- Robotics utilities
- Computer vision
- Audio synthesis
- Text-to-speech
- Speech recognition

## Runtime Diagnostics

Runtime errors include source line, column, and runtime call stack
information.

For example:

```text
[12:5] Runtime Error: Division by zero
Call Stack:
  at compute [8:14]
  at main [15:5]
```

However, Y9+ does not yet provide an interactive debugger or step-through
profiling tooling.

## Developer Tooling Is Not Yet Fully Implemented

Y9+ includes development tooling and a full IDE, but several ecosystem and
editor integrations are still missing.

Y9+ does not currently include:

- VS Code extension
- Language Server Protocol (LSP)
- Code formatter
- Linter
- Documentation generator
- Package manager
- Dependency manager