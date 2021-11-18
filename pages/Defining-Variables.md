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

Next: [Tips for Defining](Tips-for-Defining.html), Previous: [Void Variables](Void-Variables.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 12.5 Defining Global Variables

A *variable definition* is a construct that announces your intention to use a symbol as a global variable. It uses the special forms `defvar` or `defconst`, which are documented below.

A variable definition serves three purposes. First, it informs people who read the code that the symbol is *intended* to be used a certain way (as a variable). Second, it informs the Lisp system of this, optionally supplying an initial value and a documentation string. Third, it provides information to programming tools such as `etags`, allowing them to find where the variable was defined.

The difference between `defconst` and `defvar` is mainly a matter of intent, serving to inform human readers of whether the value should ever change. Emacs Lisp does not actually prevent you from changing the value of a variable defined with `defconst`. One notable difference between the two forms is that `defconst` unconditionally initializes the variable, whereas `defvar` initializes it only if it is originally void.

To define a customizable variable, you should use `defcustom` (which calls `defvar` as a subroutine). See [Variable Definitions](Variable-Definitions.html).

*   Special Form: **defvar** *symbol \[value \[doc-string]]*

    This special form defines `symbol` as a variable. Note that `symbol` is not evaluated; the symbol to be defined should appear explicitly in the `defvar` form. The variable is marked as *special*, meaning that it should always be dynamically bound (see [Variable Scoping](Variable-Scoping.html)).

    If `value` is specified, and `symbol` is void (i.e., it has no dynamically bound value; see [Void Variables](Void-Variables.html)), then `value` is evaluated and `symbol` is set to the result. But if `symbol` is not void, `value` is not evaluated, and `symbol`’s value is left unchanged. If `value` is omitted, the value of `symbol` is not changed in any case.

    Note that specifying a value, even `nil`, marks the variable as special permanently. Whereas if `value` is omitted then the variable is only marked special locally (i.e. within the current lexical scope, or file if at the top-level). This can be useful for suppressing byte compilation warnings, see [Compiler Errors](Compiler-Errors.html).

    If `symbol` has a buffer-local binding in the current buffer, `defvar` acts on the default value, which is buffer-independent, rather than the buffer-local binding. It sets the default value if the default value is void. See [Buffer-Local Variables](Buffer_002dLocal-Variables.html).

    If `symbol` is already lexically bound (e.g., if the `defvar` form occurs in a `let` form with lexical binding enabled), then `defvar` sets the dynamic value. The lexical binding remains in effect until its binding construct exits. See [Variable Scoping](Variable-Scoping.html).

    When you evaluate a top-level `defvar` form with `C-M-x` in Emacs Lisp mode (`eval-defun`), a special feature of `eval-defun` arranges to set the variable unconditionally, without testing whether its value is void.

    If the `doc-string` argument is supplied, it specifies the documentation string for the variable (stored in the symbol’s `variable-documentation` property). See [Documentation](Documentation.html).

    Here are some examples. This form defines `foo` but does not initialize it:

        (defvar foo)
             ⇒ foo

    This example initializes the value of `bar` to `23`, and gives it a documentation string:

        (defvar bar 23
          "The normal weight of a bar.")
             ⇒ bar

    The `defvar` form returns `symbol`, but it is normally used at top level in a file where its value does not matter.

    For a more elaborate example of using `defvar` without a value, see [Local defvar example](Using-Lexical-Binding.html#Local-defvar-example).

<!---->

*   Special Form: **defconst** *symbol value \[doc-string]*

    This special form defines `symbol` as a value and initializes it. It informs a person reading your code that `symbol` has a standard global value, established here, that should not be changed by the user or by other programs. Note that `symbol` is not evaluated; the symbol to be defined must appear explicitly in the `defconst`.

    The `defconst` form, like `defvar`, marks the variable as *special*, meaning that it should always be dynamically bound (see [Variable Scoping](Variable-Scoping.html)). In addition, it marks the variable as risky (see [File Local Variables](File-Local-Variables.html)).

    `defconst` always evaluates `value`, and sets the value of `symbol` to the result. If `symbol` does have a buffer-local binding in the current buffer, `defconst` sets the default value, not the buffer-local value. (But you should not be making buffer-local bindings for a symbol that is defined with `defconst`.)

    An example of the use of `defconst` is Emacs’s definition of `float-pi`—the mathematical constant *pi*, which ought not to be changed by anyone (attempts by the Indiana State Legislature notwithstanding). As the second form illustrates, however, `defconst` is only advisory.

        (defconst float-pi 3.141592653589793 "The value of Pi.")
             ⇒ float-pi

    <!---->

        (setq float-pi 3)
             ⇒ float-pi

    <!---->

        float-pi
             ⇒ 3

**Warning:** If you use a `defconst` or `defvar` special form while the variable has a local binding (made with `let`, or a function argument), it sets the local binding rather than the global binding. This is not what you usually want. To prevent this, use these special forms at top level in a file, where normally no local binding is in effect, and make sure to load the file before making a local binding for the variable.

Next: [Tips for Defining](Tips-for-Defining.html), Previous: [Void Variables](Void-Variables.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
