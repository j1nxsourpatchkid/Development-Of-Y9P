# Y9+ Language Features

Everything Y9+ can do as of Release 7.

---

## Program Structure

* Every program requires an `entry main()` function.
* `entry main()` can take no arguments or accept command-line arguments as a `string[] args`.
* Returning an exit code (e.g. `return 0;`) is optional in `entry main()`.
* If `entry main()` finishes without reaching a return statement, execution automatically returns `0`.
* User-defined functions (`fn`) are declared at file scope.
* Functions may be declared above or below `entry main()`.
* Struct declarations may appear at the top level.
* Global variable declarations may appear at the top level.
* `export` can be used on top-level functions, structs, and global variables.
* `@bring` imports can import standard library modules or source files.

```y9
fn add(int a, int b) -> int
{
    return a + b;
}

entry main()
{
    int result = add(5, 3);
    display.show(result);
}
```

`entry main()` may also receive command-line arguments:

```y9
entry main(string[] args)
{
    if (args.length > 0) do
    {
        display.show(args[0]);
    }

    return 0;
}
```

---

## Modules & Imports

Y9+ uses `@bring` to import modules.

Standard library modules can be imported using a standard library path:

```y9
@bring /std/math;
@bring /std/io;
@bring /std/fs;
```

Source files can also be imported using a relative path:

```y9
@bring "./utils.y9";
```

Circular module dependencies are detected and reported as import errors.

Top-level declarations can be exported using `export`:

```y9
export fn add(int a, int b) -> int
{
    return a + b;
}
```

Structs and global variables can also be exported:

```y9
export struct Player
{
    string name;
    change int score;
}

export change int callCount = 0;
```

Members exported by an imported module can be accessed using the module filename as a namespace:

```y9
@bring "./math_utils.y9";

entry main()
{
    math_utils.Vector2 v = math_utils.Vector2(1.0, 2.0);
    display.show(v);
}
```

---

## Output

`display.show(expr)` prints a value. It accepts literals, variables, expressions, arrays, maps, and structs.

```y9
display.show("Hello");
display.show(42);
display.show(x + 1);
display.show([1, 2, 3]);
```

---

## Input

### Expression Form

Returns a value. A prompt is optional.

```y9
string name = input.read();
int age = input.read("Enter your age: ");
```

### Statement Form

Writes directly into an existing mutable variable. A prompt is optional.

```y9
change int x = 0;

input.read(x);
input.read("Enter value: ", x);
```

Supported input types:

* `int`
* `float`
* `deci`
* `bool`
* `chr`
* `string`

Invalid input causes a runtime error.

When no expected type is available, `input.read()` defaults to `string`.

---

## Variables

Six primitive types are supported:

* `int` — integer
* `float`
* `deci`
* `bool`
* `chr`
* `string`

Types must be explicitly declared unless a value is being stored in an `auto` variable.

Variables are immutable by default. Use `change` to allow reassignment.

Type checking is enforced at runtime.

```y9
int x = 10;
change float y = 3.14;
bool flag = true;
string s = "hello";
chr c = 'A';
```

`auto` can be used when the type is inferred from the assigned value.

```y9
auto number = 42;
auto message = "Hello";
```

Global variables may also be declared at the top level:

```y9
change int callCount = 0;
```

Top-level global variables can be exported from modules using `export`.

---

## Structs

Structs are user-defined composite data types with named fields.

### Declaration

Fields are immutable by default. Use `change` to make a field mutable.

```y9
struct Player
{
    string name;
    change int score;
    change float health;
}
```

### Instantiation

Structs are instantiated by calling the struct name with field values in declaration order.

```y9
Player p = Player("Alice", 100, 98.5);
```

### Field Access

Fields are accessed using dot notation.

```y9
display.show(p.name);

p.score += 50;
p.health = 100.0;
```

Attempting to modify an immutable field produces a runtime error.

```y9
// p.name = "Bob"; // Invalid
```

### Structural Comparison

Struct instances support deep equality comparison with `==` and `!=`.

```y9
Player p1 = Player("Alice", 100, 100.0);
Player p2 = Player("Alice", 100, 100.0);

bool identical = p1 == p2;
```

Structs can be exported from modules using `export`.

---

## Arrays

Arrays are ordered, dynamically sized collections of elements.

### Declaration & Literals

Array types are declared using `type[]`.

Array literals use square brackets.

```y9
int[] numbers = [1, 2, 3, 4, 5];
change string[] names = ["Alice", "Bob"];
change int[] emptyList = [];
```

Numeric elements inside array literals are automatically widened when types are mixed.

