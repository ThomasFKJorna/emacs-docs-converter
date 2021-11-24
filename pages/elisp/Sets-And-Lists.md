

### 5.7 Using Lists as Sets

A list can represent an unordered mathematical set—simply consider a value an element of a set if it appears in the list, and ignore the order of the list. To form the union of two sets, use `append` (as long as you don’t mind having duplicate elements). You can remove `equal` duplicates using `delete-dups`. Other useful functions for sets include `memq` and `delq`, and their `equal` versions, `member` and `delete`.

> **Common Lisp note:** Common Lisp has functions `union` (which avoids duplicate elements) and `intersection` for set operations. In Emacs Lisp, variants of these facilities are provided by the `cl-lib` library. See [Lists as Sets](https://www.gnu.org/software/emacs/manual/html_node/cl/Lists-as-Sets.html#Lists-as-Sets) in Common Lisp Extensions.

### Function: **memq** *object list*

This function tests to see whether `object` is a member of `list`. If it is, `memq` returns a list starting with the first occurrence of `object`. Otherwise, it returns `nil`. The letter ‘`q`’ in `memq` says that it uses `eq` to compare `object` against the elements of the list. For example:

```lisp
(memq 'b '(a b c b a))
     ⇒ (b c b a)
```

```lisp
(memq '(2) '((1) (2)))    ; The two (2)s need not be eq.
     ⇒ Unspecified; might be nil or ((2)).
```

### Function: **delq** *object list*

This function destructively removes all elements `eq` to `object` from `list`, and returns the resulting list. The letter ‘`q`’ in `delq` says that it uses `eq` to compare `object` against the elements of the list, like `memq` and `remq`.

Typically, when you invoke `delq`, you should use the return value by assigning it to the variable which held the original list. The reason for this is explained below.

The `delq` function deletes elements from the front of the list by simply advancing down the list, and returning a sublist that starts after those elements. For example:

```lisp
(delq 'a '(a b c)) ≡ (cdr '(a b c))
```

When an element to be deleted appears in the middle of the list, removing it involves changing the CDRs (see [Setcdr](Setcdr.html)).

```lisp
(setq sample-list (list 'a 'b 'c '(4)))
     ⇒ (a b c (4))
```

```lisp
(delq 'a sample-list)
     ⇒ (b c (4))
```

```lisp
sample-list
     ⇒ (a b c (4))
```

```lisp
(delq 'c sample-list)
     ⇒ (a b (4))
```

```lisp
sample-list
     ⇒ (a b (4))
```

Note that `(delq 'c sample-list)` modifies `sample-list` to splice out the third element, but `(delq 'a sample-list)` does not splice anything—it just returns a shorter list. Don’t assume that a variable which formerly held the argument `list` now has fewer elements, or that it still holds the original list! Instead, save the result of `delq` and use that. Most often we store the result back into the variable that held the original list:

```lisp
(setq flowers (delq 'rose flowers))
```

In the following example, the `(list 4)` that `delq` attempts to match and the `(4)` in the `sample-list` are `equal` but not `eq`:

```lisp
(delq (list 4) sample-list)
     ⇒ (a c (4))
```

If you want to delete elements that are `equal` to a given value, use `delete` (see below).

### Function: **remq** *object list*

This function returns a copy of `list`, with all elements removed which are `eq` to `object`. The letter ‘`q`’ in `remq` says that it uses `eq` to compare `object` against the elements of `list`.

```lisp
(setq sample-list (list 'a 'b 'c 'a 'b 'c))
     ⇒ (a b c a b c)
```

```lisp
(remq 'a sample-list)
     ⇒ (b c b c)
```

```lisp
sample-list
     ⇒ (a b c a b c)
```

### Function: **memql** *object list*

The function `memql` tests to see whether `object` is a member of `list`, comparing members with `object` using `eql`, so floating-point elements are compared by value. If `object` is a member, `memql` returns a list starting with its first occurrence in `list`. Otherwise, it returns `nil`.

Compare this with `memq`:

```lisp
(memql 1.2 '(1.1 1.2 1.3))  ; 1.2 and 1.2 are eql.
     ⇒ (1.2 1.3)
```

```lisp
(memq 1.2 '(1.1 1.2 1.3))  ; The two 1.2s need not be eq.
     ⇒ Unspecified; might be nil or (1.2 1.3).
```

The following three functions are like `memq`, `delq` and `remq`, but use `equal` rather than `eq` to compare elements. See [Equality Predicates](Equality-Predicates.html).

### Function: **member** *object list*

The function `member` tests to see whether `object` is a member of `list`, comparing members with `object` using `equal`. If `object` is a member, `member` returns a list starting with its first occurrence in `list`. Otherwise, it returns `nil`.

Compare this with `memq`:

```lisp
(member '(2) '((1) (2)))  ; (2) and (2) are equal.
     ⇒ ((2))
```

```lisp
(memq '(2) '((1) (2)))    ; The two (2)s need not be eq.
     ⇒ Unspecified; might be nil or (2).
```

```lisp
;; Two strings with the same contents are equal.
(member "foo" '("foo" "bar"))
     ⇒ ("foo" "bar")
```

### Function: **delete** *object sequence*

This function removes all elements `equal` to `object` from `sequence`, and returns the resulting sequence.

If `sequence` is a list, `delete` is to `delq` as `member` is to `memq`: it uses `equal` to compare elements with `object`, like `member`; when it finds an element that matches, it cuts the element out just as `delq` would. As with `delq`, you should typically use the return value by assigning it to the variable which held the original list.

If `sequence` is a vector or string, `delete` returns a copy of `sequence` with all elements `equal` to `object` removed.

For example:

```lisp
(setq l (list '(2) '(1) '(2)))
(delete '(2) l)
     ⇒ ((1))
l
     ⇒ ((2) (1))
;; If you want to change l reliably,
;; write (setq l (delete '(2) l)).
```

```lisp
(setq l (list '(2) '(1) '(2)))
(delete '(1) l)
     ⇒ ((2) (2))
l
     ⇒ ((2) (2))
;; In this case, it makes no difference whether you set l,
;; but you should do so for the sake of the other case.
```

```lisp
(delete '(2) [(2) (1) (2)])
     ⇒ [(1)]
```

### Function: **remove** *object sequence*

This function is the non-destructive counterpart of `delete`. It returns a copy of `sequence`, a list, vector, or string, with elements `equal` to `object` removed. For example:

```lisp
(remove '(2) '((2) (1) (2)))
     ⇒ ((1))
```

```lisp
(remove '(2) [(2) (1) (2)])
     ⇒ [(1)]
```

> **Common Lisp note:** The functions `member`, `delete` and `remove` in GNU Emacs Lisp are derived from Maclisp, not Common Lisp. The Common Lisp versions do not use `equal` to compare elements.

### Function: **member-ignore-case** *object list*

This function is like `member`, except that `object` should be a string and that it ignores differences in letter-case and text representation: upper-case and lower-case letters are treated as equal, and unibyte strings are converted to multibyte prior to comparison.

### Function: **delete-dups** *list*

This function destructively removes all `equal` duplicates from `list`, stores the result in `list` and returns it. Of several `equal` occurrences of an element in `list`, `delete-dups` keeps the first one.

See also the function `add-to-list`, in [List Variables](List-Variables.html), for a way to add an element to a list stored in a variable and used as a set.
