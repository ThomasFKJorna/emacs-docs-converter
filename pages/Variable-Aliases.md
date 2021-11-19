

Next: [Variables with Restricted Values](Variables-with-Restricted-Values.html), Previous: [Connection Local Variables](Connection-Local-Variables.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 12.15 Variable Aliases

It is sometimes useful to make two variables synonyms, so that both variables always have the same value, and changing either one also changes the other. Whenever you change the name of a variable—either because you realize its old name was not well chosen, or because its meaning has partly changed—it can be useful to keep the old name as an *alias* of the new one for compatibility. You can do this with `defvaralias`.

*   Function: **defvaralias** *new-alias base-variable \&optional docstring*

    This function defines the symbol `new-alias` as a variable alias for symbol `base-variable`. This means that retrieving the value of `new-alias` returns the value of `base-variable`, and changing the value of `new-alias` changes the value of `base-variable`. The two aliased variable names always share the same value and the same bindings.

    If the `docstring` argument is non-`nil`, it specifies the documentation for `new-alias`; otherwise, the alias gets the same documentation as `base-variable` has, if any, unless `base-variable` is itself an alias, in which case `new-alias` gets the documentation of the variable at the end of the chain of aliases.

    This function returns `base-variable`.

Variable aliases are convenient for replacing an old name for a variable with a new name. `make-obsolete-variable` declares that the old name is obsolete and therefore that it may be removed at some stage in the future.

*   Function: **make-obsolete-variable** *obsolete-name current-name when \&optional access-type*

    This function makes the byte compiler warn that the variable `obsolete-name` is obsolete. If `current-name` is a symbol, it is the variable’s new name; then the warning message says to use `current-name` instead of `obsolete-name`. If `current-name` is a string, this is the message and there is no replacement variable. `when` should be a string indicating when the variable was first made obsolete (usually a version number string).

    The optional argument `access-type`, if non-`nil`, should specify the kind of access that will trigger obsolescence warnings; it can be either `get` or `set`.

You can make two variables synonyms and declare one obsolete at the same time using the macro `define-obsolete-variable-alias`.

*   Macro: **define-obsolete-variable-alias** *obsolete-name current-name \&optional when docstring*

    This macro marks the variable `obsolete-name` as obsolete and also makes it an alias for the variable `current-name`. It is equivalent to the following:

    ```lisp
    (defvaralias obsolete-name current-name docstring)
    (make-obsolete-variable obsolete-name current-name when)
    ```

<!---->

*   Function: **indirect-variable** *variable*

    This function returns the variable at the end of the chain of aliases of `variable`. If `variable` is not a symbol, or if `variable` is not defined as an alias, the function returns `variable`.

    This function signals a `cyclic-variable-indirection` error if there is a loop in the chain of symbols.

```lisp
(defvaralias 'foo 'bar)
(indirect-variable 'foo)
     ⇒ bar
(indirect-variable 'bar)
     ⇒ bar
(setq bar 2)
bar
     ⇒ 2
```

```lisp
foo
     ⇒ 2
```

```lisp
(setq foo 0)
bar
     ⇒ 0
foo
     ⇒ 0
```

Next: [Variables with Restricted Values](Variables-with-Restricted-Values.html), Previous: [Connection Local Variables](Connection-Local-Variables.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
