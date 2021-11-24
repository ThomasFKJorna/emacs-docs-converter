

#### 5.6.1 Altering List Elements with `setcar`

Changing the CAR of a cons cell is done with `setcar`. When used on a list, `setcar` replaces one element of a list with a different element.

### Function: **setcar** *cons object*

This function stores `object` as the new CAR of `cons`, replacing its previous CAR. In other words, it changes the CAR slot of `cons` to refer to `object`. It returns the value `object`. For example:

```lisp
(setq x (list 1 2))
     ⇒ (1 2)
```

```lisp
(setcar x 4)
     ⇒ 4
```

```lisp
x
     ⇒ (4 2)
```

When a cons cell is part of the shared structure of several lists, storing a new CAR into the cons changes one element of each of these lists. Here is an example:

```lisp
;; Create two lists that are partly shared.
(setq x1 (list 'a 'b 'c))
     ⇒ (a b c)
(setq x2 (cons 'z (cdr x1)))
     ⇒ (z b c)
```

```lisp
```

```lisp
;; Replace the CAR of a shared link.
(setcar (cdr x1) 'foo)
     ⇒ foo
x1                           ; Both lists are changed.
     ⇒ (a foo c)
x2
     ⇒ (z foo c)
```

```lisp
```

```lisp
;; Replace the CAR of a link that is not shared.
(setcar x1 'baz)
     ⇒ baz
x1                           ; Only one list is changed.
     ⇒ (baz foo c)
x2
     ⇒ (z foo c)
```

Here is a graphical depiction of the shared structure of the two lists in the variables `x1` and `x2`, showing why replacing `b` changes them both:

```lisp
        --- ---        --- ---      --- ---
x1---> |   |   |----> |   |   |--> |   |   |--> nil
        --- ---        --- ---      --- ---
         |        -->   |            |
         |       |      |            |
          --> a  |       --> b        --> c
                 |
       --- ---   |
x2--> |   |   |--
       --- ---
        |
        |
         --> z
```

Here is an alternative form of box diagram, showing the same relationship:

```lisp
x1:
 --------------       --------------       --------------
| car   | cdr  |     | car   | cdr  |     | car   | cdr  |
|   a   |   o------->|   b   |   o------->|   c   |  nil |
|       |      |  -->|       |      |     |       |      |
 --------------  |    --------------       --------------
                 |
x2:              |
 --------------  |
| car   | cdr  | |
|   z   |   o----
|       |      |
 --------------
```
