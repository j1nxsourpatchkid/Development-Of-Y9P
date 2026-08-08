# KNOWN LIMITATIONS

This is an honest list of what Y9+ does not support yet. If something isn't
listed here or in the syntax docs, assume it doesn't exist yet.

## No user-defined functions

Only `entry main()` exists. You cannot declare your own functions yet.

## @bring does nothing

`@bring /io.standard/` is parsed and stored, but it is never checked or
enforced. Removing it currently has no effect on whether the program runs.

## No logical operators

There is no `&&` or `||`. You can only combine expressions using the currently
supported arithmetic and comparison operators.

## No arrays, lists, or collections

Only single values are supported. There is no way to group multiple values
together.

## No loops

There is currently no loop syntax such as `while`, `for`, or `foreach`.

## Limited error messages

Errors currently report what went wrong but not the line or column number
in the source file.

## Documentation comments are not yet functional

`///` is recognized and parsed the same as `//`, but there is no
documentation-generation tool or special handling for `///` comments yet.

They are purely decorative right now, identical to a regular `//` comment.
