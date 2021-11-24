

### 17.6 Compiler Errors

Error and warning messages from byte compilation are printed in a buffer named `*Compile-Log*`. These messages include file names and line numbers identifying the location of the problem. The usual Emacs commands for operating on compiler output can be used on these messages.

When an error is due to invalid syntax in the program, the byte compiler might get confused about the error’s exact location. One way to investigate is to switch to the buffer ` *Compiler Input*`. (This buffer name starts with a space, so it does not show up in the Buffer Menu.) This buffer contains the program being compiled, and point shows how far the byte compiler was able to read; the cause of the error might be nearby. See [Syntax Errors](Syntax-Errors.html), for some tips for locating syntax errors.

A common type of warning issued by the byte compiler is for functions and variables that were used but not defined. Such warnings report the line number for the end of the file, not the locations where the missing functions or variables were used; to find these, you must search the file manually.

If you are sure that a warning message about a missing function or variable is unjustified, there are several ways to suppress it:

You can suppress the warning for a specific call to a function `func` by conditionalizing it on an `fboundp` test, like this:

```lisp
(if (fboundp 'func) ...(func ...)...)
```

The call to `func` must be in the `then-form` of the `if`, and `func` must appear quoted in the call to `fboundp`. (This feature operates for `cond` as well.)

Likewise, you can suppress the warning for a specific use of a variable `variable` by conditionalizing it on a `boundp` test:

```lisp
(if (boundp 'variable) ...variable...)
```

The reference to `variable` must be in the `then-form` of the `if`, and `variable` must appear quoted in the call to `boundp`.

You can tell the compiler that a function is defined using `declare-function`. See [Declaring Functions](Declaring-Functions.html).

Likewise, you can tell the compiler that a variable is defined using `defvar` with no initial value. (Note that this marks the variable as special, i.e. dynamically bound, but only within the current lexical scope, or file if at top-level.) See [Defining Variables](Defining-Variables.html).

You can also suppress compiler warnings within a certain expression using the `with-suppressed-warnings` macro:

### Special Form: **with-suppressed-warnings** *warnings body…*

In execution, this is equivalent to `(progn body...)`, but the compiler does not issue warnings for the specified conditions in `body`. `warnings` is an associative list of warning symbols and function/variable symbols they apply to. For instance, if you wish to call an obsolete function called `foo`, but want to suppress the compilation warning, say:

```lisp
(with-suppressed-warnings ((obsolete foo))
  (foo ...))
```

For more coarse-grained suppression of compiler warnings, you can use the `with-no-warnings` construct:

### Special Form: **with-no-warnings** *body…*

In execution, this is equivalent to `(progn body...)`, but the compiler does not issue warnings for anything that occurs inside `body`.

We recommend that you use `with-suppressed-warnings` instead, but if you do use this construct, that you use it around the smallest possible piece of code to avoid missing possible warnings other than one you intend to suppress.

Byte compiler warnings can be controlled more precisely by setting the variable `byte-compile-warnings`. See its documentation string for details.

Sometimes you may wish the byte-compiler warnings to be reported using `error`. If so, set `byte-compile-error-on-warn` to a non-`nil` value.
