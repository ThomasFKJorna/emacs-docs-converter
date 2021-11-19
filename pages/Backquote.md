

Next: [Eval](Eval.html), Previous: [Quoting](Quoting.html), Up: [Evaluation](Evaluation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 10.4 Backquote

*Backquote constructs* allow you to quote a list, but selectively evaluate elements of that list. In the simplest case, it is identical to the special form `quote` (described in the previous section; see [Quoting](Quoting.html)). For example, these two forms yield identical results:

```lisp
`(a list of (+ 2 3) elements)
     ⇒ (a list of (+ 2 3) elements)
```

```lisp
'(a list of (+ 2 3) elements)
     ⇒ (a list of (+ 2 3) elements)
```

The special marker ‘`,`’ inside of the argument to backquote indicates a value that isn’t constant. The Emacs Lisp evaluator evaluates the argument of ‘`,`’, and puts the value in the list structure:

```lisp
`(a list of ,(+ 2 3) elements)
     ⇒ (a list of 5 elements)
```

Substitution with ‘`,`’ is allowed at deeper levels of the list structure also. For example:

```lisp
`(1 2 (3 ,(+ 4 5)))
     ⇒ (1 2 (3 9))
```

You can also *splice* an evaluated value into the resulting list, using the special marker ‘`,@`’. The elements of the spliced list become elements at the same level as the other elements of the resulting list. The equivalent code without using ‘`` ` ``’ is often unreadable. Here are some examples:

```lisp
(setq some-list '(2 3))
     ⇒ (2 3)
```

```lisp
(cons 1 (append some-list '(4) some-list))
     ⇒ (1 2 3 4 2 3)
```

```lisp
`(1 ,@some-list 4 ,@some-list)
     ⇒ (1 2 3 4 2 3)
```

```lisp
```

```lisp
(setq list '(hack foo bar))
     ⇒ (hack foo bar)
```

```lisp
(cons 'use
  (cons 'the
    (cons 'words (append (cdr list) '(as elements)))))
     ⇒ (use the words foo bar as elements)
```

```lisp
`(use the words ,@(cdr list) as elements)
     ⇒ (use the words foo bar as elements)
```

If a subexpression of a backquote construct has no substitutions or splices, it acts like `quote` in that it yields conses, vectors and strings that might be shared and should not be modified. See [Self-Evaluating Forms](Self_002dEvaluating-Forms.html).

Next: [Eval](Eval.html), Previous: [Quoting](Quoting.html), Up: [Evaluation](Evaluation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
