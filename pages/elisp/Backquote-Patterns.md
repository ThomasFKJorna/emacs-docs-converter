

#### 11.4.3 Backquote-Style Patterns

This subsection describes *backquote-style patterns*, a set of builtin patterns that eases structural matching. For background, see [Pattern-Matching Conditional](Pattern_002dMatching-Conditional.html).

Backquote-style patterns are a powerful set of `pcase` pattern extensions (created using `pcase-defmacro`) that make it easy to match `expval` against specifications of its *structure*.

For example, to match `expval` that must be a list of two elements whose first element is a specific string and the second element is any value, you can write a core pattern:

```lisp
(and (pred listp)
     ls
```

```lisp
     (guard (= 2 (length ls)))
     (guard (string= "first" (car ls)))
     (let second-elem (cadr ls)))
```

or you can write the equivalent backquote-style pattern:

```lisp
`("first" ,second-elem)
```

The backquote-style pattern is more concise, resembles the structure of `expval`, and avoids binding `ls`.

A backquote-style pattern has the form `` `qpat `` where `qpat` can have the following forms:

`(qpat1 . qpat2)`

Matches if `expval` is a cons cell whose `car` matches `qpat1` and whose `cdr` matches `qpat2`. This readily generalizes to lists as in `(qpat1 qpat2 …)`.

`[qpat1 qpat2 … qpatm]`

Matches if `expval` is a vector of length `m` whose `0`..`(m-1)`th elements match `qpat1`, `qpat2` … `qpatm`, respectively.

*   `symbol`
*   `keyword`
*   `number`
*   `string`

Matches if the corresponding element of `expval` is `equal` to the specified literal object.

`,pattern`

Matches if the corresponding element of `expval` matches `pattern`. Note that `pattern` is any kind that `pcase` supports. (In the example above, `second-elem` is a `symbol` core pattern; it therefore matches anything, and let-binds `second-elem`.)

The *corresponding element* is the portion of `expval` that is in the same structural position as the structural position of `qpat` in the backquote-style pattern. (In the example above, the corresponding element of `second-elem` is the second element of `expval`.)

Here is an example of using `pcase` to implement a simple interpreter for a little expression language (note that this requires lexical binding for the lambda expression in the `fn` clause to properly capture `body` and `arg` (see [Lexical Binding](Lexical-Binding.html)):

```lisp
(defun evaluate (form env)
  (pcase form
    (`(add ,x ,y)       (+ (evaluate x env)
                           (evaluate y env)))
```

```lisp
    (`(call ,fun ,arg)  (funcall (evaluate fun env)
                                 (evaluate arg env)))
    (`(fn ,arg ,body)   (lambda (val)
                          (evaluate body (cons (cons arg val)
                                               env))))
```

```lisp
    ((pred numberp)     form)
    ((pred symbolp)     (cdr (assq form env)))
    (_                  (error "Syntax error: %S" form))))
```

The first three clauses use backquote-style patterns. `` `(add ,x ,y) `` is a pattern that checks that `form` is a three-element list starting with the literal symbol `add`, then extracts the second and third elements and binds them to symbols `x` and `y`, respectively. The clause body evaluates `x` and `y` and adds the results. Similarly, the `call` clause implements a function call, and the `fn` clause implements an anonymous function definition.

The remaining clauses use core patterns. `(pred numberp)` matches if `form` is a number. On match, the body evaluates it. `(pred symbolp)` matches if `form` is a symbol. On match, the body looks up the symbol in `env` and returns its association. Finally, `_` is the catch-all pattern that matches anything, so it’s suitable for reporting syntax errors.

Here are some sample programs in this small language, including their evaluation results:

```lisp
(evaluate '(add 1 2) nil)                 ⇒ 3
(evaluate '(add x y) '((x . 1) (y . 2)))  ⇒ 3
(evaluate '(call (fn x (add 1 x)) 2) nil) ⇒ 3
(evaluate '(sub 1 2) nil)                 ⇒ error
```
