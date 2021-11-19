

Next: [Property Lists](Property-Lists.html), Previous: [Sets And Lists](Sets-And-Lists.html), Up: [Lists](Lists.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 5.8 Association Lists

An *association list*, or *alist* for short, records a mapping from keys to values. It is a list of cons cells called *associations*: the CAR of each cons cell is the *key*, and the CDR is the *associated value*.[5](#FOOT5)

Here is an example of an alist. The key `pine` is associated with the value `cones`; the key `oak` is associated with `acorns`; and the key `maple` is associated with `seeds`.

```lisp
((pine . cones)
 (oak . acorns)
 (maple . seeds))
```

Both the values and the keys in an alist may be any Lisp objects. For example, in the following alist, the symbol `a` is associated with the number `1`, and the string `"b"` is associated with the *list* `(2 3)`, which is the CDR of the alist element:

```lisp
((a . 1) ("b" 2 3))
```

Sometimes it is better to design an alist to store the associated value in the CAR of the CDR of the element. Here is an example of such an alist:

```lisp
((rose red) (lily white) (buttercup yellow))
```

Here we regard `red` as the value associated with `rose`. One advantage of this kind of alist is that you can store other related information—even a list of other items—in the CDR of the CDR. One disadvantage is that you cannot use `rassq` (see below) to find the element containing a given value. When neither of these considerations is important, the choice is a matter of taste, as long as you are consistent about it for any given alist.

The same alist shown above could be regarded as having the associated value in the CDR of the element; the value associated with `rose` would be the list `(red)`.

Association lists are often used to record information that you might otherwise keep on a stack, since new associations may be added easily to the front of the list. When searching an association list for an association with a given key, the first one found is returned, if there is more than one.

In Emacs Lisp, it is *not* an error if an element of an association list is not a cons cell. The alist search functions simply ignore such elements. Many other versions of Lisp signal errors in such cases.

Note that property lists are similar to association lists in several respects. A property list behaves like an association list in which each key can occur only once. See [Property Lists](Property-Lists.html), for a comparison of property lists and association lists.

*   Function: **assoc** *key alist \&optional testfn*

    This function returns the first association for `key` in `alist`, comparing `key` against the alist elements using `testfn` if it is non-`nil` and `equal` otherwise (see [Equality Predicates](Equality-Predicates.html)). It returns `nil` if no association in `alist` has a CAR equal to `key`. For example:

    ```lisp
    (setq trees '((pine . cones) (oak . acorns) (maple . seeds)))
         ⇒ ((pine . cones) (oak . acorns) (maple . seeds))
    (assoc 'oak trees)
         ⇒ (oak . acorns)
    (cdr (assoc 'oak trees))
         ⇒ acorns
    (assoc 'birch trees)
         ⇒ nil
    ```

    Here is another example, in which the keys and values are not symbols:

    ```lisp
    (setq needles-per-cluster
          '((2 "Austrian Pine" "Red Pine")
            (3 "Pitch Pine")
            (5 "White Pine")))

    (cdr (assoc 3 needles-per-cluster))
         ⇒ ("Pitch Pine")
    (cdr (assoc 2 needles-per-cluster))
         ⇒ ("Austrian Pine" "Red Pine")
    ```

The function `assoc-string` is much like `assoc` except that it ignores certain differences between strings. See [Text Comparison](Text-Comparison.html).

*   Function: **rassoc** *value alist*

    This function returns the first association with value `value` in `alist`. It returns `nil` if no association in `alist` has a CDR `equal` to `value`.

    `rassoc` is like `assoc` except that it compares the CDR of each `alist` association instead of the CAR. You can think of this as reverse `assoc`, finding the key for a given value.

<!---->

*   Function: **assq** *key alist*

    This function is like `assoc` in that it returns the first association for `key` in `alist`, but it makes the comparison using `eq`. `assq` returns `nil` if no association in `alist` has a CAR `eq` to `key`. This function is used more often than `assoc`, since `eq` is faster than `equal` and most alists use symbols as keys. See [Equality Predicates](Equality-Predicates.html).

    ```lisp
    (setq trees '((pine . cones) (oak . acorns) (maple . seeds)))
         ⇒ ((pine . cones) (oak . acorns) (maple . seeds))
    (assq 'pine trees)
         ⇒ (pine . cones)
    ```

    On the other hand, `assq` is not usually useful in alists where the keys may not be symbols:

    ```lisp
    (setq leaves
          '(("simple leaves" . oak)
            ("compound leaves" . horsechestnut)))

    (assq "simple leaves" leaves)
         ⇒ Unspecified; might be nil or ("simple leaves" . oak).
    (assoc "simple leaves" leaves)
         ⇒ ("simple leaves" . oak)
    ```

<!---->

*   Function: **alist-get** *key alist \&optional default remove testfn*

    This function is similar to `assq`. It finds the first association `(key . value)` by comparing `key` with `alist` elements, and, if found, returns the `value` of that association. If no association is found, the function returns `default`. Comparison of `key` against `alist` elements uses the function specified by `testfn`, defaulting to `eq`.

    This is a generalized variable (see [Generalized Variables](Generalized-Variables.html)) that can be used to change a value with `setf`. When using it to set a value, optional argument `remove` non-`nil` means to remove `key`’s association from `alist` if the new value is `eql` to `default`.

<!---->

*   Function: **rassq** *value alist*

    This function returns the first association with value `value` in `alist`. It returns `nil` if no association in `alist` has a CDR `eq` to `value`.

    `rassq` is like `assq` except that it compares the CDR of each `alist` association instead of the CAR. You can think of this as reverse `assq`, finding the key for a given value.

    For example:

    ```lisp
    (setq trees '((pine . cones) (oak . acorns) (maple . seeds)))

    (rassq 'acorns trees)
         ⇒ (oak . acorns)
    (rassq 'spores trees)
         ⇒ nil
    ```

    `rassq` cannot search for a value stored in the CAR of the CDR of an element:

    ```lisp
    (setq colors '((rose red) (lily white) (buttercup yellow)))

    (rassq 'white colors)
         ⇒ nil
    ```

    In this case, the CDR of the association `(lily white)` is not the symbol `white`, but rather the list `(white)`. This becomes clearer if the association is written in dotted pair notation:

    ```lisp
    (lily white) ≡ (lily . (white))
    ```

<!---->

*   Function: **assoc-default** *key alist \&optional test default*

    This function searches `alist` for a match for `key`. For each element of `alist`, it compares the element (if it is an atom) or the element’s CAR (if it is a cons) against `key`, by calling `test` with two arguments: the element or its CAR, and `key`. The arguments are passed in that order so that you can get useful results using `string-match` with an alist that contains regular expressions (see [Regexp Search](Regexp-Search.html)). If `test` is omitted or `nil`, `equal` is used for comparison.

    If an alist element matches `key` by this criterion, then `assoc-default` returns a value based on this element. If the element is a cons, then the value is the element’s CDR. Otherwise, the return value is `default`.

    If no alist element matches `key`, `assoc-default` returns `nil`.

<!---->

*   Function: **copy-alist** *alist*

    This function returns a two-level deep copy of `alist`: it creates a new copy of each association, so that you can alter the associations of the new alist without changing the old one.

    ```lisp
    (setq needles-per-cluster
          '((2 . ("Austrian Pine" "Red Pine"))
            (3 . ("Pitch Pine"))
    ```

    ```lisp
            (5 . ("White Pine"))))
    ⇒
    ((2 "Austrian Pine" "Red Pine")
     (3 "Pitch Pine")
     (5 "White Pine"))

    (setq copy (copy-alist needles-per-cluster))
    ⇒
    ((2 "Austrian Pine" "Red Pine")
     (3 "Pitch Pine")
     (5 "White Pine"))

    (eq needles-per-cluster copy)
         ⇒ nil
    (equal needles-per-cluster copy)
         ⇒ t
    (eq (car needles-per-cluster) (car copy))
         ⇒ nil
    (cdr (car (cdr needles-per-cluster)))
         ⇒ ("Pitch Pine")
    ```

    ```lisp
    (eq (cdr (car (cdr needles-per-cluster)))
        (cdr (car (cdr copy))))
         ⇒ t
    ```

    This example shows how `copy-alist` makes it possible to change the associations of one copy without affecting the other:

    ```lisp
    (setcdr (assq 3 copy) '("Martian Vacuum Pine"))
    (cdr (assq 3 needles-per-cluster))
         ⇒ ("Pitch Pine")
    ```

<!---->

*   Function: **assq-delete-all** *key alist*

    This function deletes from `alist` all the elements whose CAR is `eq` to `key`, much as if you used `delq` to delete each such element one by one. It returns the shortened alist, and often modifies the original list structure of `alist`. For correct results, use the return value of `assq-delete-all` rather than looking at the saved value of `alist`.

    ```lisp
    (setq alist (list '(foo 1) '(bar 2) '(foo 3) '(lose 4)))
         ⇒ ((foo 1) (bar 2) (foo 3) (lose 4))
    (assq-delete-all 'foo alist)
         ⇒ ((bar 2) (lose 4))
    alist
         ⇒ ((foo 1) (bar 2) (lose 4))
    ```

<!---->

*   Function: **assoc-delete-all** *key alist \&optional test*

    This function is like `assq-delete-all` except that it accepts an optional argument `test`, a predicate function to compare the keys in `alist`. If omitted or `nil`, `test` defaults to `equal`. As `assq-delete-all`, this function often modifies the original list structure of `alist`.

<!---->

*   Function: **rassq-delete-all** *value alist*

    This function deletes from `alist` all the elements whose CDR is `eq` to `value`. It returns the shortened alist, and often modifies the original list structure of `alist`. `rassq-delete-all` is like `assq-delete-all` except that it compares the CDR of each `alist` association instead of the CAR.

<!---->

*   Macro: **let-alist** *alist body*

    Creates a binding for each symbol used as keys the association list `alist`, prefixed with dot. This can be useful when accessing several items in the same association list, and it’s best understood through a simple example:

    ```lisp
    (setq colors '((rose . red) (lily . white) (buttercup . yellow)))
    (let-alist colors
      (if (eq .rose 'red)
          .lily))
    => white
    ```

    The `body` is inspected at compilation time, and only the symbols that appear in `body` with a ‘`.`’ as the first character in the symbol name will be bound. Finding the keys is done with `assq`, and the `cdr` of the return value of this `assq` is assigned as the value for the binding.

    Nested association lists is supported:

    ```lisp
    (setq colors '((rose . red) (lily (belladonna . yellow) (brindisi . pink))))
    (let-alist colors
      (if (eq .rose 'red)
          .lily.belladonna))
    => yellow
    ```

    Nesting `let-alist` inside each other is allowed, but the code in the inner `let-alist` can’t access the variables bound by the outer `let-alist`.

***

#### Footnotes

##### [(5)](#DOCF5)

This usage of “key” is not related to the term “key sequence”; it means a value used to look up an item in a table. In this case, the table is the alist, and the alist associations are the items.

Next: [Property Lists](Property-Lists.html), Previous: [Sets And Lists](Sets-And-Lists.html), Up: [Lists](Lists.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
