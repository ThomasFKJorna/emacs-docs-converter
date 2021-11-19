

Next: [Writing Emacs Primitives](Writing-Emacs-Primitives.html), Previous: [Memory Usage](Memory-Usage.html), Up: [GNU Emacs Internals](GNU-Emacs-Internals.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### E.6 C Dialect

The C part of Emacs is portable to C99 or later: C11-specific features such as ‘`<stdalign.h>`’ and ‘`_Noreturn`’ are not used without a check, typically at configuration time, and the Emacs build procedure provides a substitute implementation if necessary. Some C11 features, such as anonymous structures and unions, are too difficult to emulate, so they are avoided entirely.

At some point in the future the base C dialect will no doubt change to C11.
