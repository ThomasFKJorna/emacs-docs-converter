

Next: [Association List Type](Association-List-Type.html), Previous: [Box Diagrams](Box-Diagrams.html), Up: [Cons Cell Type](Cons-Cell-Type.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.4.6.2 Dotted Pair Notation

*Dotted pair notation* is a general syntax for cons cells that represents the CAR and CDR explicitly. In this syntax, `(a . b)` stands for a cons cell whose CAR is the object `a` and whose CDR is the object `b`. Dotted pair notation is more general than list syntax because the CDR does not have to be a list. However, it is more cumbersome in cases where list syntax would work. In dotted pair notation, the list ‘`(1 2 3)`’ is written as ‘`(1 . (2 . (3 . nil)))`’. For `nil`-terminated lists, you can use either notation, but list notation is usually clearer and more convenient. When printing a list, the dotted pair notation is only used if the CDR of a cons cell is not a list.

Here’s an example using boxes to illustrate dotted pair notation. This example shows the pair `(rose . violet)`:

```lisp
    --- ---
   |   |   |--> violet
    --- ---
     |
     |
      --> rose
```

You can combine dotted pair notation with list notation to represent conveniently a chain of cons cells with a non-`nil` final CDR. You write a dot after the last element of the list, followed by the CDR of the final cons cell. For example, `(rose violet . buttercup)` is equivalent to `(rose . (violet . buttercup))`. The object looks like this:

```lisp
    --- ---      --- ---
   |   |   |--> |   |   |--> buttercup
    --- ---      --- ---
     |            |
     |            |
      --> rose     --> violet
```

The syntax `(rose . violet . buttercup)` is invalid because there is nothing that it could mean. If anything, it would say to put `buttercup` in the CDR of a cons cell whose CDR is already used for `violet`.

The list `(rose violet)` is equivalent to `(rose . (violet))`, and looks like this:

```lisp
    --- ---      --- ---
   |   |   |--> |   |   |--> nil
    --- ---      --- ---
     |            |
     |            |
      --> rose     --> violet
```

Similarly, the three-element list `(rose violet buttercup)` is equivalent to `(rose . (violet . (buttercup)))`. It looks like this:

```lisp
    --- ---      --- ---      --- ---
   |   |   |--> |   |   |--> |   |   |--> nil
    --- ---      --- ---      --- ---
     |            |            |
     |            |            |
      --> rose     --> violet   --> buttercup
```

Next: [Association List Type](Association-List-Type.html), Previous: [Box Diagrams](Box-Diagrams.html), Up: [Cons Cell Type](Cons-Cell-Type.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
