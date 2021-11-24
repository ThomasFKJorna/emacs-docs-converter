

#### 5.6.3 Functions that Rearrange Lists

Here are some functions that rearrange lists destructively by modifying the CDRs of their component cons cells. These functions are destructive because they chew up the original lists passed to them as arguments, relinking their cons cells to form a new list that is the returned value.

See `delq`, in [Sets And Lists](Sets-And-Lists.html), for another function that modifies cons cells.

### Function: **nconc** *\&rest lists*

This function returns a list containing all the elements of `lists`. Unlike `append` (see [Building Lists](Building-Lists.html)), the `lists` are *not* copied. Instead, the last CDR of each of the `lists` is changed to refer to the following list. The last of the `lists` is not altered. For example:

```lisp
(setq x (list 1 2 3))
     ⇒ (1 2 3)
```

```lisp
(nconc x '(4 5))
     ⇒ (1 2 3 4 5)
```

```lisp
x
     ⇒ (1 2 3 4 5)
```

Since the last argument of `nconc` is not itself modified, it is reasonable to use a constant list, such as `'(4 5)`, as in the above example. For the same reason, the last argument need not be a list:

```lisp
(setq x (list 1 2 3))
     ⇒ (1 2 3)
```

```lisp
(nconc x 'z)
     ⇒ (1 2 3 . z)
```

```lisp
x
     ⇒ (1 2 3 . z)
```

However, the other arguments (all but the last) should be mutable lists.

A common pitfall is to use a constant list as a non-last argument to `nconc`. If you do this, the resulting behavior is undefined. It is possible that your program will change each time you run it! Here is what might happen (though this is not guaranteed to happen):

```lisp
(defun add-foo (x)            ; We want this function to add
  (nconc '(foo) x))           ;   foo to the front of its arg.
```

```lisp
```

```lisp
(symbol-function 'add-foo)
     ⇒ (lambda (x) (nconc '(foo) x))
```

```lisp
```

```lisp
(setq xx (add-foo '(1 2)))    ; It seems to work.
     ⇒ (foo 1 2)
```

```lisp
(setq xy (add-foo '(3 4)))    ; What happened?
     ⇒ (foo 1 2 3 4)
```

```lisp
(eq xx xy)
     ⇒ t
```

```lisp
```

```lisp
(symbol-function 'add-foo)
     ⇒ (lambda (x) (nconc '(foo 1 2 3 4) x))
```
