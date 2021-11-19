

Next: [Obsolete Functions](Obsolete-Functions.html), Previous: [Closures](Closures.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 13.11 Advising Emacs Lisp Functions

When you need to modify a function defined in another library, or when you need to modify a hook like `foo-function`, a process filter, or basically any variable or object field which holds a function value, you can use the appropriate setter function, such as `fset` or `defun` for named functions, `setq` for hook variables, or `set-process-filter` for process filters, but those are often too blunt, completely throwing away the previous value.

The *advice* feature lets you add to the existing definition of a function, by *advising the function*. This is a cleaner method than redefining the whole function.

Emacs’s advice system provides two sets of primitives for that: the core set, for function values held in variables and object fields (with the corresponding primitives being `add-function` and `remove-function`) and another set layered on top of it for named functions (with the main primitives being `advice-add` and `advice-remove`).

As a trivial example, here’s how to add advice that’ll modify the return value of a function every time it’s called:

```lisp
(defun my-double (x)
  (* x 2))
(defun my-increase (x)
  (+ x 1))
(advice-add 'my-double :filter-return #'my-increase)
```

After adding this advice, if you call `my-double` with ‘`3`’, the return value will be ‘`7`’. To remove this advice, say

```lisp
(advice-remove 'my-double #'my-increase)
```

A more advanced example would be to trace the calls to the process filter of a process `proc`:

```lisp
(defun my-tracing-function (proc string)
  (message "Proc %S received %S" proc string))

(add-function :before (process-filter proc) #'my-tracing-function)
```

This will cause the process’s output to be passed to `my-tracing-function` before being passed to the original process filter. `my-tracing-function` receives the same arguments as the original function. When you’re done with it, you can revert to the untraced behavior with:

```lisp
(remove-function (process-filter proc) #'my-tracing-function)
```

Similarly, if you want to trace the execution of the function named `display-buffer`, you could use:

```lisp
(defun his-tracing-function (orig-fun &rest args)
  (message "display-buffer called with args %S" args)
  (let ((res (apply orig-fun args)))
    (message "display-buffer returned %S" res)
    res))

(advice-add 'display-buffer :around #'his-tracing-function)
```

Here, `his-tracing-function` is called instead of the original function and receives the original function (additionally to that function’s arguments) as argument, so it can call it if and when it needs to. When you’re tired of seeing this output, you can revert to the untraced behavior with:

```lisp
(advice-remove 'display-buffer #'his-tracing-function)
```

The arguments `:before` and `:around` used in the above examples specify how the two functions are composed, since there are many different ways to do it. The added function is also called a piece of *advice*.

|                                                             |    |                                        |
| :---------------------------------------------------------- | -- | :------------------------------------- |
| • [Core Advising Primitives](Core-Advising-Primitives.html) |    | Primitives to manipulate advice.       |
| • [Advising Named Functions](Advising-Named-Functions.html) |    | Advising named functions.              |
| • [Advice Combinators](Advice-Combinators.html)             |    | Ways to compose advice.                |
| • [Porting Old Advice](Porting-Old-Advice.html)             |    | Adapting code using the old defadvice. |

Next: [Obsolete Functions](Obsolete-Functions.html), Previous: [Closures](Closures.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
