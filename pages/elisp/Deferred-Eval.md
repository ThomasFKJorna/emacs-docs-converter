

### 10.6 Deferred and Lazy Evaluation

Sometimes it is useful to delay the evaluation of an expression, for example if you want to avoid performing a time-consuming calculation if it turns out that the result is not needed in the future of the program. The `thunk` library provides the following functions and macros to support such *deferred evaluation*:

### Macro: **thunk-delay** *forms…*

Return a *thunk* for evaluating the `forms`. A thunk is a closure (see [Closures](Closures.html)) that inherits the lexical environment of the `thunk-delay` call. Using this macro requires `lexical-binding`.

### Function: **thunk-force** *thunk*

Force `thunk` to perform the evaluation of the forms specified in the `thunk-delay` that created the thunk. The result of the evaluation of the last form is returned. The `thunk` also “remembers” that it has been forced: Any further calls of `thunk-force` with the same `thunk` will just return the same result without evaluating the forms again.

### Macro: **thunk-let** *(bindings…) forms…*

This macro is analogous to `let` but creates “lazy” variable bindings. Any binding has the form `(symbol value-form)`. Unlike `let`, the evaluation of any `value-form` is deferred until the binding of the according `symbol` is used for the first time when evaluating the `forms`. Any `value-form` is evaluated at most once. Using this macro requires `lexical-binding`.

Example:

```lisp
(defun f (number)
  (thunk-let ((derived-number
              (progn (message "Calculating 1 plus 2 times %d" number)
                     (1+ (* 2 number)))))
    (if (> number 10)
        derived-number
      number)))
```

```lisp
```

```lisp
(f 5)
⇒ 5
```

```lisp
```

```lisp
(f 12)
-| Calculating 1 plus 2 times 12
⇒ 25
```

```lisp
```

Because of the special nature of lazily bound variables, it is an error to set them (e.g. with `setq`).

### Macro: **thunk-let\*** *(bindings…) forms…*

This is like `thunk-let` but any expression in `bindings` is allowed to refer to preceding bindings in this `thunk-let*` form. Using this macro requires `lexical-binding`.

```lisp
(thunk-let* ((x (prog2 (message "Calculating x...")
                    (+ 1 1)
                  (message "Finished calculating x")))
             (y (prog2 (message "Calculating y...")
                    (+ x 1)
                  (message "Finished calculating y")))
             (z (prog2 (message "Calculating z...")
                    (+ y 1)
                  (message "Finished calculating z")))
             (a (prog2 (message "Calculating a...")
                    (+ z 1)
                  (message "Finished calculating a"))))
  (* z x))

-| Calculating z...
-| Calculating y...
-| Calculating x...
-| Finished calculating x
-| Finished calculating y
-| Finished calculating z
⇒ 8
```

`thunk-let` and `thunk-let*` use thunks implicitly: their expansion creates helper symbols and binds them to thunks wrapping the binding expressions. All references to the original variables in the body `forms` are then replaced by an expression that calls `thunk-force` with the according helper variable as the argument. So, any code using `thunk-let` or `thunk-let*` could be rewritten to use thunks, but in many cases using these macros results in nicer code than using thunks explicitly.
