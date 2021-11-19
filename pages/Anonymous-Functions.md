

Next: [Generic Functions](Generic-Functions.html), Previous: [Mapping Functions](Mapping-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 13.7 Anonymous Functions

Although functions are usually defined with `defun` and given names at the same time, it is sometimes convenient to use an explicit lambda expression—an *anonymous function*. Anonymous functions are valid wherever function names are. They are often assigned as variable values, or as arguments to functions; for instance, you might pass one as the `function` argument to `mapcar`, which applies that function to each element of a list (see [Mapping Functions](Mapping-Functions.html)). See [describe-symbols example](Accessing-Documentation.html#describe_002dsymbols-example), for a realistic example of this.

When defining a lambda expression that is to be used as an anonymous function, you can in principle use any method to construct the list. But typically you should use the `lambda` macro, or the `function` special form, or the `#'` read syntax:

*   Macro: **lambda** *args \[doc] \[interactive] body…*

    This macro returns an anonymous function with argument list `args`, documentation string `doc` (if any), interactive spec `interactive` (if any), and body forms given by `body`.

    Under dynamic binding, this macro effectively makes `lambda` forms self-quoting: evaluating a form whose CAR is `lambda` yields the form itself:

    ```lisp
    (lambda (x) (* x x))
         ⇒ (lambda (x) (* x x))
    ```

    Note that when evaluating under lexical binding the result is a closure object (see [Closures](Closures.html)).

    The `lambda` form has one other effect: it tells the Emacs evaluator and byte-compiler that its argument is a function, by using `function` as a subroutine (see below).

<!---->

*   Special Form: **function** *function-object*

    This special form returns `function-object` without evaluating it. In this, it is similar to `quote` (see [Quoting](Quoting.html)). But unlike `quote`, it also serves as a note to the Emacs evaluator and byte-compiler that `function-object` is intended to be used as a function. Assuming `function-object` is a valid lambda expression, this has two effects:

    *   When the code is byte-compiled, `function-object` is compiled into a byte-code function object (see [Byte Compilation](Byte-Compilation.html)).
    *   When lexical binding is enabled, `function-object` is converted into a closure. See [Closures](Closures.html).

    When `function-object` is a symbol and the code is byte compiled, the byte-compiler will warn if that function is not defined or might not be known at run time.

The read syntax `#'` is a short-hand for using `function`. The following forms are all equivalent:

```lisp
(lambda (x) (* x x))
(function (lambda (x) (* x x)))
#'(lambda (x) (* x x))
```

In the following example, we define a `change-property` function that takes a function as its third argument, followed by a `double-property` function that makes use of `change-property` by passing it an anonymous function:

```lisp
(defun change-property (symbol prop function)
  (let ((value (get symbol prop)))
    (put symbol prop (funcall function value))))
```

```lisp
```

```lisp
(defun double-property (symbol prop)
  (change-property symbol prop (lambda (x) (* 2 x))))
```

Note that we do not quote the `lambda` form.

If you compile the above code, the anonymous function is also compiled. This would not happen if, say, you had constructed the anonymous function by quoting it as a list:

```lisp
(defun double-property (symbol prop)
  (change-property symbol prop '(lambda (x) (* 2 x))))
```

In that case, the anonymous function is kept as a lambda expression in the compiled code. The byte-compiler cannot assume this list is a function, even though it looks like one, since it does not know that `change-property` intends to use it as a function.

Next: [Generic Functions](Generic-Functions.html), Previous: [Mapping Functions](Mapping-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
