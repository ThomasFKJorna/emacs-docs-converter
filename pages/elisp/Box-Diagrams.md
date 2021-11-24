

#### 2.4.6.1 Drawing Lists as Box Diagrams

A list can be illustrated by a diagram in which the cons cells are shown as pairs of boxes, like dominoes. (The Lisp reader cannot read such an illustration; unlike the textual notation, which can be understood by both humans and computers, the box illustrations can be understood only by humans.) This picture represents the three-element list `(rose violet buttercup)`:

```lisp
    --- ---      --- ---      --- ---
   |   |   |--> |   |   |--> |   |   |--> nil
    --- ---      --- ---      --- ---
     |            |            |
     |            |            |
      --> rose     --> violet   --> buttercup
```

In this diagram, each box represents a slot that can hold or refer to any Lisp object. Each pair of boxes represents a cons cell. Each arrow represents a reference to a Lisp object, either an atom or another cons cell.

In this example, the first box, which holds the CAR of the first cons cell, refers to or holds `rose` (a symbol). The second box, holding the CDR of the first cons cell, refers to the next pair of boxes, the second cons cell. The CAR of the second cons cell is `violet`, and its CDR is the third cons cell. The CDR of the third (and last) cons cell is `nil`.

Here is another diagram of the same list, `(rose violet buttercup)`, sketched in a different manner:

```lisp
 ---------------       ----------------       -------------------
| car   | cdr   |     | car    | cdr   |     | car       | cdr   |
| rose  |   o-------->| violet |   o-------->| buttercup |  nil  |
|       |       |     |        |       |     |           |       |
 ---------------       ----------------       -------------------
```

A list with no elements in it is the *empty list*; it is identical to the symbol `nil`. In other words, `nil` is both a symbol and a list.

Here is the list `(A ())`, or equivalently `(A nil)`, depicted with boxes and arrows:

```lisp
    --- ---      --- ---
   |   |   |--> |   |   |--> nil
    --- ---      --- ---
     |            |
     |            |
      --> A        --> nil
```

Here is a more complex illustration, showing the three-element list, `((pine needles) oak maple)`, the first element of which is a two-element list:

```lisp
    --- ---      --- ---      --- ---
   |   |   |--> |   |   |--> |   |   |--> nil
    --- ---      --- ---      --- ---
     |            |            |
     |            |            |
     |             --> oak      --> maple
     |
     |     --- ---      --- ---
      --> |   |   |--> |   |   |--> nil
           --- ---      --- ---
            |            |
            |            |
             --> pine     --> needles
```

The same list represented in the second box notation looks like this:

```lisp
 --------------       --------------       --------------
| car   | cdr  |     | car   | cdr  |     | car   | cdr  |
|   o   |   o------->| oak   |   o------->| maple |  nil |
|   |   |      |     |       |      |     |       |      |
 -- | ---------       --------------       --------------
    |
    |
    |        --------------       ----------------
    |       | car   | cdr  |     | car     | cdr  |
     ------>| pine  |   o------->| needles |  nil |
            |       |      |     |         |      |
             --------------       ----------------
```
