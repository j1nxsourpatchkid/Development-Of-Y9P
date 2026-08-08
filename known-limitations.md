KNOWN LIMITATIONS

This is an honest list of what Y9+ does not support yet. If something isn't
listed here or in the syntax docs, assume it doesn't exist yet.

## No control flow

There is no if, else, or any form of loop (while, for, etc.). Every program
runs top to bottom with no branching or repetition. This is now unblocked
at the expression level since comparisons produce real booleans, but if/loops
themselves are not implemented yet.

## No user-defined functions

Only entry main() exists. You cannot declare your own functions yet.

## @bring does nothing

@bring /io.standard/ is parsed and stored, but it is never checked or
enforced. Removing it currently has no effect on whether the program runs.

## No logical operators

There is no && or ||. You can only test one comparison at a time.

## No arrays, lists, or collections

Only single values are supported. There is no way to group multiple values
together.

## Limited error messages

Errors currently report what went wrong but not the line or column number
in the source file.

## Documentation comments are not yet functional

/// is recognized and parsed the same as //, but there is no doc-generation
tool or special handling for /// comments yet. They are purely decorative
right now, identical to a regular // comment.
