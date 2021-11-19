

Next: [Printing Notation](Printing-Notation.html), Previous: [nil and t](nil-and-t.html), Up: [Conventions](Conventions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 1.3.3 Evaluation Notation

A Lisp expression that you can evaluate is called a *form*. Evaluating a form always produces a result, which is a Lisp object. In the examples in this manual, this is indicated with ‘`⇒`’:

```lisp
(car '(1 2))
     ⇒ 1
```

You can read this as “`(car '(1 2))` evaluates to 1”.

When a form is a macro call, it expands into a new form for Lisp to evaluate. We show the result of the expansion with ‘`→`’. We may or may not show the result of the evaluation of the expanded form.

```lisp
(third '(a b c))
     → (car (cdr (cdr '(a b c))))
     ⇒ c
```

To help describe one form, we sometimes show another form that produces identical results. The exact equivalence of two forms is indicated with ‘`≡`’.

```lisp
(make-sparse-keymap) ≡ (list 'keymap)
```
