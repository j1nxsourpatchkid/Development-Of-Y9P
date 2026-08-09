# Release Number: 2026.08.09 - 6

## User-defined functions (`fn`)

* Declare custom functions with `fn name(params) -> ReturnType { ... }`.
* Return type is optional; missing `->` means a void function.
* Parameters can be made mutable with `change`.
* Runtime checks enforce argument counts, parameter types, and return types.
* Functions can now be declared anywhere at file scope, including below `entry main()`.
* Recursive function calls are supported.

## Structs

* Define custom data types using `struct`.
* Struct fields are immutable by default.
* Fields can be made mutable with `change`.
* Structs can be instantiated using their type name.
* Access struct fields using dot notation (`person.name`).
* Mutable fields can be reassigned.
* Structs support deep equality with `==` and `!=`.
* Structs can be stored in arrays.
* Arrays of structs are supported.

## Strings

* String property `text.length` returns the number of characters.
* String indexing (`text[0]`) returns a `chr`.
* Strings can be iterated using `for ... in ... do`.
* Added string methods:

  * `indexOf()`
  * `substring()`
  * `split()`
  * `trim()`
  * `replace()`
  * `toUpper()`
  * `toLower()`
* Strings remain immutable.

## Arrays

* Deep array equality is now supported with `==` and `!=`.
* Added `indexOf(value)` – returns the index of an element.
* Added `slice(start, end)` – returns a portion of an array.
* Added `concat(other)` – combines two arrays.
* Existing `push()`, `pop()`, and `remove()` methods remain supported.

## Collection iteration

* Added `for ... in ... do` iteration for arrays.
* Added `for ... in ... do` iteration for strings.
* Array iteration returns each element.
* String iteration returns each character as a `chr`.

## Mathematical operators

* Added modulo operator `%`.
* Added compound modulo assignment `%=`.

## Ternary operator

* Added conditional expressions using `condition ? true_expr : false_expr`.
* Ternary expressions short-circuit evaluation.
* Ternary precedence is below `||` and above assignment.

## Top-level global variables

* Variables can now be declared at file scope outside functions and `entry main()`.
* Globals are accessible from functions.
* Globals are immutable by default.
* `change` can be used for mutable globals.
* Globals can be exported from modules.
* Local variables can shadow global variables.

## Function hoisting

* Functions can now be declared above or below `entry main()`.
* Functions must still be declared at file scope.

## CLI arguments

* `entry main()` can now optionally accept a `string[] args` parameter.
* Command-line arguments are provided as a Y9+ string array.
* The original `entry main()` syntax remains valid.
* Programs without an explicit return default to exit code `0`.

## `@bring` modules

* `@bring` is now functional.
* Local `.y9` files can be imported using paths such as `@bring ./helpers.y9/`.
* Standard library modules can be imported using paths such as `@bring /std/math/`.
* Imported modules support explicit exports.
* Module scope is isolated.
* Circular module dependencies are detected.

## Standard library

* Added `/std/math/`.
* Added `/std/io/`.
* Added `/std/fs/`.
* Math functionality includes common operations such as `abs`, `sqrt`, `floor`, `ceil`, `round`, `pow`, `min`, `max`, `sin`, `cos`, and `tan`.
* File-system functionality includes file reading, writing, and existence checks.
* Standard library functionality builds on Y9+'s native runtime APIs.

## Language-level error handling

* Added `try do ... catch (...) do ...`.
* Runtime errors can now be caught without terminating the entire program.
* The caught error is provided to the `catch` block as a `string`.

## Precise runtime error locations

* Runtime errors now include line and column numbers.
* Runtime errors use the format `[line:col] Runtime Error: ...`.
* Runtime source locations are preserved through expression and statement evaluation.

## Breaking changes from Release 5

* `@bring` is now an active module system instead of being parsed and ignored.
* Functions may now appear below `entry main()`.
* Top-level global variables are now supported.
* `entry main(string[] args)` is now supported.
* Runtime errors now include source line and column information.
* Strings now support `.length` and indexing.
* Arrays now support deep equality and additional methods.
* `try / catch` is now supported.
* Structs are now supported.
