

Previous: [Bool-Vectors](Bool_002dVectors.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 6.8 Managing a Fixed-Size Ring of Objects

A *ring* is a fixed-size data structure that supports insertion, deletion, rotation, and modulo-indexed reference and traversal. An efficient ring data structure is implemented by the `ring` package. It provides the functions listed in this section.

Note that several rings in Emacs, like the kill ring and the mark ring, are actually implemented as simple lists, *not* using the `ring` package; thus the following functions won’t work on them.

*   Function: **make-ring** *size*

    This returns a new ring capable of holding `size` objects. `size` should be an integer.

<!---->

*   Function: **ring-p** *object*

    This returns `t` if `object` is a ring, `nil` otherwise.

<!---->

*   Function: **ring-size** *ring*

    This returns the maximum capacity of the `ring`.

<!---->

*   Function: **ring-length** *ring*

    This returns the number of objects that `ring` currently contains. The value will never exceed that returned by `ring-size`.

<!---->

*   Function: **ring-elements** *ring*

    This returns a list of the objects in `ring`, in order, newest first.

<!---->

*   Function: **ring-copy** *ring*

    This returns a new ring which is a copy of `ring`. The new ring contains the same (`eq`) objects as `ring`.

<!---->

*   Function: **ring-empty-p** *ring*

    This returns `t` if `ring` is empty, `nil` otherwise.

The newest element in the ring always has index 0. Higher indices correspond to older elements. Indices are computed modulo the ring length. Index -1 corresponds to the oldest element, -2 to the next-oldest, and so forth.

*   Function: **ring-ref** *ring index*

    This returns the object in `ring` found at index `index`. `index` may be negative or greater than the ring length. If `ring` is empty, `ring-ref` signals an error.

<!---->

*   Function: **ring-insert** *ring object*

    This inserts `object` into `ring`, making it the newest element, and returns `object`.

    If the ring is full, insertion removes the oldest element to make room for the new element.

<!---->

*   Function: **ring-remove** *ring \&optional index*

    Remove an object from `ring`, and return that object. The argument `index` specifies which item to remove; if it is `nil`, that means to remove the oldest item. If `ring` is empty, `ring-remove` signals an error.

<!---->

*   Function: **ring-insert-at-beginning** *ring object*

    This inserts `object` into `ring`, treating it as the oldest element. The return value is not significant.

    If the ring is full, this function removes the newest element to make room for the inserted element.

<!---->

*   Function: **ring-resize** *ring size*

    Set the size of `ring` to `size`. If the new size is smaller, then the oldest items in the ring are discarded.

If you are careful not to exceed the ring size, you can use the ring as a first-in-first-out queue. For example:

```lisp
(let ((fifo (make-ring 5)))
  (mapc (lambda (obj) (ring-insert fifo obj))
        '(0 one "two"))
  (list (ring-remove fifo) t
        (ring-remove fifo) t
        (ring-remove fifo)))
     ⇒ (0 t one t "two")
```

Previous: [Bool-Vectors](Bool_002dVectors.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
