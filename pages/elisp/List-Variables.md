

### 5.5 Modifying List Variables

These functions, and one macro, provide convenient ways to modify a list which is stored in a variable.

### Macro: **push** *element listname*

This macro creates a new list whose CAR is `element` and whose CDR is the list specified by `listname`, and saves that list in `listname`. In the simplest case, `listname` is an unquoted symbol naming a list, and this macro is equivalent to `(setq listname (cons element listname))`.

```lisp
(setq l '(a b))
     ⇒ (a b)
(push 'c l)
     ⇒ (c a b)
l
     ⇒ (c a b)
```

More generally, `listname` can be a generalized variable. In that case, this macro does the equivalent of `(setf listname (cons element listname))`. See [Generalized Variables](Generalized-Variables.html).

For the `pop` macro, which removes the first element from a list, See [List Elements](List-Elements.html).

Two functions modify lists that are the values of variables.

### Function: **add-to-list** *symbol element \&optional append compare-fn*

This function sets the variable `symbol` by consing `element` onto the old value, if `element` is not already a member of that value. It returns the resulting list, whether updated or not. The value of `symbol` had better be a list already before the call. `add-to-list` uses `compare-fn` to compare `element` against existing list members; if `compare-fn` is `nil`, it uses `equal`.

Normally, if `element` is added, it is added to the front of `symbol`, but if the optional argument `append` is non-`nil`, it is added at the end.

The argument `symbol` is not implicitly quoted; `add-to-list` is an ordinary function, like `set` and unlike `setq`. Quote the argument yourself if that is what you want.

Do not use this function when `symbol` refers to a lexical variable.

Here’s a scenario showing how to use `add-to-list`:

```lisp
(setq foo '(a b))
     ⇒ (a b)

(add-to-list 'foo 'c)     ;; Add c.
     ⇒ (c a b)

(add-to-list 'foo 'b)     ;; No effect.
     ⇒ (c a b)

foo                       ;; foo was changed.
     ⇒ (c a b)
```

An equivalent expression for `(add-to-list 'var value)` is this:

```lisp
(if (member value var)
    var
  (setq var (cons value var)))
```

### Function: **add-to-ordered-list** *symbol element \&optional order*

This function sets the variable `symbol` by inserting `element` into the old value, which must be a list, at the position specified by `order`. If `element` is already a member of the list, its position in the list is adjusted according to `order`. Membership is tested using `eq`. This function returns the resulting list, whether updated or not.

The `order` is typically a number (integer or float), and the elements of the list are sorted in non-decreasing numerical order.

`order` may also be omitted or `nil`. Then the numeric order of `element` stays unchanged if it already has one; otherwise, `element` has no numeric order. Elements without a numeric list order are placed at the end of the list, in no particular order.

Any other value for `order` removes the numeric order of `element` if it already has one; otherwise, it is equivalent to `nil`.

The argument `symbol` is not implicitly quoted; `add-to-ordered-list` is an ordinary function, like `set` and unlike `setq`. Quote the argument yourself if necessary.

The ordering information is stored in a hash table on `symbol`’s `list-order` property. `symbol` cannot refer to a lexical variable.

Here’s a scenario showing how to use `add-to-ordered-list`:

```lisp
(setq foo '())
     ⇒ nil

(add-to-ordered-list 'foo 'a 1)     ;; Add a.
     ⇒ (a)

(add-to-ordered-list 'foo 'c 3)     ;; Add c.
     ⇒ (a c)

(add-to-ordered-list 'foo 'b 2)     ;; Add b.
     ⇒ (a b c)

(add-to-ordered-list 'foo 'b 4)     ;; Move b.
     ⇒ (a c b)

(add-to-ordered-list 'foo 'd)       ;; Append d.
     ⇒ (a c b d)

(add-to-ordered-list 'foo 'e)       ;; Add e.
     ⇒ (a c b e d)

foo                       ;; foo was changed.
     ⇒ (a c b e d)
```
