

Next: [Type Predicates](Type-Predicates.html), Previous: [Editing Types](Editing-Types.html), Up: [Lisp Data Types](Lisp-Data-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 2.6 Read Syntax for Circular Objects

To represent shared or circular structures within a complex of Lisp objects, you can use the reader constructs ‘`#n=`’ and ‘`#n#`’.

Use `#n=` before an object to label it for later reference; subsequently, you can use `#n#` to refer the same object in another place. Here, `n` is some integer. For example, here is how to make a list in which the first element recurs as the third element:

```lisp
(#1=(a) b #1#)
```

This differs from ordinary syntax such as this

```lisp
((a) b (a))
```

which would result in a list whose first and third elements look alike but are not the same Lisp object. This shows the difference:

```lisp
(prog1 nil
  (setq x '(#1=(a) b #1#)))
(eq (nth 0 x) (nth 2 x))
     ⇒ t
(setq x '((a) b (a)))
(eq (nth 0 x) (nth 2 x))
     ⇒ nil
```

You can also use the same syntax to make a circular structure, which appears as an element within itself. Here is an example:

```lisp
#1=(a #1#)
```

This makes a list whose second element is the list itself. Here’s how you can see that it really works:

```lisp
(prog1 nil
  (setq x '#1=(a #1#)))
(eq x (cadr x))
     ⇒ t
```

The Lisp printer can produce this syntax to record circular and shared structure in a Lisp object, if you bind the variable `print-circle` to a non-`nil` value. See [Output Variables](Output-Variables.html).

Next: [Type Predicates](Type-Predicates.html), Previous: [Editing Types](Editing-Types.html), Up: [Lisp Data Types](Lisp-Data-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
