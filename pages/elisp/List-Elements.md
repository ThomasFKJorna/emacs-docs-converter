

### 5.3 Accessing Elements of Lists

### Function: **car** *cons-cell*

This function returns the value referred to by the first slot of the cons cell `cons-cell`. In other words, it returns the CAR of `cons-cell`.

As a special case, if `cons-cell` is `nil`, this function returns `nil`. Therefore, any list is a valid argument. An error is signaled if the argument is not a cons cell or `nil`.

```lisp
(car '(a b c))
     ⇒ a
```

```lisp
(car '())
     ⇒ nil
```

### Function: **cdr** *cons-cell*

This function returns the value referred to by the second slot of the cons cell `cons-cell`. In other words, it returns the CDR of `cons-cell`.

As a special case, if `cons-cell` is `nil`, this function returns `nil`; therefore, any list is a valid argument. An error is signaled if the argument is not a cons cell or `nil`.

```lisp
(cdr '(a b c))
     ⇒ (b c)
```

```lisp
(cdr '())
     ⇒ nil
```

### Function: **car-safe** *object*

This function lets you take the CAR of a cons cell while avoiding errors for other data types. It returns the CAR of `object` if `object` is a cons cell, `nil` otherwise. This is in contrast to `car`, which signals an error if `object` is not a list.

```lisp
(car-safe object)
≡
(let ((x object))
  (if (consp x)
      (car x)
    nil))
```

### Function: **cdr-safe** *object*

This function lets you take the CDR of a cons cell while avoiding errors for other data types. It returns the CDR of `object` if `object` is a cons cell, `nil` otherwise. This is in contrast to `cdr`, which signals an error if `object` is not a list.

```lisp
(cdr-safe object)
≡
(let ((x object))
  (if (consp x)
      (cdr x)
    nil))
```

### Macro: **pop** *listname*

This macro provides a convenient way to examine the CAR of a list, and take it off the list, all at once. It operates on the list stored in `listname`. It removes the first element from the list, saves the CDR into `listname`, then returns the removed element.

In the simplest case, `listname` is an unquoted symbol naming a list; in that case, this macro is equivalent to `(prog1 (car listname) (setq listname (cdr listname)))`.

```lisp
x
     ⇒ (a b c)
(pop x)
     ⇒ a
x
     ⇒ (b c)
```

More generally, `listname` can be a generalized variable. In that case, this macro saves into `listname` using `setf`. See [Generalized Variables](Generalized-Variables.html).

For the `push` macro, which adds an element to a list, See [List Variables](List-Variables.html).

### Function: **nth** *n list*

This function returns the `n`th element of `list`. Elements are numbered starting with zero, so the CAR of `list` is element number zero. If the length of `list` is `n` or less, the value is `nil`.

```lisp
(nth 2 '(1 2 3 4))
     ⇒ 3
```

```lisp
(nth 10 '(1 2 3 4))
     ⇒ nil

(nth n x) ≡ (car (nthcdr n x))
```

The function `elt` is similar, but applies to any kind of sequence. For historical reasons, it takes its arguments in the opposite order. See [Sequence Functions](Sequence-Functions.html).

### Function: **nthcdr** *n list*

This function returns the `n`th CDR of `list`. In other words, it skips past the first `n` links of `list` and returns what follows.

If `n` is zero, `nthcdr` returns all of `list`. If the length of `list` is `n` or less, `nthcdr` returns `nil`.

```lisp
(nthcdr 1 '(1 2 3 4))
     ⇒ (2 3 4)
```

```lisp
(nthcdr 10 '(1 2 3 4))
     ⇒ nil
```

```lisp
(nthcdr 0 '(1 2 3 4))
     ⇒ (1 2 3 4)
```

### Function: **last** *list \&optional n*

This function returns the last link of `list`. The `car` of this link is the list’s last element. If `list` is null, `nil` is returned. If `n` is non-`nil`, the `n`th-to-last link is returned instead, or the whole of `list` if `n` is bigger than `list`’s length.

### Function: **safe-length** *list*

This function returns the length of `list`, with no risk of either an error or an infinite loop. It generally returns the number of distinct cons cells in the list. However, for circular lists, the value is just an upper bound; it is often too large.

If `list` is not `nil` or a cons cell, `safe-length` returns 0.

The most common way to compute the length of a list, when you are not worried that it may be circular, is with `length`. See [Sequence Functions](Sequence-Functions.html).

### Function: **caar** *cons-cell*

This is the same as `(car (car cons-cell))`.

### Function: **cadr** *cons-cell*

This is the same as `(car (cdr cons-cell))` or `(nth 1 cons-cell)`.

### Function: **cdar** *cons-cell*

This is the same as `(cdr (car cons-cell))`.

### Function: **cddr** *cons-cell*

This is the same as `(cdr (cdr cons-cell))` or `(nthcdr 2 cons-cell)`.

In addition to the above, 24 additional compositions of `car` and `cdr` are defined as `cxxxr` and `cxxxxr`, where each `x` is either `a` or `d`. `cadr`, `caddr`, and `cadddr` pick out the second, third or fourth elements of a list, respectively. `cl-lib` provides the same under the names `cl-second`, `cl-third`, and `cl-fourth`. See [List Functions](https://www.gnu.org/software/emacs/manual/html_node/cl/List-Functions.html#List-Functions) in Common Lisp Extensions.

### Function: **butlast** *x \&optional n*

This function returns the list `x` with the last element, or the last `n` elements, removed. If `n` is greater than zero it makes a copy of the list so as not to damage the original list. In general, `(append (butlast x n) (last x n))` will return a list equal to `x`.

### Function: **nbutlast** *x \&optional n*

This is a version of `butlast` that works by destructively modifying the `cdr` of the appropriate element, rather than making a copy of the list.
