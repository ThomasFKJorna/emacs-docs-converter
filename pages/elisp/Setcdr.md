

#### 5.6.2 Altering the CDR of a List

The lowest-level primitive for modifying a CDR is `setcdr`:

### Function: **setcdr** *cons object*

This function stores `object` as the new CDR of `cons`, replacing its previous CDR. In other words, it changes the CDR slot of `cons` to refer to `object`. It returns the value `object`.

Here is an example of replacing the CDR of a list with a different list. All but the first element of the list are removed in favor of a different sequence of elements. The first element is unchanged, because it resides in the CAR of the list, and is not reached via the CDR.

```lisp
(setq x (list 1 2 3))
     ⇒ (1 2 3)
```

```lisp
(setcdr x '(4))
     ⇒ (4)
```

```lisp
x
     ⇒ (1 4)
```

You can delete elements from the middle of a list by altering the CDRs of the cons cells in the list. For example, here we delete the second element, `b`, from the list `(a b c)`, by changing the CDR of the first cons cell:

```lisp
(setq x1 (list 'a 'b 'c))
     ⇒ (a b c)
(setcdr x1 (cdr (cdr x1)))
     ⇒ (c)
x1
     ⇒ (a c)
```

Here is the result in box notation:

```lisp
                   --------------------
                  |                    |
 --------------   |   --------------   |    --------------
| car   | cdr  |  |  | car   | cdr  |   -->| car   | cdr  |
|   a   |   o-----   |   b   |   o-------->|   c   |  nil |
|       |      |     |       |      |      |       |      |
 --------------       --------------        --------------
```

The second cons cell, which previously held the element `b`, still exists and its CAR is still `b`, but it no longer forms part of this list.

It is equally easy to insert a new element by changing CDRs:

```lisp
(setq x1 (list 'a 'b 'c))
     ⇒ (a b c)
(setcdr x1 (cons 'd (cdr x1)))
     ⇒ (d b c)
x1
     ⇒ (a d b c)
```

Here is this result in box notation:

```lisp
 --------------        -------------       -------------
| car  | cdr   |      | car  | cdr  |     | car  | cdr  |
|   a  |   o   |   -->|   b  |   o------->|   c  |  nil |
|      |   |   |  |   |      |      |     |      |      |
 --------- | --   |    -------------       -------------
           |      |
     -----         --------
    |                      |
    |    ---------------   |
    |   | car   | cdr   |  |
     -->|   d   |   o------
        |       |       |
         ---------------
```
