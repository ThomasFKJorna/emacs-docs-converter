

Previous: [Rx Functions](Rx-Functions.html), Up: [Rx Notation](Rx-Notation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 34.3.3.3 Defining new `rx` forms

The `rx` notation can be extended by defining new symbols and parameterized forms in terms of other `rx` expressions. This is handy for sharing parts between several regexps, and for making complex ones easier to build and understand by putting them together from smaller pieces.

For example, you could define `name` to mean `(one-or-more letter)`, and `(quoted x)` to mean `(seq ?' x ?')` for any `x`. These forms could then be used in `rx` expressions like any other: `(rx (quoted name))` would match a nonempty sequence of letters inside single quotes.

The Lisp macros below provide different ways of binding names to definitions. Common to all of them are the following rules:

*   Built-in `rx` forms, like `digit` and `group`, cannot be redefined.
*   The definitions live in a name space of their own, separate from that of Lisp variables. There is thus no need to attach a suffix like `-regexp` to names; they cannot collide with anything else.
*   Definitions cannot refer to themselves recursively, directly or indirectly. If you find yourself needing this, you want a parser, not a regular expression.
*   Definitions are only ever expanded in calls to `rx` or `rx-to-string`, not merely by their presence in definition macros. This means that the order of definitions doesn’t matter, even when they refer to each other, and that syntax errors only show up when they are used, not when they are defined.
*   User-defined forms are allowed wherever arbitrary `rx` expressions are expected; for example, in the body of a `zero-or-one` form, but not inside `any` or `category` forms. They are also allowed inside `not` and `intersection` forms.

<!---->

*   Macro: **rx-define** *name \[arglist] rx-form*

    Define `name` globally in all subsequent calls to `rx` and `rx-to-string`. If `arglist` is absent, then `name` is defined as a plain symbol to be replaced with `rx-form`. Example:

    ```lisp
    (rx-define haskell-comment (seq "--" (zero-or-more nonl)))
    (rx haskell-comment)
         ⇒ "--.*"
    ```

    If `arglist` is present, it must be a list of zero or more argument names, and `name` is then defined as a parameterized form. When used in an `rx` expression as `(name arg…)`, each `arg` will replace the corresponding argument name inside `rx-form`.

    `arglist` may end in `&rest` and one final argument name, denoting a rest parameter. The rest parameter will expand to all extra actual argument values not matched by any other parameter in `arglist`, spliced into `rx-form` where it occurs. Example:

    ```lisp
    (rx-define moan (x y &rest r) (seq x (one-or-more y) r "!"))
    (rx (moan "MOO" "A" "MEE" "OW"))
         ⇒ "MOOA+MEEOW!"
    ```

    Since the definition is global, it is recommended to give `name` a package prefix to avoid name clashes with definitions elsewhere, as is usual when naming non-local variables and functions.

<!---->

*   Macro: **rx-let** *(bindings…) body…*

    Make the `rx` definitions in `bindings` available locally for `rx` macro invocations in `body`, which is then evaluated.

    Each element of `bindings` is on the form `(name [arglist] rx-form)`, where the parts have the same meaning as in `rx-define` above. Example:

    ```lisp
    (rx-let ((comma-separated (item) (seq item (0+ "," item)))
             (number (1+ digit))
             (numbers (comma-separated number)))
      (re-search-forward (rx "(" numbers ")")))
    ```

    The definitions are only available during the macro-expansion of `body`, and are thus not present during execution of compiled code.

    `rx-let` can be used not only inside a function, but also at top level to include global variable and function definitions that need to share a common set of `rx` forms. Since the names are local inside `body`, there is no need for any package prefixes. Example:

    ```lisp
    (rx-let ((phone-number (seq (opt ?+) (1+ (any digit ?-)))))
      (defun find-next-phone-number ()
        (re-search-forward (rx phone-number)))
      (defun phone-number-p (string)
        (string-match-p (rx bos phone-number eos) string)))
    ```

    The scope of the `rx-let` bindings is lexical, which means that they are not visible outside `body` itself, even in functions called from `body`.

<!---->

*   Macro: **rx-let-eval** *bindings body…*

    Evaluate `bindings` to a list of bindings as in `rx-let`, and evaluate `body` with those bindings in effect for calls to `rx-to-string`.

    This macro is similar to `rx-let`, except that the `bindings` argument is evaluated (and thus needs to be quoted if it is a list literal), and the definitions are substituted at run time, which is required for `rx-to-string` to work. Example:

    ```lisp
    (rx-let-eval
        '((ponder (x) (seq "Where have all the " x " gone?")))
      (looking-at (rx-to-string
                   '(ponder (or "flowers" "young girls"
                                "left socks")))))
    ```

    Another difference from `rx-let` is that the `bindings` are dynamically scoped, and thus also available in functions called from `body`. However, they are not visible inside functions defined in `body`.

Previous: [Rx Functions](Rx-Functions.html), Up: [Rx Notation](Rx-Notation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
