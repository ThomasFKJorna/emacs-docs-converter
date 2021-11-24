

### 11.4 Pattern-Matching Conditional

Aside from the four basic conditional forms, Emacs Lisp also has a pattern-matching conditional form, the `pcase` macro, a hybrid of `cond` and `cl-case` (see [Conditionals](https://www.gnu.org/software/emacs/manual/html_node/cl/Conditionals.html#Conditionals) in Common Lisp Extensions) that overcomes their limitations and introduces the *pattern matching programming style*. The limitations that `pcase` overcomes are:

The `cond` form chooses among alternatives by evaluating the predicate `condition` of each of its clauses (see [Conditionals](Conditionals.html)). The primary limitation is that variables let-bound in `condition` are not available to the clause’s `body-forms`.

Another annoyance (more an inconvenience than a limitation) is that when a series of `condition` predicates implement equality tests, there is a lot of repeated code. (`cl-case` solves this inconvenience.)

The `cl-case` macro chooses among alternatives by evaluating the equality of its first argument against a set of specific values.

Its limitations are two-fold:

1.  The equality tests use `eql`.
2.  The values must be known and written in advance.

These render `cl-case` unsuitable for strings or compound data structures (e.g., lists or vectors). (`cond` doesn’t have these limitations, but it has others, see above.)

Conceptually, the `pcase` macro borrows the first-arg focus of `cl-case` and the clause-processing flow of `cond`, replacing `condition` with a generalization of the equality test which is a variant of *pattern matching*, and adding facilities so that you can concisely express a clause’s predicate, and arrange to share let-bindings between a clause’s predicate and `body-forms`.

The concise expression of a predicate is known as a *pattern*. When the predicate, called on the value of the first arg, returns non-`nil`, we say that “the pattern matches the value” (or sometimes “the value matches the pattern”).

|                                                                               |    |                                            |
| :---------------------------------------------------------------------------- | -- | :----------------------------------------- |
| • [The `pcase` macro](pcase-Macro.html)                                       |    | Includes examples and caveats.             |
| • [Extending `pcase`](Extending-pcase.html)                                   |    | Define new kinds of patterns.              |
| • [Backquote-Style Patterns](Backquote-Patterns.html)                         |    | Structural patterns matching.              |
| • [Destructuring with pcase Patterns](Destructuring-with-pcase-Patterns.html) |    | Using pcase patterns to extract subfields. |
