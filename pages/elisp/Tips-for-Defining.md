<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Accessing Variables](Accessing-Variables.html), Previous: [Defining Variables](Defining-Variables.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 12.6 Tips for Defining Variables Robustly

When you define a variable whose value is a function, or a list of functions, use a name that ends in ‘`-function`’ or ‘`-functions`’, respectively.

There are several other variable name conventions; here is a complete list:

*   ‘`…-hook`’

    The variable is a normal hook (see [Hooks](Hooks.html)).

*   ‘`…-function`’

    The value is a function.

*   ‘`…-functions`’

    The value is a list of functions.

*   ‘`…-form`’

    The value is a form (an expression).

*   ‘`…-forms`’

    The value is a list of forms (expressions).

*   ‘`…-predicate`’

    The value is a predicate—a function of one argument that returns non-`nil` for success and `nil` for failure.

*   ‘`…-flag`’

    The value is significant only as to whether it is `nil` or not. Since such variables often end up acquiring more values over time, this convention is not strongly recommended.

*   ‘`…-program`’

    The value is a program name.

*   ‘`…-command`’

    The value is a whole shell command.

*   ‘`…-switches`’

    The value specifies options for a command.

*   ‘`prefix--…`’

    The variable is intended for internal use and is defined in the file `prefix.el`. (Emacs code contributed before 2018 may follow other conventions, which are being phased out.)

*   ‘`…-internal`’

    The variable is intended for internal use and is defined in C code. (Emacs code contributed before 2018 may follow other conventions, which are being phased out.)

When you define a variable, always consider whether you should mark it as safe or risky; see [File Local Variables](File-Local-Variables.html).

When defining and initializing a variable that holds a complicated value (such as a keymap with bindings in it), it’s best to put the entire computation of the value into the `defvar`, like this:

    (defvar my-mode-map
      (let ((map (make-sparse-keymap)))
        (define-key map "\C-c\C-a" 'my-command)
        …
        map)
      docstring)

This method has several benefits. First, if the user quits while loading the file, the variable is either still uninitialized or initialized properly, never in-between. If it is still uninitialized, reloading the file will initialize it properly. Second, reloading the file once the variable is initialized will not alter it; that is important if the user has run hooks to alter part of the contents (such as, to rebind keys). Third, evaluating the `defvar` form with `C-M-x` will reinitialize the map completely.

Putting so much code in the `defvar` form has one disadvantage: it puts the documentation string far away from the line which names the variable. Here’s a safe way to avoid that:

    (defvar my-mode-map nil
      docstring)
    (unless my-mode-map
      (let ((map (make-sparse-keymap)))
        (define-key map "\C-c\C-a" 'my-command)
        …
        (setq my-mode-map map)))

This has all the same advantages as putting the initialization inside the `defvar`, except that you must type `C-M-x` twice, once on each form, if you do want to reinitialize the variable.

Next: [Accessing Variables](Accessing-Variables.html), Previous: [Defining Variables](Defining-Variables.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
