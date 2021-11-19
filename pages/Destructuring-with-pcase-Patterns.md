

Previous: [Backquote Patterns](Backquote-Patterns.html), Up: [Pattern-Matching Conditional](Pattern_002dMatching-Conditional.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 11.4.4 Destructuring with `pcase` Patterns

Pcase patterns not only express a condition on the form of the objects they can match, but they can also extract sub-fields of those objects. For example we can extract 2 elements from a list that is the value of the variable `my-list` with the following code:

```lisp
  (pcase my-list
    (`(add ,x ,y)  (message "Contains %S and %S" x y)))
```

This will not only extract `x` and `y` but will additionally test that `my-list` is a list containing exactly 3 elements and whose first element is the symbol `add`. If any of those tests fail, `pcase` will immediately return `nil` without calling `message`.

Extraction of multiple values stored in an object is known as *destructuring*. Using `pcase` patterns allows to perform *destructuring binding*, which is similar to a local binding (see [Local Variables](Local-Variables.html)), but gives values to multiple elements of a variable by extracting those values from an object of compatible structure.

The macros described in this section use `pcase` patterns to perform destructuring binding. The condition of the object to be of compatible structure means that the object must match the pattern, because only then the object’s subfields can be extracted. For example:

```lisp
  (pcase-let ((`(add ,x ,y) my-list))
    (message "Contains %S and %S" x y))
```

does the same as the previous example, except that it directly tries to extract `x` and `y` from `my-list` without first verifying if `my-list` is a list which has the right number of elements and has `add` as its first element. The precise behavior when the object does not actually match the pattern is undefined, although the body will not be silently skipped: either an error is signaled or the body is run with some of the variables potentially bound to arbitrary values like `nil`.

The pcase patterns that are useful for destructuring bindings are generally those described in [Backquote Patterns](Backquote-Patterns.html), since they express a specification of the structure of objects that will match.

For an alternative facility for destructuring binding, see [seq-let](Sequence-Functions.html#seq_002dlet).

*   Macro: **pcase-let** *bindings body…*

    Perform destructuring binding of variables according to `bindings`, and then evaluate `body`.

    `bindings` is a list of bindings of the form `(pattern exp)`, where `exp` is an expression to evaluate and `pattern` is a `pcase` pattern.

    All `exp`s are evaluated first, after which they are matched against their respective `pattern`, introducing new variable bindings that can then be used inside `body`. The variable bindings are produced by destructuring binding of elements of `pattern` to the values of the corresponding elements of the evaluated `exp`.

<!---->

*   Macro: **pcase-let\*** *bindings body…*

    Perform destructuring binding of variables according to `bindings`, and then evaluate `body`.

    `bindings` is a list of bindings of the form `(pattern exp)`, where `exp` is an expression to evaluate and `pattern` is a `pcase` pattern. The variable bindings are produced by destructuring binding of elements of `pattern` to the values of the corresponding elements of the evaluated `exp`.

    Unlike `pcase-let`, but similarly to `let*`, each `exp` is matched against its corresponding `pattern` before processing the next element of `bindings`, so the variable bindings introduced in each one of the `bindings` are available in the `exp`s of the `bindings` that follow it, additionally to being available in `body`.

<!---->

*   Macro: **pcase-dolist** *(pattern list) body…*

    Execute `body` once for each element of `list`, on each iteration performing a destructuring binding of variables in `pattern` to the values of the corresponding subfields of the element of `list`. The bindings are performed as if by `pcase-let`. When `pattern` is a simple variable, this ends up being equivalent to `dolist` (see [Iteration](Iteration.html)).

Previous: [Backquote Patterns](Backquote-Patterns.html), Up: [Pattern-Matching Conditional](Pattern_002dMatching-Conditional.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