For example, `[1, 2.5]` produces a `float[]`.

### Indexing & Assignment

Arrays use 0-based indexing for reading and writing.

```y9
int first = numbers[0];

numbers[0] = 99;
```

Accessing an out-of-bounds index produces a runtime error.

### Array Property

`.length` returns the number of elements in the array as an `int`.

```y9
int len = numbers.length;
```

### Array Methods

`.push(val)` appends a value to the end of the array and returns the new length.

`.pop()` removes and returns the last element.

`.remove(index)` removes and returns the element at the specified index.

`.indexOf(val)` returns the index of a value, or `-1` when the value is not found.

`.slice(start, end)` returns a sub-array from `start` up to, but not including, `end`.

`.concat(other)` returns a new array containing the elements of both arrays.

`.forEach(fn)` executes a function for every element.

`.filter(fn)` returns a new array containing elements for which the function returns `true`.

`.map(fn)` returns a new array containing the transformed elements.

`.reduce(initial, fn)` reduces the array to a single value.

```y9
change int[] items = [10, 20, 30];

items.push(40);

int last = items.pop();

int item = items.remove(0);

int index = items.indexOf(30);

int[] sub = items.slice(0, 1);

int[] merged = items.concat([50, 60]);

items.forEach(fn(int value) {
    display.show(value);
});

int[] doubled = items.map(fn(int value) -> int {
    return value * 2;
});
```

### Array Deep Equality

Arrays support deep structural comparison using `==` and `!=`.

```y9
bool same = ([1, 2, 3] == [1, 2, 3]);
```

---

## Maps

Maps store key-value associations using the `map[K]V` type.

### Declaration & Literals

```y9
map[string]int scores = {
    "Alice": 100,
    "Bob": 90
};
```

### Indexing

Maps support key-based indexing.

```y9
int score = scores["Alice"];
```

### Map Methods

`.get(key)` retrieves a value.

`.set(key, value)` inserts or updates a value.

`.has(key)` checks whether a key exists.

`.remove(key)` removes a key-value pair.

`.keys()` returns the map's keys as an array.

```y9
map[string]string user = {
    "name": "Alice"
};

user.set("role", "admin");

string name = user.get("name");

bool hasRole = user.has("role");

string[] keys = user.keys();

user.remove("role");
```

---

## Functions

Custom functions are declared using `fn`.

### Syntax

```y9
fn doubleValue(int n) -> int
{
    return n * 2;
}
```

Functions may accept zero or more parameters.

Parameters are immutable by default. Use `change` before a parameter type to make it mutable within the function.

```y9
fn increment(change int val) -> int
{
    val += 1;
    return val;
}
```

Return type annotations use `-> Type`.

Functions that omit `-> Type` are treated as void functions.

```y9
fn sayHello()
{
    display.show("Hello");
}
```

Functions can be called from `entry main()` or from other functions.

```y9
int result = doubleValue(5);
```

Functions support recursion.

Functions are also first-class values.

---

## First-Class Functions, Lambdas & Closures

Functions can be stored in variables, passed as arguments, and returned from other functions.

### Anonymous Lambdas

Lambdas use `fn(params) -> ReturnType { ... }` syntax.

```y9
auto doubleVal = fn(int x) -> int
{
    return x * 2;
};

display.show(doubleVal(5));
```

### Closures

Lambdas can capture variables from surrounding lexical scopes.

```y9
int factor = 3;

auto multiplier = fn(int x) -> int
{
    return x * factor;
};

display.show(multiplier(4));
```

Captured variables remain available to the closure after the surrounding expression or function scope has ended.

---

## Pattern Matching

Y9+ supports structural pattern matching using `match`.

### Value Matching

```y9
int code = 200;

match (code)
{
    case 200 do display.show("OK");
    case 404 do display.show("Not Found");
    case _ do display.show("Unknown");
}
```

### Type Matching

```y9
match (value)
{
    case int do display.show("It's an int");
    case string do display.show("It's a string");
    case _ do display.show("Other type");
}
```

`case _` acts as a wildcard fallback.

---

## Comments

```y9
// Single-line comment
```

`///` documentation comments are recognized but currently have no special behavior.

They are parsed the same way as regular `//` comments and are currently decorative only.

---

## Arithmetic

Basic operators:

* `+`
* `-`
* `*`
* `/`
* `%`

Multiplication and division have higher precedence than addition and subtraction.

Modulo (`%`) returns the remainder of a numeric division.

```y9
int remainder = 10 % 3;
```

Numeric type widening:

```text
int → float → deci
```

when mixing numeric types.

