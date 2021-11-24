

### 5.4 Building Cons Cells and Lists

Many functions build lists, as lists reside at the very heart of Lisp. `cons` is the fundamental list-building function; however, it is interesting to note that `list` is used more times in the source code for Emacs than `cons`.

### Function: **cons** *object1 object2*

This function is the most basic function for building new list structure. It creates a new cons cell, making `object1` the CAR, and `object2` the CDR. It then returns the new cons cell. The arguments `object1` and `object2` may be any Lisp objects, but most often `object2` is a list.

```lisp
(cons 1 '(2))
     ⇒ (1 2)
```

```lisp
(cons 1 '())
     ⇒ (1)
```

```lisp
(cons 1 2)
     ⇒ (1 . 2)
```

`cons` is often used to add a single element to the front of a list. This is called *consing the element onto the list*. [4](#FOOT4) For example:

```lisp
(setq list (cons newelt list))
```

Note that there is no conflict between the variable named `list` used in this example and the function named `list` described below; any symbol can serve both purposes.

### Function: **list** *\&rest objects*

This function creates a list with `objects` as its elements. The resulting list is always `nil`-terminated. If no `objects` are given, the empty list is returned.

```lisp
(list 1 2 3 4 5)
     ⇒ (1 2 3 4 5)
```

```lisp
(list 1 2 '(3 4 5) 'foo)
     ⇒ (1 2 (3 4 5) foo)
```

```lisp
(list)
     ⇒ nil
```

### Function: **make-list** *length object*

This function creates a list of `length` elements, in which each element is `object`. Compare `make-list` with `make-string` (see [Creating Strings](Creating-Strings.html)).

```lisp
(make-list 3 'pigs)
     ⇒ (pigs pigs pigs)
```

```lisp
(make-list 0 'pigs)
     ⇒ nil
```

```lisp
(setq l (make-list 3 '(a b)))
     ⇒ ((a b) (a b) (a b))
(eq (car l) (cadr l))
     ⇒ t
```

### Function: **append** *\&rest sequences*

This function returns a list containing all the elements of `sequences`. The `sequences` may be lists, vectors, bool-vectors, or strings, but the last one should usually be a list. All arguments except the last one are copied, so none of the arguments is altered. (See `nconc` in [Rearrangement](Rearrangement.html), for a way to join lists with no copying.)

More generally, the final argument to `append` may be any Lisp object. The final argument is not copied or converted; it becomes the CDR of the last cons cell in the new list. If the final argument is itself a list, then its elements become in effect elements of the result list. If the final element is not a list, the result is a dotted list since its final CDR is not `nil` as required in a proper list (see [Cons Cells](Cons-Cells.html)).

Here is an example of using `append`:

```lisp
(setq trees '(pine oak))
     ⇒ (pine oak)
(setq more-trees (append '(maple birch) trees))
     ⇒ (maple birch pine oak)
```

```lisp
```

```lisp
trees
     ⇒ (pine oak)
more-trees
     ⇒ (maple birch pine oak)
```

```lisp
(eq trees (cdr (cdr more-trees)))
     ⇒ t
```

You can see how `append` works by looking at a box diagram. The variable `trees` is set to the list `(pine oak)` and then the variable `more-trees` is set to the list `(maple birch pine oak)`. However, the variable `trees` continues to refer to the original list:

```lisp
more-trees                trees
|                           |
|     --- ---      --- ---   -> --- ---      --- ---
 --> |   |   |--> |   |   |--> |   |   |--> |   |   |--> nil
      --- ---      --- ---      --- ---      --- ---
       |            |            |            |
       |            |            |            |
        --> maple    -->birch     --> pine     --> oak
```

An empty sequence contributes nothing to the value returned by `append`. As a consequence of this, a final `nil` argument forces a copy of the previous argument:

```lisp
trees
     ⇒ (pine oak)
```

```lisp
(setq wood (append trees nil))
     ⇒ (pine oak)
```

```lisp
wood
     ⇒ (pine oak)
```

```lisp
(eq wood trees)
     ⇒ nil
```

This once was the usual way to copy a list, before the function `copy-sequence` was invented. See [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html).

Here we show the use of vectors and strings as arguments to `append`:

```lisp
(append [a b] "cd" nil)
     ⇒ (a b 99 100)
```

With the help of `apply` (see [Calling Functions](Calling-Functions.html)), we can append all the lists in a list of lists:

```lisp
(apply 'append '((a b c) nil (x y z) nil))
     ⇒ (a b c x y z)
```

If no `sequences` are given, `nil` is returned:

```lisp
(append)
     ⇒ nil
```

Here are some examples where the final argument is not a list:

```lisp
(append '(x y) 'z)
     ⇒ (x y . z)
(append '(x y) [z])
     ⇒ (x y . [z])
```

The second example shows that when the final argument is a sequence but not a list, the sequence’s elements do not become elements of the resulting list. Instead, the sequence becomes the final CDR, like any other non-list final argument.

### Function: **copy-tree** *tree \&optional vecp*

This function returns a copy of the tree `tree`. If `tree` is a cons cell, this makes a new cons cell with the same CAR and CDR, then recursively copies the CAR and CDR in the same way.

Normally, when `tree` is anything other than a cons cell, `copy-tree` simply returns `tree`. However, if `vecp` is non-`nil`, it copies vectors too (and operates recursively on their elements).

### Function: **flatten-tree** *tree*

This function returns a “flattened” copy of `tree`, that is, a list containing all the non-`nil` terminal nodes, or leaves, of the tree of cons cells rooted at `tree`. Leaves in the returned list are in the same order as in `tree`.

```lisp
(flatten-tree '(1 (2 . 3) nil (4 5 (6)) 7))
    ⇒(1 2 3 4 5 6 7)
```

### Function: **number-sequence** *from \&optional to separation*

This function returns a list of numbers starting with `from` and incrementing by `separation`, and ending at or just before `to`. `separation` can be positive or negative and defaults to 1. If `to` is `nil` or numerically equal to `from`, the value is the one-element list `(from)`. If `to` is less than `from` with a positive `separation`, or greater than `from` with a negative `separation`, the value is `nil` because those arguments specify an empty sequence.

If `separation` is 0 and `to` is neither `nil` nor numerically equal to `from`, `number-sequence` signals an error, since those arguments specify an infinite sequence.

All arguments are numbers. Floating-point arguments can be tricky, because floating-point arithmetic is inexact. For instance, depending on the machine, it may quite well happen that `(number-sequence 0.4 0.6 0.2)` returns the one element list `(0.4)`, whereas `(number-sequence 0.4 0.8 0.2)` returns a list with three elements. The `n`th element of the list is computed by the exact formula `(+ from (* n separation))`. Thus, if one wants to make sure that `to` is included in the list, one can pass an expression of this exact type for `to`. Alternatively, one can replace `to` with a slightly larger value (or a slightly more negative value if `separation` is negative).

Some examples:

```lisp
(number-sequence 4 9)
     ⇒ (4 5 6 7 8 9)
(number-sequence 9 4 -1)
     ⇒ (9 8 7 6 5 4)
(number-sequence 9 4 -2)
     ⇒ (9 7 5)
(number-sequence 8)
     ⇒ (8)
(number-sequence 8 5)
     ⇒ nil
(number-sequence 5 8 -1)
     ⇒ nil
(number-sequence 1.5 6 2)
     ⇒ (1.5 3.5 5.5)
```

***

#### Footnotes

##### [(4)](#DOCF4)

There is no strictly equivalent way to add an element to the end of a list. You can use `(append listname (list newelt))`, which creates a whole new list by copying `listname` and adding `newelt` to its end. Or you can use `(nconc listname (list newelt))`, which modifies `listname` by following all the CDRs and then replacing the terminating `nil`. Compare this to adding an element to the beginning of a list with `cons`, which neither copies nor modifies the list.
