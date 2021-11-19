

Next: [Accessing Documentation](Accessing-Documentation.html), Up: [Documentation](Documentation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 24.1 Documentation Basics

A documentation string is written using the Lisp syntax for strings, with double-quote characters surrounding the text. It is, in fact, an actual Lisp string. When the string appears in the proper place in a function or variable definition, it serves as the function’s or variable’s documentation.

In a function definition (a `lambda` or `defun` form), the documentation string is specified after the argument list, and is normally stored directly in the function object. See [Function Documentation](Function-Documentation.html). You can also put function documentation in the `function-documentation` property of a function name (see [Accessing Documentation](Accessing-Documentation.html)).

In a variable definition (a `defvar` form), the documentation string is specified after the initial value. See [Defining Variables](Defining-Variables.html). The string is stored in the variable’s `variable-documentation` property.

Sometimes, Emacs does not keep documentation strings in memory. There are two such circumstances. Firstly, to save memory, the documentation for preloaded functions and variables (including primitives) is kept in a file named `DOC`, in the directory specified by `doc-directory` (see [Accessing Documentation](Accessing-Documentation.html)). Secondly, when a function or variable is loaded from a byte-compiled file, Emacs avoids loading its documentation string (see [Docs and Compilation](Docs-and-Compilation.html)). In both cases, Emacs looks up the documentation string from the file only when needed, such as when the user calls `C-h f` (`describe-function`) for a function.

Documentation strings can contain special *key substitution sequences*, referring to key bindings which are looked up only when the user views the documentation. This allows the help commands to display the correct keys even if a user rearranges the default key bindings. See [Keys in Documentation](Keys-in-Documentation.html).

In the documentation string of an autoloaded command (see [Autoload](Autoload.html)), these key-substitution sequences have an additional special effect: they cause `C-h f` on the command to trigger autoloading. (This is needed for correctly setting up the hyperlinks in the `*Help*` buffer.)

Next: [Accessing Documentation](Accessing-Documentation.html), Up: [Documentation](Documentation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
