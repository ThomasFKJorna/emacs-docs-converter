

### 14.2 Expansion of a Macro Call

A macro call looks just like a function call in that it is a list which starts with the name of the macro. The rest of the elements of the list are the arguments of the macro.

Evaluation of the macro call begins like evaluation of a function call except for one crucial difference: the macro arguments are the actual expressions appearing in the macro call. They are not evaluated before they are given to the macro definition. By contrast, the arguments of a function are results of evaluating the elements of the function call list.

Having obtained the arguments, Lisp invokes the macro definition just as a function is invoked. The argument variables of the macro are bound to the argument values from the macro call, or to a list of them in the case of a `&rest` argument. And the macro body executes and returns its value just as a function body does.

The second crucial difference between macros and functions is that the value returned by the macro body is an alternate Lisp expression, also known as the *expansion* of the macro. The Lisp interpreter proceeds to evaluate the expansion as soon as it comes back from the macro.

Since the expansion is evaluated in the normal manner, it may contain calls to other macros. It may even be a call to the same macro, though this is unusual.

Note that Emacs tries to expand macros when loading an uncompiled Lisp file. This is not always possible, but if it is, it speeds up subsequent execution. See [How Programs Do Loading](How-Programs-Do-Loading.html).

You can see the expansion of a given macro call by calling `macroexpand`.

### Function: **macroexpand** *form \&optional environment*

This function expands `form`, if it is a macro call. If the result is another macro call, it is expanded in turn, until something which is not a macro call results. That is the value returned by `macroexpand`. If `form` is not a macro call to begin with, it is returned as given.

Note that `macroexpand` does not look at the subexpressions of `form` (although some macro definitions may do so). Even if they are macro calls themselves, `macroexpand` does not expand them.

The function `macroexpand` does not expand calls to inline functions. Normally there is no need for that, since a call to an inline function is no harder to understand than a call to an ordinary function.

If `environment` is provided, it specifies an alist of macro definitions that shadow the currently defined macros. Byte compilation uses this feature.

```lisp
(defmacro inc (var)
    (list 'setq var (list '1+ var)))
```

```lisp
```

```lisp
(macroexpand '(inc r))
     ⇒ (setq r (1+ r))
```

```lisp
```

```lisp
(defmacro inc2 (var1 var2)
    (list 'progn (list 'inc var1) (list 'inc var2)))
```

```lisp
```

```lisp
(macroexpand '(inc2 r s))
     ⇒ (progn (inc r) (inc s))  ; inc not expanded here.
```

### Function: **macroexpand-all** *form \&optional environment*

`macroexpand-all` expands macros like `macroexpand`, but will look for and expand all macros in `form`, not just at the top-level. If no macros are expanded, the return value is `eq` to `form`.

Repeating the example used for `macroexpand` above with `macroexpand-all`, we see that `macroexpand-all` *does* expand the embedded calls to `inc`:

```lisp
(macroexpand-all '(inc2 r s))
     ⇒ (progn (setq r (1+ r)) (setq s (1+ s)))
```

### Function: **macroexpand-1** *form \&optional environment*

This function expands macros like `macroexpand`, but it only performs one step of the expansion: if the result is another macro call, `macroexpand-1` will not expand it.
