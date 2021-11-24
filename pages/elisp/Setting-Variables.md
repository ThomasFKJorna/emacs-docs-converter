

### 12.8 Setting Variable Values

The usual way to change the value of a variable is with the special form `setq`. When you need to compute the choice of variable at run time, use the function `set`.

### Special Form: **setq** *\[symbol form]…*

This special form is the most common method of changing a variable’s value. Each `symbol` is given a new value, which is the result of evaluating the corresponding `form`. The current binding of the symbol is changed.

`setq` does not evaluate `symbol`; it sets the symbol that you write. We say that this argument is *automatically quoted*. The ‘`q`’ in `setq` stands for “quoted”.

The value of the `setq` form is the value of the last `form`.

```lisp
(setq x (1+ 2))
     ⇒ 3
```

```lisp
x                   ; x now has a global value.
     ⇒ 3
```

```lisp
(let ((x 5))
  (setq x 6)        ; The local binding of x is set.
  x)
     ⇒ 6
```

```lisp
x                   ; The global value is unchanged.
     ⇒ 3
```

Note that the first `form` is evaluated, then the first `symbol` is set, then the second `form` is evaluated, then the second `symbol` is set, and so on:

```lisp
(setq x 10          ; Notice that x is set before
      y (1+ x))     ;   the value of y is computed.
     ⇒ 11
```

### Function: **set** *symbol value*

This function puts `value` in the value cell of `symbol`. Since it is a function rather than a special form, the expression written for `symbol` is evaluated to obtain the symbol to set. The return value is `value`.

When dynamic variable binding is in effect (the default), `set` has the same effect as `setq`, apart from the fact that `set` evaluates its `symbol` argument whereas `setq` does not. But when a variable is lexically bound, `set` affects its *dynamic* value, whereas `setq` affects its current (lexical) value. See [Variable Scoping](Variable-Scoping.html).

```lisp
(set one 1)
error→ Symbol's value as variable is void: one
```

```lisp
(set 'one 1)
     ⇒ 1
```

```lisp
(set 'two 'one)
     ⇒ one
```

```lisp
(set two 2)         ; two evaluates to symbol one.
     ⇒ 2
```

```lisp
one                 ; So it is one that was set.
     ⇒ 2
(let ((one 1))      ; This binding of one is set,
  (set 'one 3)      ;   not the global value.
  one)
     ⇒ 3
```

```lisp
one
     ⇒ 2
```

If `symbol` is not actually a symbol, a `wrong-type-argument` error is signaled.

```lisp
(set '(x y) 'z)
error→ Wrong type argument: symbolp, (x y)
```
