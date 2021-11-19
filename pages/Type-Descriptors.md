

Next: [Autoload Type](Autoload-Type.html), Previous: [Record Type](Record-Type.html), Up: [Programming Types](Programming-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.4.18 Type Descriptors

A *type descriptor* is a `record` which holds information about a type. Slot 1 in the record must be a symbol naming the type, and `type-of` relies on this to return the type of `record` objects. No other type descriptor slot is used by Emacs; they are free for use by Lisp extensions.

An example of a type descriptor is any instance of `cl-structure-class`.
