

### 12.7 Accessing Variable Values

The usual way to reference a variable is to write the symbol which names it. See [Symbol Forms](Symbol-Forms.html).

Occasionally, you may want to reference a variable which is only determined at run time. In that case, you cannot specify the variable name in the text of the program. You can use the `symbol-value` function to extract the value.

### Function: **symbol-value** *symbol*

This function returns the value stored in `symbol`’s value cell. This is where the variable’s current (dynamic) value is stored. If the variable has no local binding, this is simply its global value. If the variable is void, a `void-variable` error is signaled.

If the variable is lexically bound, the value reported by `symbol-value` is not necessarily the same as the variable’s lexical value, which is determined by the lexical environment rather than the symbol’s value cell. See [Variable Scoping](Variable-Scoping.html).

```lisp
(setq abracadabra 5)
     ⇒ 5
```

```lisp
(setq foo 9)
     ⇒ 9
```

```lisp
```

```lisp
;; Here the symbol abracadabra
;;   is the symbol whose value is examined.
(let ((abracadabra 'foo))
  (symbol-value 'abracadabra))
     ⇒ foo
```

```lisp
```

```lisp
;; Here, the value of abracadabra,
;;   which is foo,
;;   is the symbol whose value is examined.
(let ((abracadabra 'foo))
  (symbol-value abracadabra))
     ⇒ 9
```

```lisp
```

```lisp
(symbol-value 'abracadabra)
     ⇒ 5
```