```y9
int a = 5 + 3 * 2;
float b = 10 / 3.0;
```

Unary negation works on numeric literals, variables, and expressions.

```y9
int c = -5;
int d = -a;
```

Division or modulo by zero causes a runtime error.

### String Operators

`+` concatenates strings:

```y9
string a = "Hello" + " World";
```

`-` removes all occurrences of a substring:

```y9
string a = "banana" - "a";
```

Result:

```text
bnn
```

`*` repeats a string by an integer:

```y9
string a = "ab" * 3;
```

Result:

```text
ababab
```

String concatenation also supports combining a string with other values:

```y9
string message = "Score: " + 100;
```

String repetition supports either operand order:

```y9
string a = "ab" * 3;
string b = 3 * "ab";
```

---

## Logical Operators

Supported operators:

* `&&` — Logical AND
* `||` — Logical OR
* `!` — Logical NOT

Logical operators operate on `bool` values.

`&&` and `||` use short-circuit evaluation.

```y9
bool valid = (x > 0) && (x < 100);
bool check = isReady || !hasFailed;
```

---

## Compound Assignment

Supported operators:

* `+=`
* `-=`
* `*=`
* `/=`
* `%=`

Numeric types (`int`, `float`, `deci`) support all five.

Strings support `+=` for concatenation.

Compound assignment requires the target variable or mutable struct field to be mutable.

```y9
change int x = 10;

x += 5;
x -= 3;
x *= 2;
x /= 2;
x %= 3;
```

---

## Comparisons

Supported operators:

* `==`
* `!=`
* `<`
* `>`
* `<=`
* `>=`

All comparisons produce a `bool`.

Comparisons work across numeric types using numeric widening.

Strings and chars support lexicographic comparison.

Bools support equality comparisons (`==`, `!=`) only.

Arrays, maps, and structs support appropriate equality comparisons.

```y9
bool b1 = 5 < 10;
bool b2 = "abc" == "abc";
bool b3 = true != false;

bool b4 = ([1, 2] == [1, 2]);
```

---

## Control Flow

### if / elif / else

```y9
if (condition) do
{
    // block
}
elif (condition) do
{
    // block
}
else do
{
    // block
}
```

The condition must evaluate to `bool`.

Both block and single-statement forms are supported.

```y9
if (x > 5) do display.show("big");
```

### Ternary Expression

Y9+ supports conditional ternary expressions:

```y9
int maxVal = (a > b) ? a : b;
```

The condition must evaluate to `bool`.

---

## Loops

### while

```y9
while (condition) do
{
    // body
}
```

Repeats while the condition evaluates to `true`.

### for — Range-Based

```y9
for i in 0..10 do
{
    display.show(i);
}
```

The end value is exclusive.

An optional `step` is supported:

```y9
for i in 0..10 step 2 do
{
    display.show(i);
}
```

Negative steps are supported:

```y9
for i in 10..0 step -1 do
{
    display.show(i);
}
```

### for — Iterable

The `for` loop can iterate directly over arrays and strings.

```y9
int[] nums = [10, 20, 30];

for item in nums do
{
    display.show(item);
}
```

Strings produce a `chr` for each character:

```y9
for ch in "Y9+" do
{
    display.show(ch);
}
```

### for — C-Style

```y9
for (change int i = 0; i < 10; i += 1) do
{
    display.show(i);
}
```

---

## Loop Control

### break

```y9
break;
```

Immediately exits the nearest enclosing loop.

### next

```y9
next;
```

Skips the remainder of the current iteration and proceeds to the next iteration.

---

## Error Handling

### try / catch

`try` / `catch` blocks catch runtime errors occurring inside the `try` block.

The `catch` parameter must be explicitly typed as `string`.

```y9
try do
{
    int zero = 0;
    int result = 10 / zero;
}
catch (string err) do
{
    display.show("Caught error: " + err);
}
```

Y9+ also supports user-defined exception throwing.

```y9
throw "Something went wrong";
```

Custom exceptions can be caught using `try / catch`.

---

## Return

`return expr;` immediately exits a function with the evaluated expression result.

`entry main()` may explicitly return an integer-compatible `int`, but an explicit return is optional.

If `entry main()` finishes without reaching a return statement, it automatically returns `0`.

Void functions can use `return;` without an expression.

---

## Scoping

Y9+ uses lexical block scoping.

Variables declared inside `{ }` are not visible outside that block.

Loop variables declared by `for` are scoped to the loop.

Function parameters and local variables are scoped to their function.

Struct fields are scoped to their containing struct and are accessed through a struct instance.

Top-level global variables have module-level scope.

