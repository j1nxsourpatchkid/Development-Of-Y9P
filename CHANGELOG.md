# Release Number: 2026.08.08 - 5

## User‑defined functions (`fn`)

* Declare custom functions with `fn name(params) -> ReturnType { ... }`.
* Return type is optional; missing `->` means a void function.
* Parameters can be made mutable with `change`.
* Runtime checks enforce argument counts, parameter types, and return types.

## Arrays

* Array type written as `type[]` (e.g. `int[]`, `string[]`).
* Array literals: `[1, 2, 3]`; empty `[]` allowed with explicit type.
* Indexing: `arr[0]` (read) and `arr[0] = val` (write) with bounds checks.
* Property `arr.length` – returns the number of elements.
* Methods:
  * `push(val)` – appends element, returns new length.
  * `pop()` – removes and returns last element.
  * `remove(index)` – removes and returns element at index.

## Logical operators

* `&&` (AND) and `||` (OR) with short‑circuit evaluation.
* Unary `!` (NOT) for booleans.

## Precise error locations

* Syntax errors now include line and column numbers: `[line:col] Syntax Error: …`.

## Breaking changes from Release 4

* `num` renamed to `int`.
* `mut` replaced by `change`.
  * Before: `mut num x = 5;`
  * Now: `change int x = 5;`