

Next: [Symbol Forms](Symbol-Forms.html), Up: [Forms](Forms.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 10.2.1 Self-Evaluating Forms

A *self-evaluating form* is any form that is not a list or symbol. Self-evaluating forms evaluate to themselves: the result of evaluation is the same object that was evaluated. Thus, the number 25 evaluates to 25, and the string `"foo"` evaluates to the string `"foo"`. Likewise, evaluating a vector does not cause evaluation of the elements of the vector—it returns the same vector with its contents unchanged.

```lisp
'123               ; A number, shown without evaluation.
     ⇒ 123
```

```lisp
123                ; Evaluated as usual—result is the same.
     ⇒ 123
```

```lisp
(eval '123)        ; Evaluated "by hand"—result is the same.
     ⇒ 123
```

```lisp
(eval (eval '123)) ; Evaluating twice changes nothing.
     ⇒ 123
```

A self-evaluating form yields a value that becomes part of the program, and you should not try to modify it via `setcar`, `aset` or similar operations. The Lisp interpreter might unify the constants yielded by your program’s self-evaluating forms, so that these constants might share structure. See [Mutability](Mutability.html).

It is common to write numbers, characters, strings, and even vectors in Lisp code, taking advantage of the fact that they self-evaluate. However, it is quite unusual to do this for types that lack a read syntax, because there’s no way to write them textually. It is possible to construct Lisp expressions containing these types by means of a Lisp program. Here is an example:

```lisp
;; Build an expression containing a buffer object.
(setq print-exp (list 'print (current-buffer)))
     ⇒ (print #<buffer eval.texi>)
```

```lisp
;; Evaluate it.
(eval print-exp)
     -| #<buffer eval.texi>
     ⇒ #<buffer eval.texi>
```

Next: [Symbol Forms](Symbol-Forms.html), Up: [Forms](Forms.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
