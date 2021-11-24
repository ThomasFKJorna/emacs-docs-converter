

### 11.3 Constructs for Combining Conditions

This section describes constructs that are often used together with `if` and `cond` to express complicated conditions. The constructs `and` and `or` can also be used individually as kinds of multiple conditional constructs.

### Function: **not** *condition*

This function tests for the falsehood of `condition`. It returns `t` if `condition` is `nil`, and `nil` otherwise. The function `not` is identical to `null`, and we recommend using the name `null` if you are testing for an empty list.

### Special Form: **and** *conditions…*

The `and` special form tests whether all the `conditions` are true. It works by evaluating the `conditions` one by one in the order written.

If any of the `conditions` evaluates to `nil`, then the result of the `and` must be `nil` regardless of the remaining `conditions`; so `and` returns `nil` right away, ignoring the remaining `conditions`.

If all the `conditions` turn out non-`nil`, then the value of the last of them becomes the value of the `and` form. Just `(and)`, with no `conditions`, returns `t`, appropriate because all the `conditions` turned out non-`nil`. (Think about it; which one did not?)

Here is an example. The first condition returns the integer 1, which is not `nil`. Similarly, the second condition returns the integer 2, which is not `nil`. The third condition is `nil`, so the remaining condition is never evaluated.

```lisp
(and (print 1) (print 2) nil (print 3))
     -| 1
     -| 2
⇒ nil
```

Here is a more realistic example of using `and`:

```lisp
(if (and (consp foo) (eq (car foo) 'x))
    (message "foo is a list starting with x"))
```

Note that `(car foo)` is not executed if `(consp foo)` returns `nil`, thus avoiding an error.

`and` expressions can also be written using either `if` or `cond`. Here’s how:

```lisp
(and arg1 arg2 arg3)
≡
(if arg1 (if arg2 arg3))
≡
(cond (arg1 (cond (arg2 arg3))))
```

### Special Form: **or** *conditions…*

The `or` special form tests whether at least one of the `conditions` is true. It works by evaluating all the `conditions` one by one in the order written.

If any of the `conditions` evaluates to a non-`nil` value, then the result of the `or` must be non-`nil`; so `or` returns right away, ignoring the remaining `conditions`. The value it returns is the non-`nil` value of the condition just evaluated.

If all the `conditions` turn out `nil`, then the `or` expression returns `nil`. Just `(or)`, with no `conditions`, returns `nil`, appropriate because all the `conditions` turned out `nil`. (Think about it; which one did not?)

For example, this expression tests whether `x` is either `nil` or the integer zero:

```lisp
(or (eq x nil) (eq x 0))
```

Like the `and` construct, `or` can be written in terms of `cond`. For example:

```lisp
(or arg1 arg2 arg3)
≡
(cond (arg1)
      (arg2)
      (arg3))
```

You could almost write `or` in terms of `if`, but not quite:

```lisp
(if arg1 arg1
  (if arg2 arg2
    arg3))
```

This is not completely equivalent because it can evaluate `arg1` or `arg2` twice. By contrast, `(or arg1 arg2 arg3)` never evaluates any argument more than once.

### Function: **xor** *condition1 condition2*

This function returns the boolean exclusive-or of `condition1` and `condition2`. That is, `xor` returns `nil` if either both arguments are `nil`, or both are non-`nil`. Otherwise, it returns the value of that argument which is non-`nil`.

Note that in contrast to `or`, both arguments are always evaluated.
