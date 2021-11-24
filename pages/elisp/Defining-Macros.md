

### 14.4 Defining Macros

A Lisp macro object is a list whose CAR is `macro`, and whose CDR is a function. Expansion of the macro works by applying the function (with `apply`) to the list of *unevaluated* arguments from the macro call.

It is possible to use an anonymous Lisp macro just like an anonymous function, but this is never done, because it does not make sense to pass an anonymous macro to functionals such as `mapcar`. In practice, all Lisp macros have names, and they are almost always defined with the `defmacro` macro.

### Macro: **defmacro** *name args \[doc] \[declare] body…*

`defmacro` defines the symbol `name` (which should not be quoted) as a macro that looks like this:

```lisp
(macro lambda args . body)
```

(Note that the CDR of this list is a lambda expression.) This macro object is stored in the function cell of `name`. The meaning of `args` is the same as in a function, and the keywords `&rest` and `&optional` may be used (see [Argument List](Argument-List.html)). Neither `name` nor `args` should be quoted. The return value of `defmacro` is undefined.

`doc`, if present, should be a string specifying the macro’s documentation string. `declare`, if present, should be a `declare` form specifying metadata for the macro (see [Declare Form](Declare-Form.html)). Note that macros cannot have interactive declarations, since they cannot be called interactively.

Macros often need to construct large list structures from a mixture of constants and nonconstant parts. To make this easier, use the ‘`` ` ``’ syntax (see [Backquote](Backquote.html)). For example:

```lisp
(defmacro t-becomes-nil (variable)
  `(if (eq ,variable t)
       (setq ,variable nil)))
```

```lisp
```

```lisp
(t-becomes-nil foo)
     ≡ (if (eq foo t) (setq foo nil))
```
