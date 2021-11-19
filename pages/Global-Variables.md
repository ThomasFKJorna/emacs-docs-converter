

Next: [Constant Variables](Constant-Variables.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 12.1 Global Variables

The simplest way to use a variable is *globally*. This means that the variable has just one value at a time, and this value is in effect (at least for the moment) throughout the Lisp system. The value remains in effect until you specify a new one. When a new value replaces the old one, no trace of the old value remains in the variable.

You specify a value for a symbol with `setq`. For example,

```lisp
(setq x '(a b))
```

gives the variable `x` the value `(a b)`. Note that `setq` is a special form (see [Special Forms](Special-Forms.html)); it does not evaluate its first argument, the name of the variable, but it does evaluate the second argument, the new value.

Once the variable has a value, you can refer to it by using the symbol itself as an expression. Thus,

```lisp
x ⇒ (a b)
```

assuming the `setq` form shown above has already been executed.

If you do set the same variable again, the new value replaces the old one:

```lisp
x
     ⇒ (a b)
```

```lisp
(setq x 4)
     ⇒ 4
```

```lisp
x
     ⇒ 4
```
