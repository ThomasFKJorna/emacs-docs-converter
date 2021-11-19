

Next: [Array Type](Array-Type.html), Previous: [Sequence Type](Sequence-Type.html), Up: [Programming Types](Programming-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.4.6 Cons Cell and List Types

A *cons cell* is an object that consists of two slots, called the CAR slot and the CDR slot. Each slot can *hold* any Lisp object. We also say that the CAR of this cons cell is whatever object its CAR slot currently holds, and likewise for the CDR.

A *list* is a series of cons cells, linked together so that the CDR slot of each cons cell holds either the next cons cell or the empty list. The empty list is actually the symbol `nil`. See [Lists](Lists.html), for details. Because most cons cells are used as part of lists, we refer to any structure made out of cons cells as a *list structure*.

> A note to C programmers: a Lisp list thus works as a *linked list* built up of cons cells. Because pointers in Lisp are implicit, we do not distinguish between a cons cell slot holding a value versus pointing to the value.

Because cons cells are so central to Lisp, we also have a word for an object which is not a cons cell. These objects are called *atoms*.

The read syntax and printed representation for lists are identical, and consist of a left parenthesis, an arbitrary number of elements, and a right parenthesis. Here are examples of lists:

```lisp
(A 2 "A")            ; A list of three elements.
()                   ; A list of no elements (the empty list).
nil                  ; A list of no elements (the empty list).
("A ()")             ; A list of one element: the string "A ()".
(A ())               ; A list of two elements: A and the empty list.
(A nil)              ; Equivalent to the previous.
((A B C))            ; A list of one element
                     ;   (which is a list of three elements).
```

Upon reading, each object inside the parentheses becomes an element of the list. That is, a cons cell is made for each element. The CAR slot of the cons cell holds the element, and its CDR slot refers to the next cons cell of the list, which holds the next element in the list. The CDR slot of the last cons cell is set to hold `nil`.

The names CAR and CDR derive from the history of Lisp. The original Lisp implementation ran on an IBM 704 computer which divided words into two parts, the address and the decrement; CAR was an instruction to extract the contents of the address part of a register, and CDR an instruction to extract the contents of the decrement. By contrast, cons cells are named for the function `cons` that creates them, which in turn was named for its purpose, the construction of cells.

|                                                       |    |                                  |
| :---------------------------------------------------- | -- | :------------------------------- |
| • [Box Diagrams](Box-Diagrams.html)                   |    | Drawing pictures of lists.       |
| • [Dotted Pair Notation](Dotted-Pair-Notation.html)   |    | A general syntax for cons cells. |
| • [Association List Type](Association-List-Type.html) |    | A specially constructed list.    |

Next: [Array Type](Array-Type.html), Previous: [Sequence Type](Sequence-Type.html), Up: [Programming Types](Programming-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
