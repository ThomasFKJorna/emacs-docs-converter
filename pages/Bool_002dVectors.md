

Next: [Rings](Rings.html), Previous: [Char-Tables](Char_002dTables.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 6.7 Bool-vectors

A bool-vector is much like a vector, except that it stores only the values `t` and `nil`. If you try to store any non-`nil` value into an element of the bool-vector, the effect is to store `t` there. As with all arrays, bool-vector indices start from 0, and the length cannot be changed once the bool-vector is created. Bool-vectors are constants when evaluated.

Several functions work specifically with bool-vectors; aside from that, you manipulate them with same functions used for other kinds of arrays.

*   Function: **make-bool-vector** *length initial*

    Return a new bool-vector of `length` elements, each one initialized to `initial`.

<!---->

*   Function: **bool-vector** *\&rest objects*

    This function creates and returns a bool-vector whose elements are the arguments, `objects`.

<!---->

*   Function: **bool-vector-p** *object*

    This returns `t` if `object` is a bool-vector, and `nil` otherwise.

There are also some bool-vector set operation functions, described below:

*   Function: **bool-vector-exclusive-or** *a b \&optional c*

    Return *bitwise exclusive or* of bool vectors `a` and `b`. If optional argument `c` is given, the result of this operation is stored into `c`. All arguments should be bool vectors of the same length.

<!---->

*   Function: **bool-vector-union** *a b \&optional c*

    Return *bitwise or* of bool vectors `a` and `b`. If optional argument `c` is given, the result of this operation is stored into `c`. All arguments should be bool vectors of the same length.

<!---->

*   Function: **bool-vector-intersection** *a b \&optional c*

    Return *bitwise and* of bool vectors `a` and `b`. If optional argument `c` is given, the result of this operation is stored into `c`. All arguments should be bool vectors of the same length.

<!---->

*   Function: **bool-vector-set-difference** *a b \&optional c*

    Return *set difference* of bool vectors `a` and `b`. If optional argument `c` is given, the result of this operation is stored into `c`. All arguments should be bool vectors of the same length.

<!---->

*   Function: **bool-vector-not** *a \&optional b*

    Return *set complement* of bool vector `a`. If optional argument `b` is given, the result of this operation is stored into `b`. All arguments should be bool vectors of the same length.

<!---->

*   Function: **bool-vector-subsetp** *a b*

    Return `t` if every `t` value in `a` is also `t` in `b`, `nil` otherwise. All arguments should be bool vectors of the same length.

<!---->

*   Function: **bool-vector-count-consecutive** *a b i*

    Return the number of consecutive elements in `a` equal `b` starting at `i`. `a` is a bool vector, `b` is `t` or `nil`, and `i` is an index into `a`.

<!---->

*   Function: **bool-vector-count-population** *a*

    Return the number of elements that are `t` in bool vector `a`.

The printed form represents up to 8 boolean values as a single character:

```lisp
(bool-vector t nil t nil)
     ⇒ #&4"^E"
(bool-vector)
     ⇒ #&0""
```

You can use `vconcat` to print a bool-vector like other vectors:

```lisp
(vconcat (bool-vector nil t nil t))
     ⇒ [nil t nil t]
```

Here is another example of creating, examining, and updating a bool-vector:

```lisp
(setq bv (make-bool-vector 5 t))
     ⇒ #&5"^_"
(aref bv 1)
     ⇒ t
(aset bv 3 nil)
     ⇒ nil
bv
     ⇒ #&5"^W"
```

These results make sense because the binary codes for control-\_ and control-W are 11111 and 10111, respectively.

Next: [Rings](Rings.html), Previous: [Char-Tables](Char_002dTables.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
