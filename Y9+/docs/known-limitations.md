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

## Tuples and Destructuring

Tuple types `(T, U)` and tuple destructuring assignments `(a, b) = get_pair();` are not implemented yet. Return structs or arrays instead.
```

is now supported for trait-based bounds. Generic type parameters can still
represent any valid Y9+ type when no constraint is specified.

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

## Debugger & Profiling Tooling

Y9+ prints runtime stack traces on errors, but does not yet provide an interactive step-through debugger (breakpoints, watch expressions) or CPU/memory profiling tools.
```

However, Y9+ does not yet provide an interactive debugger or step-through
profiling tooling.

## Developer Tooling & Ecosystem Roadmap

Y9+ includes command-line tooling (`y9 run`, `y9 check`, `y9 test`, `y9 fmt`, `y9 repl`, and `y9 ast`). A standalone IDE is planned for Release 13.

Y9+ does not currently include:

- Standalone IDE (Target: Release 13)
- Language Server Protocol (LSP) (Target: Release 12)
- VS Code extension
- Linter
- Documentation generator
- Package manager
- Dependency manager