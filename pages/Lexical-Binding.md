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

Next: [Using Lexical Binding](Using-Lexical-Binding.html), Previous: [Dynamic Binding Tips](Dynamic-Binding-Tips.html), Up: [Variable Scoping](Variable-Scoping.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 12.10.3 Lexical Binding

Lexical binding was introduced to Emacs, as an optional feature, in version 24.1. We expect its importance to increase with time. Lexical binding opens up many more opportunities for optimization, so programs using it are likely to run faster in future Emacs versions. Lexical binding is also more compatible with concurrency, which was added to Emacs in version 26.1.

A lexically-bound variable has *lexical scope*, meaning that any reference to the variable must be located textually within the binding construct. Here is an example (see [Using Lexical Binding](Using-Lexical-Binding.html), for how to actually enable lexical binding):

    (let ((x 1))    ; x is lexically bound.
      (+ x 3))
         ⇒ 4

    (defun getx ()
      x)            ; x is used free in this function.

    (let ((x 1))    ; x is lexically bound.
      (getx))
    error→ Symbol's value as variable is void: x

Here, the variable `x` has no global value. When it is lexically bound within a `let` form, it can be used in the textual confines of that `let` form. But it can *not* be used from within a `getx` function called from the `let` form, since the function definition of `getx` occurs outside the `let` form itself.

Here is how lexical binding works. Each binding construct defines a *lexical environment*, specifying the variables that are bound within the construct and their local values. When the Lisp evaluator wants the current value of a variable, it looks first in the lexical environment; if the variable is not specified in there, it looks in the symbol’s value cell, where the dynamic value is stored.

(Internally, the lexical environment is an alist of symbol-value pairs, with the final element in the alist being the symbol `t` rather than a cons cell. Such an alist can be passed as the second argument to the `eval` function, in order to specify a lexical environment in which to evaluate a form. See [Eval](Eval.html). Most Emacs Lisp programs, however, should not interact directly with lexical environments in this way; only specialized programs like debuggers.)

Lexical bindings have indefinite extent. Even after a binding construct has finished executing, its lexical environment can be “kept around” in Lisp objects called *closures*. A closure is created when you define a named or anonymous function with lexical binding enabled. See [Closures](Closures.html), for details.

When a closure is called as a function, any lexical variable references within its definition use the retained lexical environment. Here is an example:

    (defvar my-ticker nil)   ; We will use this dynamically bound
                             ; variable to store a closure.

    (let ((x 0))             ; x is lexically bound.
      (setq my-ticker (lambda ()
                        (setq x (1+ x)))))
        ⇒ (closure ((x . 0) t) ()
              (setq x (1+ x)))

    (funcall my-ticker)
        ⇒ 1

    (funcall my-ticker)
        ⇒ 2

    (funcall my-ticker)
        ⇒ 3

    x                        ; Note that x has no global value.
    error→ Symbol's value as variable is void: x

The `let` binding defines a lexical environment in which the variable `x` is locally bound to 0. Within this binding construct, we define a lambda expression which increments `x` by one and returns the incremented value. This lambda expression is automatically turned into a closure, in which the lexical environment lives on even after the `let` binding construct has exited. Each time we evaluate the closure, it increments `x`, using the binding of `x` in that lexical environment.

Note that unlike dynamic variables which are tied to the symbol object itself, the relationship between lexical variables and symbols is only present in the interpreter (or compiler). Therefore, functions which take a symbol argument (like `symbol-value`, `boundp`, and `set`) can only retrieve or modify a variable’s dynamic binding (i.e., the contents of its symbol’s value cell).

Next: [Using Lexical Binding](Using-Lexical-Binding.html), Previous: [Dynamic Binding Tips](Dynamic-Binding-Tips.html), Up: [Variable Scoping](Variable-Scoping.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
