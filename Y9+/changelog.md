# Release Number: 2026.08.10 - 7

## First-Class Functions, Lambdas & Lexical Closures

* Functions are now first-class citizens.
* Functions can be stored in variables, passed as arguments, and returned from function calls.
* Added anonymous lambda syntax: `fn(params) -> ReturnType { ... }`.
* Added lexical closure scope capturing.
* Lambdas can capture variables from their surrounding lexical scopes.
* Added functional collection methods to arrays:

  * `.forEach()`
  * `.filter()`
  * `.map()`
  * `.reduce()`

## Native Map Collection Type (`map[K]V`)

* Added native Map data structure using `map[KeyType]ValueType`.
* Added map literals using `{ key: value }`.
* Added map indexing using `map[key]`.
* Added map methods:

  * `.get()`
  * `.set()`
  * `.has()`
  * `.remove()`
  * `.keys()`

## Pattern Matching (`match`)

* Added `match` statements and expressions.
* Supports direct value matching.
* Supports wildcard matching using `case _`.
* Supports type matching using cases such as `case int` and `case string`.
* Supports fallback cases.

## HTTP, Networking & WebSockets

* Added native HTTP client functions: `http.get()` and `http.post()`.
* Added native HTTP server creation using `http.listen(port, handlerFn)`.
* Added native TCP socket server support using `net.listen(port, handlerFn)`.
* Added native WebSocket server endpoint support using `ws.server(port, handlerFn)`.

## Multithreading & Synchronization

* Added thread spawning using `thread.spawn(scriptPath, args)`.
* Added thread management using `thread.join(id)` and `thread.isActive(id)`.
* Added thread synchronization primitives:

  * `sync.mutexCreate()`
  * `sync.mutexLock(id)`
  * `sync.mutexUnlock(id)`

## Operating System, Process & Environment APIs

* Added OS inspection methods:

  * `os.platform()`
  * `os.arch()`
  * `os.hostname()`
  * `os.tmpdir()`
  * `os.homedir()`
  * `os.cpus()`
  * `os.uptime()`
  * `os.totalMem()`
  * `os.freeMem()`
* Added process control methods:

  * `process.exec()`
  * `process.exit()`
  * `process.pid()`
  * `process.cwd()`
  * `process.chdir()`
  * `process.args()`
* Added environment variable manipulation:

  * `env.get()`
  * `env.set()`
  * `env.has()`
  * `env.remove()`
  * `env.all()`
* Added time and date functionality:

  * `time.now()`
  * `time.sleep()`
  * `time.dateString()`

## JSON, Cryptography, Database & Compression

* Added JSON serialization and parsing:

  * `json.parse()`
  * `json.stringify()`
* Added cryptographic hashing and encoding:

  * `crypto.sha256()`
  * `crypto.md5()`
  * `crypto.base64Encode()`
  * `crypto.base64Decode()`
* Added file-backed key-value storage:

  * `db.set()`
  * `db.get()`
  * `db.has()`
  * `db.delete()`
  * `db.all()`
  * `db.clear()`
* Added compression:

  * `compress.gzip()`
  * `compress.gunzip()`
  * `compress.deflate()`
  * `compress.inflate()`

## Data Analysis & Statistics

* Added CSV parsing and stringification:

  * `data.parseCsv()`
  * `data.toCsv()`
* Added statistical computation functions:

  * `stats.sum()`
  * `stats.mean()`
  * `stats.median()`
  * `stats.variance()`
  * `stats.stdDev()`
  * `stats.min()`
  * `stats.max()`

## User-Defined Exception Throwing

* Added user-defined exception throwing using `throw`.
* User code can now raise custom runtime errors.
* Custom thrown errors can be caught using `try / catch`.

## Internal Architecture & Refactoring

* Modularized the type checker component into `src/checker/`.
* Split checker functionality into:

  * `context.ts`
  * `expressions.ts`
  * `statements.ts`
  * `checker.ts`
* Improved internal maintainability and separation of type-checking responsibilities.

## Release 7 Summary

Release 7 significantly expands Y9+ beyond its previous core language features by adding:

* First-class functions
* Lambdas
* Lexical closures
* Higher-order array operations
* Native maps
* Pattern matching
* HTTP
* TCP networking
* WebSockets
* Multithreading
* Mutex synchronization
* OS APIs
* Process APIs
* Environment variables
* Time/date APIs
* JSON
* Cryptography
* File-backed databases
* Compression
* CSV processing
* Statistical utilities
* User-defined exception throwing
* Improved internal compiler/type-checker architecture
