

### 13.15 Telling the Compiler that a Function is Defined

Byte-compiling a file often produces warnings about functions that the compiler doesn’t know about (see [Compiler Errors](Compiler-Errors.html)). Sometimes this indicates a real problem, but usually the functions in question are defined in other files which would be loaded if that code is run. For example, byte-compiling `simple.el` used to warn:

```lisp
simple.el:8727:1:Warning: the function ‘shell-mode’ is not known to be
    defined.
```

In fact, `shell-mode` is used only in a function that executes `(require 'shell)` before calling `shell-mode`, so `shell-mode` will be defined properly at run-time. When you know that such a warning does not indicate a real problem, it is good to suppress the warning. That makes new warnings which might mean real problems more visible. You do that with `declare-function`.

All you need to do is add a `declare-function` statement before the first use of the function in question:

```lisp
(declare-function shell-mode "shell" ())
```

This says that `shell-mode` is defined in `shell.el` (the ‘`.el`’ can be omitted). The compiler takes for granted that that file really defines the function, and does not check.

The optional third argument specifies the argument list of `shell-mode`. In this case, it takes no arguments (`nil` is different from not specifying a value). In other cases, this might be something like `(file &optional overwrite)`. You don’t have to specify the argument list, but if you do the byte compiler can check that the calls match the declaration.

### Macro: **declare-function** *function file \&optional arglist fileonly*

Tell the byte compiler to assume that `function` is defined in the file `file`. The optional third argument `arglist` is either `t`, meaning the argument list is unspecified, or a list of formal parameters in the same style as `defun`. An omitted `arglist` defaults to `t`, not `nil`; this is atypical behavior for omitted arguments, and it means that to supply a fourth but not third argument one must specify `t` for the third-argument placeholder instead of the usual `nil`. The optional fourth argument `fileonly` non-`nil` means check only that `file` exists, not that it actually defines `function`.

To verify that these functions really are declared where `declare-function` says they are, use `check-declare-file` to check all `declare-function` calls in one source file, or use `check-declare-directory` check all the files in and under a certain directory.

These commands find the file that ought to contain a function’s definition using `locate-library`; if that finds no file, they expand the definition file name relative to the directory of the file that contains the `declare-function` call.

You can also say that a function is a primitive by specifying a file name ending in ‘`.c`’ or ‘`.m`’. This is useful only when you call a primitive that is defined only on certain systems. Most primitives are always defined, so they will never give you a warning.

Sometimes a file will optionally use functions from an external package. If you prefix the filename in the `declare-function` statement with ‘`ext:`’, then it will be checked if it is found, otherwise skipped without error.

There are some function definitions that ‘`check-declare`’ does not understand (e.g., `defstruct` and some other macros). In such cases, you can pass a non-`nil` `fileonly` argument to `declare-function`, meaning to only check that the file exists, not that it actually defines the function. Note that to do this without having to specify an argument list, you should set the `arglist` argument to `t` (because `nil` means an empty argument list, as opposed to an unspecified one).
