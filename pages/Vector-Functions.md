

Next: [Char-Tables](Char_002dTables.html), Previous: [Vectors](Vectors.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 6.5 Functions for Vectors

Here are some functions that relate to vectors:

*   Function: **vectorp** *object*

    This function returns `t` if `object` is a vector.

    ```lisp
    (vectorp [a])
         ⇒ t
    (vectorp "asdf")
         ⇒ nil
    ```

<!---->

*   Function: **vector** *\&rest objects*

    This function creates and returns a vector whose elements are the arguments, `objects`.

    ```lisp
    (vector 'foo 23 [bar baz] "rats")
         ⇒ [foo 23 [bar baz] "rats"]
    (vector)
         ⇒ []
    ```

<!---->

*   Function: **make-vector** *length object*

    This function returns a new vector consisting of `length` elements, each initialized to `object`.

    ```lisp
    (setq sleepy (make-vector 9 'Z))
         ⇒ [Z Z Z Z Z Z Z Z Z]
    ```

<!---->

*   Function: **vconcat** *\&rest sequences*

    This function returns a new vector containing all the elements of `sequences`. The arguments `sequences` may be proper lists, vectors, strings or bool-vectors. If no `sequences` are given, the empty vector is returned.

    The value is either the empty vector, or is a newly constructed nonempty vector that is not `eq` to any existing vector.

    ```lisp
    (setq a (vconcat '(A B C) '(D E F)))
         ⇒ [A B C D E F]
    (eq a (vconcat a))
         ⇒ nil
    ```

    ```lisp
    (vconcat)
         ⇒ []
    (vconcat [A B C] "aa" '(foo (6 7)))
         ⇒ [A B C 97 97 foo (6 7)]
    ```

    The `vconcat` function also allows byte-code function objects as arguments. This is a special feature to make it easy to access the entire contents of a byte-code function object. See [Byte-Code Objects](Byte_002dCode-Objects.html).

    For other concatenation functions, see `mapconcat` in [Mapping Functions](Mapping-Functions.html), `concat` in [Creating Strings](Creating-Strings.html), and `append` in [Building Lists](Building-Lists.html).

The `append` function also provides a way to convert a vector into a list with the same elements:

```lisp
(setq avector [1 two (quote (three)) "four" [five]])
     ⇒ [1 two '(three) "four" [five]]
(append avector nil)
     ⇒ (1 two '(three) "four" [five])
```

Next: [Char-Tables](Char_002dTables.html), Previous: [Vectors](Vectors.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
