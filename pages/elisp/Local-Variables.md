

### 12.3 Local Variables

Global variables have values that last until explicitly superseded with new values. Sometimes it is useful to give a variable a *local value*—a value that takes effect only within a certain part of a Lisp program. When a variable has a local value, we say that it is *locally bound* to that value, and that it is a *local variable*.

For example, when a function is called, its argument variables receive local values, which are the actual arguments supplied to the function call; these local bindings take effect within the body of the function. To take another example, the `let` special form explicitly establishes local bindings for specific variables, which take effect only within the body of the `let` form.

We also speak of the *global binding*, which is where (conceptually) the global value is kept.

Establishing a local binding saves away the variable’s previous value (or lack of one). We say that the previous value is *shadowed*. Both global and local values may be shadowed. If a local binding is in effect, using `setq` on the local variable stores the specified value in the local binding. When that local binding is no longer in effect, the previously shadowed value (or lack of one) comes back.

A variable can have more than one local binding at a time (e.g., if there are nested `let` forms that bind the variable). The *current binding* is the local binding that is actually in effect. It determines the value returned by evaluating the variable symbol, and it is the binding acted on by `setq`.

For most purposes, you can think of the current binding as the innermost local binding, or the global binding if there is no local binding. To be more precise, a rule called the *scoping rule* determines where in a program a local binding takes effect. The default scoping rule in Emacs Lisp is called *dynamic scoping*, which simply states that the current binding at any given point in the execution of a program is the most recently-created binding for that variable that still exists. For details about dynamic scoping, and an alternative scoping rule called *lexical scoping*, See [Variable Scoping](Variable-Scoping.html).

The special forms `let` and `let*` exist to create local bindings:

### Special Form: **let** *(bindings…) forms…*

This special form sets up local bindings for a certain set of variables, as specified by `bindings`, and then evaluates all of the `forms` in textual order. Its return value is the value of the last form in `forms`. The local bindings set up by `let` will be in effect only within the body of `forms`.

Each of the `bindings` is either (i) a symbol, in which case that symbol is locally bound to `nil`; or (ii) a list of the form `(symbol value-form)`, in which case `symbol` is locally bound to the result of evaluating `value-form`. If `value-form` is omitted, `nil` is used.

All of the `value-form`s in `bindings` are evaluated in the order they appear and *before* binding any of the symbols to them. Here is an example of this: `z` is bound to the old value of `y`, which is 2, not the new value of `y`, which is 1.

```lisp
(setq y 2)
     ⇒ 2
```

```lisp
```

```lisp
(let ((y 1)
      (z y))
  (list y z))
     ⇒ (1 2)
```

On the other hand, the order of *bindings* is unspecified: in the following example, either 1 or 2 might be printed.

```lisp
(let ((x 1)
      (x 2))
  (print x))
```

Therefore, avoid binding a variable more than once in a single `let` form.

### Special Form: **let\*** *(bindings…) forms…*

This special form is like `let`, but it binds each variable right after computing its local value, before computing the local value for the next variable. Therefore, an expression in `bindings` can refer to the preceding symbols bound in this `let*` form. Compare the following example with the example above for `let`.

```lisp
(setq y 2)
     ⇒ 2
```

```lisp
```

```lisp
(let* ((y 1)
       (z y))    ; Use the just-established value of y.
  (list y z))
     ⇒ (1 1)
```

### Special Form: **letrec** *(bindings…) forms…*

This special form is like `let*`, but all the variables are bound before any of the local values are computed. The values are then assigned to the locally bound variables. This is only useful when lexical binding is in effect, and you want to create closures that refer to bindings that would otherwise not yet be in effect when using `let*`.

For instance, here’s a closure that removes itself from a hook after being run once:

```lisp
(letrec ((hookfun (lambda ()
                    (message "Run once")
                    (remove-hook 'post-command-hook hookfun))))
  (add-hook 'post-command-hook hookfun))
```

Here is a complete list of the other facilities that create local bindings:

Function calls (see [Functions](Functions.html)).

Macro calls (see [Macros](Macros.html)).

`condition-case` (see [Errors](Errors.html)).

Variables can also have buffer-local bindings (see [Buffer-Local Variables](Buffer_002dLocal-Variables.html)); a few variables have terminal-local bindings (see [Multiple Terminals](Multiple-Terminals.html)). These kinds of bindings work somewhat like ordinary local bindings, but they are localized depending on where you are in Emacs.

### User Option: **max-specpdl-size**

This variable defines the limit on the total number of local variable bindings and `unwind-protect` cleanups (see [Cleaning Up from Nonlocal Exits](Cleanups.html)) that are allowed before Emacs signals an error (with data `"Variable binding depth exceeds max-specpdl-size"`).

This limit, with the associated error when it is exceeded, is one way that Lisp avoids infinite recursion on an ill-defined function. `max-lisp-eval-depth` provides another limit on depth of nesting. See [Eval](Eval.html#Definition-of-max_002dlisp_002deval_002ddepth).

The default value is 1600. Entry to the Lisp debugger increases the value, if there is little room left, to make sure the debugger itself has room to execute.
