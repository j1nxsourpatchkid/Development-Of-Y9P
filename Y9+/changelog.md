# Release Number: 2026.08.18 - 9

## Traits & Abstract Interfaces

* Added `trait` declarations to define shared method contracts: `trait Printable { fn toString(self) -> string; }`.
* Added `Self` type keyword representing the implementing concrete type.
* Added support for generic traits: `trait Transformer<T> { fn transform(self, T val) -> Self; }`.
* Added support for default trait method bodies that can be overridden by implementations.
* Added trait exporting and cross-module trait imports via `@bring`.

## Implementations (`impl`) & Struct Methods

* Added inherent method implementation blocks: `impl Vector2 { fn length(self) -> float { ... } }`.
* Added trait implementation blocks: `impl Printable for Vector2 { fn toString(self) -> string { ... } }`.
* Added trait implementations for primitive types: `impl Printable for int { ... }`.
* Added static method support (methods without a `self` receiver, e.g., `Vector2.new(x, y)`).
* Added receiver mutability annotations:
  * `self` (immutable receiver — prevents mutating instance fields or calling mutating methods).
  * `change self` (mutable receiver — allows field mutation on `self`).
* Added strict call-site mutability enforcement: calling a `change self` method on an immutable instance or immutable struct field triggers a compile-time `TypeError`.
* Added Coherence / Orphan Rule validation: an `impl Trait for Target` is rejected if neither the trait nor the target type is declared in the current module.

## Generic Constraints & Trait Bounds

* Added single trait bound constraints on type parameters: `fn printItem<T: Printable>(T item)`.
* Added multiple trait bound syntax using `+`: `fn process<T: Printable + Summarizable>(T item)`.
* Trait bounds are supported on generic functions, generic structs, generic traits, and generic `impl` blocks.
* Added compile-time trait bound satisfaction checking during type unification and invocation.

## Language Protocols & Operator Hooks

* **Comparison Protocol**: Implementing `compare(self, TargetType other) -> int` overrides `<`, `<=`, `>`, `>=`, `==`, and `!=`.
* **Equality Protocol**: Implementing `equals(self, TargetType other) -> bool` overrides `==` and `!=`.
* **Display Protocol**: `display.show(val)` automatically invokes `toString(self) -> string` when implemented.
* **Custom Iterator Protocol**: `for item in iterable do` automatically drives any custom type implementing `next(change self) -> Option<T>` until `Option.None`.

## Release 9 Summary

Release 9 expands Y9+ into a trait-oriented, protocol-driven language by adding:

* `trait` declarations with default methods
* `impl` blocks for inherent methods and trait implementations
* Static methods and constructors
* `Self` type and receiver mutability (`self` vs `change self`)
* Trait implementations for primitive types
* Single and multiple trait bounds (`<T: Trait1 + Trait2>`)
* Orphan rule coherence enforcement
* Operator overloading via `compare` and `equals`
* Custom `for..in` iteration protocol via `next()`
* Display integration via `toString()`
* Static immutability path enforcement