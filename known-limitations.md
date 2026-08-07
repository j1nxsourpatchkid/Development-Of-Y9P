KNOWN LIMITATIONS

This is an honest list of what Y9+ does not support yet. If something isn't
listed here or in the syntax docs, assume it doesn't exist yet.

## No expressions

You cannot combine values. Only a single literal or a single variable is
allowed on the right-hand side of an assignment or declaration.

Works:
num score = 5;
score = age;

Does not work:
num score = 5 + 3;
score = age + 1;

## No control flow

There is no if, else, or any form of loop (while, for, etc.). Every program
runs top to bottom with no branching or repetition.

## No user-defined functions

Only entry main() exists. You cannot declare your own functions yet.

## @bring does nothing

@bring /io.standard/ is parsed and stored, but it is never checked or
enforced. Removing it currently has no effect on whether the program runs.

## No string concatenation

You cannot combine strings, e.g. "Hello, " + name is not supported.

## No comparison or logical operators

There is no ==, !=, <, >, &&, ||, or similar. Booleans can only be set
directly to true or false.

## No arrays, lists, or collections

Only single values are supported. There is no way to group multiple values
together.

## Limited error messages

Errors currently report what went wrong but not the line or column number
in the source file.

There is currently no syntax for writing comments inside a .y9 file.

## Documentation comments are not yet functional

/// is recognized and parsed the same as //, but there is no doc-generation
tool or special handling for /// comments yet. They are purely decorative
right now, identical to a regular // comment.
