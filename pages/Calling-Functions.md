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

Next: [Mapping Functions](Mapping-Functions.html), Previous: [Defining Functions](Defining-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 13.5 Calling Functions

Defining functions is only half the battle. Functions don’t do anything until you *call* them, i.e., tell them to run. Calling a function is also known as *invocation*.

The most common way of invoking a function is by evaluating a list. For example, evaluating the list `(concat "a" "b")` calls the function `concat` with arguments `"a"` and `"b"`. See [Evaluation](Evaluation.html), for a description of evaluation.

When you write a list as an expression in your program, you specify which function to call, and how many arguments to give it, in the text of the program. Usually that’s just what you want. Occasionally you need to compute at run time which function to call. To do that, use the function `funcall`. When you also need to determine at run time how many arguments to pass, use `apply`.

*   Function: **funcall** *function \&rest arguments*

    `funcall` calls `function` with `arguments`, and returns whatever `function` returns.

    Since `funcall` is a function, all of its arguments, including `function`, are evaluated before `funcall` is called. This means that you can use any expression to obtain the function to be called. It also means that `funcall` does not see the expressions you write for the `arguments`, only their values. These values are *not* evaluated a second time in the act of calling `function`; the operation of `funcall` is like the normal procedure for calling a function, once its arguments have already been evaluated.

    The argument `function` must be either a Lisp function or a primitive function. Special forms and macros are not allowed, because they make sense only when given the unevaluated argument expressions. `funcall` cannot provide these because, as we saw above, it never knows them in the first place.

    If you need to use `funcall` to call a command and make it behave as if invoked interactively, use `funcall-interactively` (see [Interactive Call](Interactive-Call.html)).

        (setq f 'list)
             ⇒ list

    <!---->

        (funcall f 'x 'y 'z)
             ⇒ (x y z)

    <!---->

        (funcall f 'x 'y '(z))
             ⇒ (x y (z))

    <!---->

        (funcall 'and t nil)
        error→ Invalid function: #<subr and>

    Compare these examples with the examples of `apply`.

<!---->

*   Function: **apply** *function \&rest arguments*

    `apply` calls `function` with `arguments`, just like `funcall` but with one difference: the last of `arguments` is a list of objects, which are passed to `function` as separate arguments, rather than a single list. We say that `apply` *spreads* this list so that each individual element becomes an argument.

    `apply` returns the result of calling `function`. As with `funcall`, `function` must either be a Lisp function or a primitive function; special forms and macros do not make sense in `apply`.

        (setq f 'list)
             ⇒ list

    <!---->

        (apply f 'x 'y 'z)
        error→ Wrong type argument: listp, z

    <!---->

        (apply '+ 1 2 '(3 4))
             ⇒ 10

    <!---->

        (apply '+ '(1 2 3 4))
             ⇒ 10

    ```
    ```

        (apply 'append '((a b c) nil (x y z) nil))
             ⇒ (a b c x y z)

    For an interesting example of using `apply`, see [Definition of mapcar](Mapping-Functions.html#Definition-of-mapcar).

Sometimes it is useful to fix some of the function’s arguments at certain values, and leave the rest of arguments for when the function is actually called. The act of fixing some of the function’s arguments is called *partial application* of the function[11](#FOOT11). The result is a new function that accepts the rest of arguments and calls the original function with all the arguments combined.

Here’s how to do partial application in Emacs Lisp:

*   Function: **apply-partially** *func \&rest args*

    This function returns a new function which, when called, will call `func` with the list of arguments composed from `args` and additional arguments specified at the time of the call. If `func` accepts `n` arguments, then a call to `apply-partially` with `m < n`<!-- /@w --> arguments will produce a new function of `n - m`<!-- /@w --> arguments.

    Here’s how we could define the built-in function `1+`, if it didn’t exist, using `apply-partially` and `+`, another built-in function:

        (defalias '1+ (apply-partially '+ 1)
          "Increment argument by one.")

    <!---->

        (1+ 10)
             ⇒ 11

It is common for Lisp functions to accept functions as arguments or find them in data structures (especially in hook variables and property lists) and call them using `funcall` or `apply`. Functions that accept function arguments are often called *functionals*.

Sometimes, when you call a functional, it is useful to supply a no-op function as the argument. Here are two different kinds of no-op function:

*   Function: **identity** *argument*

    This function returns `argument` and has no side effects.

<!---->

*   Function: **ignore** *\&rest arguments*

    This function ignores any `arguments` and returns `nil`.

Some functions are user-visible *commands*, which can be called interactively (usually by a key sequence). It is possible to invoke such a command exactly as though it was called interactively, by using the `call-interactively` function. See [Interactive Call](Interactive-Call.html).

***

#### Footnotes

##### [(11)](#DOCF11)

This is related to, but different from *currying*, which transforms a function that takes multiple arguments in such a way that it can be called as a chain of functions, each one with a single argument.

Next: [Mapping Functions](Mapping-Functions.html), Previous: [Defining Functions](Defining-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
