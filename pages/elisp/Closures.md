

### 13.10 Closures

As explained in [Variable Scoping](Variable-Scoping.html), Emacs can optionally enable lexical binding of variables. When lexical binding is enabled, any named function that you create (e.g., with `defun`), as well as any anonymous function that you create using the `lambda` macro or the `function` special form or the `#'` syntax (see [Anonymous Functions](Anonymous-Functions.html)), is automatically converted into a *closure*.

A closure is a function that also carries a record of the lexical environment that existed when the function was defined. When it is invoked, any lexical variable references within its definition use the retained lexical environment. In all other respects, closures behave much like ordinary functions; in particular, they can be called in the same way as ordinary functions.

See [Lexical Binding](Lexical-Binding.html), for an example of using a closure.

Currently, an Emacs Lisp closure object is represented by a list with the symbol `closure` as the first element, a list representing the lexical environment as the second element, and the argument list and body forms as the remaining elements:

```lisp
;; lexical binding is enabled.
(lambda (x) (* x x))
     ⇒ (closure (t) (x) (* x x))
```

However, the fact that the internal structure of a closure is exposed to the rest of the Lisp world is considered an internal implementation detail. For this reason, we recommend against directly examining or altering the structure of closure objects.
