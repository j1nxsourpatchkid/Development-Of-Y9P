# Release Number: 2026.08.17 - 8

## Parametric Generic Functions

* Added support for type parameters in functions: `fn identity<T>(T value) -> T`.
* Added support for multiple generic type parameters: `fn pair<T, U>(T a, U b) -> Pair<T, U>`.
* Added automatic generic type argument inference at call sites (e.g. `identity(42)` automatically infers `T = int`).
* Added explicit type argument invocation syntax (e.g. `identity<int>(42)`).
* Generic functions support type-safe returns and parameter passing.

## Generic Structs

* Added generic type parameters to struct declarations: `struct Box<T> { T value; }`.
* Added multi-parameter generic structs: `struct Pair<T, U> { T first; U second; }`.
* Added support for arbitrarily nested generic types: `Box<Box<int>>`.
* Added recursive generic field type substitution upon property access and mutation.
* Constructor calls automatically infer or validate generic type arguments: `Box(42)` produces `Box<int>`.

## Enums & Algebraic Data Types (ADTs)

* Added user-defined `enum` declarations with tagged union variants.
* Variants can be unit variants (`None`) or carry typed payload fields (`Some(T value)`).
* Added generic enums enabling algebraic data types such as `Option<T>` and `Result<T, E>`:
  * `enum Option<T> { Some(T value), None }`
  * `enum Result<T, E> { Ok(T value), Err(E error) }`
* Added variant instantiation syntax: `Option.Some(42)` and `Option<int>.Some(42)`.
* Variant fields are statically type-checked at construction.

## Pattern Matching with Enum Destructuring

* Extended `match` statements and expressions to match against enum variants.
* Added payload destructuring: binds variant payload fields directly into typed local variables in match arms.
* Added static type propagation into match case bindings via generic type substitution.
* Added static exhaustiveness checking for enum matches: verifies that all variants of an enum are covered when no `else` fallback is provided.

## Generic Higher-Order Functions & Lambdas

* Lambdas now support type parameters: `fn<T>(T value) -> T { return value; }`.
* Added generic higher-order functions: `fn apply<T, U>(T val, fn(T) -> U f) -> U`.
* Generic functions seamlessly accept lambdas and closures with inferred parameter and return types.

## Cross-Module Generic Types

* Generic structs, enums, and functions can be exported using `export`.
* Imported modules accessed via `@bring` retain generic type signatures across module boundaries.
* Added module-namespaced generic instantiation and variant access (e.g. `containers.Box<int>` or `containers.Option.Some(10)`).

## Type Checker & Compiler Machinery

* **Bidirectional Type Unification**: Implemented `unifyTypes()` to infer type arguments from call-site arguments automatically.
* **Type Substitution Engine**: Implemented `substituteType()` to recursively replace type parameters with concrete types across nested structs, enums, and functions.
* **Type Parameter Scoping**: Added lexical type parameter scopes to prevent naming collisions and illegal shadowing.
* **Generic Arity & Safety Validation**: The checker enforces exact type argument counts and treats different generic instantiations (such as `Box<int>` and `Box<string>`) as distinct, incompatible types.
* **Parser Disambiguation**: Added speculative lookahead and backtracking to disambiguate `<` as a comparison operator vs the opening of generic type arguments (`<T>`).

## Release 8 Summary

Release 8 expands Y9+ into a strongly-typed, parametrically polymorphic language by adding:

* Generic functions
* Generic structs
* Generic enums / Algebraic Data Types (ADTs)
* Pattern matching with enum variant destructuring
* Match exhaustiveness checking
* Automatic generic type inference
* Explicit generic type arguments
* Nested generic types (`Box<Box<T>>`)
* Generic higher-order functions and lambdas
* Cross-module generic exports and imports
* Type unification and recursive substitution engine
* Disambiguated generic parser grammar