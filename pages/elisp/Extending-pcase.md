

#### 11.4.2 Extending `pcase`

The `pcase` macro supports several kinds of patterns (see [Pattern-Matching Conditional](Pattern_002dMatching-Conditional.html)). You can add support for other kinds of patterns using the `pcase-defmacro` macro.

### Macro: **pcase-defmacro** *name args \[doc] \&rest body*

Define a new kind of pattern for `pcase`, to be invoked as `(name actual-args)`. The `pcase` macro expands this into a function call that evaluates `body`, whose job it is to rewrite the invoked pattern into some other pattern, in an environment where `args` are bound to `actual-args`.

Additionally, arrange to display `doc` along with the docstring of `pcase`. By convention, `doc` should use `EXPVAL` to stand for the result of evaluating `expression` (first arg to `pcase`).

Typically, `body` rewrites the invoked pattern to use more basic patterns. Although all patterns eventually reduce to core patterns, `body` need not use core patterns straight away. The following example defines two patterns, named `less-than` and `integer-less-than`.

```lisp
(pcase-defmacro less-than (n)
  "Matches if EXPVAL is a number less than N."
  `(pred (> ,n)))
```

```lisp
```

```lisp
(pcase-defmacro integer-less-than (n)
  "Matches if EXPVAL is an integer less than N."
  `(and (pred integerp)
        (less-than ,n)))
```

Note that the docstrings mention `args` (in this case, only one: `n`) in the usual way, and also mention `EXPVAL` by convention. The first rewrite (i.e., `body` for `less-than`) uses one core pattern: `pred`. The second uses two core patterns: `and` and `pred`, as well as the newly-defined pattern `less-than`. Both use a single backquote construct (see [Backquote](Backquote.html)).
