

Next: [Hooks for Loading](Hooks-for-Loading.html), Previous: [Where Defined](Where-Defined.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 16.9 Unloading

You can discard the functions and variables loaded by a library to reclaim memory for other Lisp objects. To do this, use the function `unload-feature`:

*   Command: **unload-feature** *feature \&optional force*

    This command unloads the library that provided feature `feature`. It undefines all functions, macros, and variables defined in that library with `defun`, `defalias`, `defsubst`, `defmacro`, `defconst`, `defvar`, and `defcustom`. It then restores any autoloads formerly associated with those symbols. (Loading saves these in the `autoload` property of the symbol.)

    Before restoring the previous definitions, `unload-feature` runs `remove-hook` to remove functions in the library from certain hooks. These hooks include variables whose names end in ‘`-hook`’ (or the deprecated suffix ‘`-hooks`’), plus those listed in `unload-feature-special-hooks`, as well as `auto-mode-alist`. This is to prevent Emacs from ceasing to function because important hooks refer to functions that are no longer defined.

    Standard unloading activities also undoes ELP profiling of functions in that library, unprovides any features provided by the library, and cancels timers held in variables defined by the library.

    If these measures are not sufficient to prevent malfunction, a library can define an explicit unloader named `feature-unload-function`. If that symbol is defined as a function, `unload-feature` calls it with no arguments before doing anything else. It can do whatever is appropriate to unload the library. If it returns `nil`, `unload-feature` proceeds to take the normal unload actions. Otherwise it considers the job to be done.

    Ordinarily, `unload-feature` refuses to unload a library on which other loaded libraries depend. (A library `a` depends on library `b` if `a` contains a `require` for `b`.) If the optional argument `force` is non-`nil`, dependencies are ignored and you can unload any library.

The `unload-feature` function is written in Lisp; its actions are based on the variable `load-history`.

*   Variable: **unload-feature-special-hooks**

    This variable holds a list of hooks to be scanned before unloading a library, to remove functions defined in the library.

Next: [Hooks for Loading](Hooks-for-Loading.html), Previous: [Where Defined](Where-Defined.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
