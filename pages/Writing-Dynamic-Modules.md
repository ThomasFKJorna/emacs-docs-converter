

Next: [Object Internals](Object-Internals.html), Previous: [Writing Emacs Primitives](Writing-Emacs-Primitives.html), Up: [GNU Emacs Internals](GNU-Emacs-Internals.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### E.8 Writing Dynamically-Loaded Modules

This section describes the Emacs module API and how to use it as part of writing extension modules for Emacs. The module API is defined in the C programming language, therefore the description and the examples in this section assume the module is written in C. For other programming languages, you will need to use the appropriate bindings, interfaces and facilities for calling C code. Emacs C code requires a C99 or later compiler (see [C Dialect](C-Dialect.html)), and so the code examples in this section also follow that standard.

Writing a module and integrating it into Emacs comprises the following tasks:

*   Writing initialization code for the module.
*   Writing one or more module functions.
*   Communicating values and objects between Emacs and your module functions.
*   Handling of error conditions and nonlocal exits.

The following subsections describe these tasks and the API itself in more detail.

Once your module is written, compile it to produce a shared library, according to the conventions of the underlying platform. Then place the shared library in a directory mentioned in `load-path` (see [Library Search](Library-Search.html)), where Emacs will find it.

If you wish to verify the conformance of a module to the Emacs dynamic module API, invoke Emacs with the `--module-assertions` option. See [Initial Options](https://www.gnu.org/software/emacs/manual/html_node/emacs/Initial-Options.html#Initial-Options) in The GNU Emacs Manual.

|                                                       |    |    |
| :---------------------------------------------------- | -- | :- |
| • [Module Initialization](Module-Initialization.html) |    |    |
| • [Module Functions](Module-Functions.html)           |    |    |
| • [Module Values](Module-Values.html)                 |    |    |
| • [Module Misc](Module-Misc.html)                     |    |    |
| • [Module Nonlocal](Module-Nonlocal.html)             |    |    |

Next: [Object Internals](Object-Internals.html), Previous: [Writing Emacs Primitives](Writing-Emacs-Primitives.html), Up: [GNU Emacs Internals](GNU-Emacs-Internals.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
