# Release Number: 2026.09.16 - 10

## Resilient Parser & Diagnostic Engine

* Replaced single-crash errors with a multi-diagnostic collector (`DiagnosticBag`).
* Added panic-mode parser synchronization to report multiple syntax and type errors in one run.
* Added terminal code rendering with line numbers, file paths, and ASCII caret pointers (`^`) highlighting error locations.

## Language Ergonomics

* **String Interpolation**: Added formatted strings using `$"Hello {name}"` and backticks (`` `...` ``).
* **Bitwise Operators**: Added `&`, `|`, `^`, `~`, `<<`, `>>` and compound assignments `&=`, `|=`, `^=`, `<<=`, `>>=`.
* **Ergonomic Unwrap (`?`)**: Added postfix unwrap operator for `Option<T>` and `Result<T, E>`.

## Native Testing Framework (`y9 test`)

* Added `test "name" { ... }` block declaration syntax.
* Added native `assert(condition, message?)` statement.
* Added `y9 test <file.y9>` CLI command with execution timers and pass/fail summaries.

## Background Multithreading

* Replaced synchronous process execution with Node.js `worker_threads`.
* Background threads now execute concurrently on separate OS thread pools with `SharedArrayBuffer` and `Atomics`.

## Unified CLI Toolchain & REPL

* Introduced the `y9` CLI suite:
  * `y9 run <file>`: Typecheck and execute.
  * `y9 check <file>`: Static analysis check without running.
  * `y9 test <file>`: Run test suites.
  * `y9 fmt <file>`: Automatic source code formatter.
  * `y9 repl`: Interactive REPL.