Closures can capture variables from their surrounding lexical scope.

---

## Standard Library & Native APIs

Y9+ provides a growing standard library and native runtime API surface accessible through `@bring`.

### Mathematics

The math library provides common mathematical operations:

```text
abs
sqrt
floor
ceil
round
pow
min
max
sin
cos
tan
```

### Filesystem & IO

Y9+ provides filesystem and IO functionality for reading, writing, and managing data.

### HTTP

HTTP client functionality:

```y9
string response = http.get("https://example.com");

string result = http.post(
    "http://localhost:8080/data",
    "{\"key\":\"value\"}"
);
```

HTTP servers can be created using:

```y9
http.listen(8080, fn(string req) -> string
{
    return "Hello from Y9+ HTTP Server!";
});
```

### Networking

TCP server functionality is available through:

```y9
net.listen(9000, fn(string msg) -> string
{
    return "Echo: " + msg;
});
```

### WebSockets

WebSocket server endpoints can be created using:

```y9
ws.server(8081, fn(string msg)
{
    display.show("WS Received: " + msg);
});
```

### Threads & Synchronization

Y9+ provides threading and mutex synchronization:

```y9
int threadId = thread.spawn("./worker.y9", "arg1");

bool active = thread.isActive(threadId);
string output = thread.join(threadId);

int mutexId = sync.mutexCreate();

sync.mutexLock(mutexId);
// Critical section
sync.mutexUnlock(mutexId);
```

### Operating System APIs

Y9+ can inspect operating-system information:

```y9
string platform = os.platform();
string architecture = os.arch();
string hostname = os.hostname();
string home = os.homedir();

int cpus = os.cpus();
int memory = os.freeMem();
```

### Process Management

```y9
string output = process.exec("command");
int pid = process.pid();
string cwd = process.cwd();

process.chdir("path");
process.exit(0);
```

### Environment Variables

```y9
env.set("PORT", "8080");

string port = env.get("PORT");
bool exists = env.has("PORT");

env.remove("PORT");
map[string]string variables = env.all();
```

### Time

```y9
int now = time.now();

time.sleep(1000);

string date = time.dateString();
```

### JSON

```y9
string jsonStr = json.stringify(myMap);
auto obj = json.parse(jsonStr);
```

### Cryptography

```y9
string hash = crypto.sha256("password");
string md5 = crypto.md5("password");

string encoded = crypto.base64Encode("text");
string decoded = crypto.base64Decode(encoded);
```

### File-Backed Database

```y9
db.set("./store.json", "user1", "Alice");

string val = db.get("./store.json", "user1");

bool exists = db.has("./store.json", "user1");

db.delete("./store.json", "user1");
```

### Compression

```y9
string compressed = compress.gzip("long text");
string original = compress.gunzip(compressed);
```

### Data Analysis

CSV parsing and generation:

```y9
string[][] table = data.parseCsv(csv);
string csvOutput = data.toCsv(table);
```

Statistical operations include:

```text
sum
mean
median
variance
stdDev
min
max
```

---

## Escape Sequences

Strings and character literals support escape sequences.

Supported sequences include:

```text
\n  Newline
\t  Horizontal Tab
\r  Carriage Return
\\  Backslash
\"  Double Quote
\'  Single Quote
```

Example:

```y9
string msg = "Hello\n\tWorld!";
chr newline = '\n';
```

---

## Full Example

```y9
@bring /std/math;

struct Player
{
    string name;
    change int score;
}

fn isEven(int val) -> bool
{
    return val % 2 == 0;
}

fn filterEvens(int[] nums) -> int[]
{
    return nums.filter(fn(int value) -> bool
    {
        return isEven(value);
    });
}

fn mainScore(int[] nums) -> int
{
    return nums.reduce(0, fn(int acc, int value) -> int
    {
        return acc + value;
    });
}

entry main(string[] args)
{
    Player player = Player("Alice", 0);

    int[] numbers = [1, 2, 3, 4, 5, 6, 7, 8];

    int[] evensOnly = filterEvens(numbers);

    player.score = mainScore(evensOnly);

    display.show("Player: " + player.name);
    display.show("Score: " + player.score);

    map[string]int data = {
        "score": player.score,
        "count": evensOnly.length
    };

    match (player.score)
    {
        case 20 do display.show("Perfect");
        case int do display.show("Integer score");
        case _ do display.show("Unknown");
    }

    try do
    {
        if (player.score < 0) do
        {
            throw "Invalid score";
        }
    }
    catch (string err) do
    {
        display.show("Caught error: " + err);
    }
}
```
