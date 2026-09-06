# Release Number: 2026.09.16 - 11

## Deterministic Resource Cleanup (`defer`)

* Added `defer { ... }` block syntax.
* Deferred statements execute in reverse (LIFO) order upon exiting a function or `entry main`.
* Cleanup is guaranteed across standard returns, normal function completion, and early-return error propagation.

## Idiomatic Error Propagation (`?`)

* Upgraded `?` from fatal runtime aborts to automatic early error bubbling.
* Using `expr?` in a function returning `Option<T>` returns `Option.None` automatically on missing values.
* Using `expr?` in a function returning `Result<T, E>` returns `Result.Err` automatically on errors without matching boilerplate.

## Module Import Aliasing & Specifiers

* **Import Aliasing**: Added support for `@bring "path" as Alias;` to eliminate namespace collisions.
* **Selective Imports**: Added support for `@bring { funcA, funcB } from "path";` to import symbols directly into local scope.

## Systems Concurrency & Binary IO Engine

* **Channels (`parallel.y9`)**: Added lock-free inter-thread communication channels (`create_channel()`, `send()`, `recv()`).
* **Binary Buffers (`io.y9`)**: Added low-level raw byte buffer allocation and UTF-8 string streaming (`alloc()`, `write()`, `read()`).

## Vectorized Machine Learning Primitives (`ml.y9`)

* Backed `ml.y9` with native engine operations:
  * 2D Matrix multiplication (`matmul`).
  * Vector dot products (`dot`).
  * Native activation functions (`relu` and `sigmoid`).

## IDE Foundation (`y9 ast`)

* Added `y9 ast <file.y9>` CLI command to emit syntax trees in clean JSON format for the Release 13 IDE.