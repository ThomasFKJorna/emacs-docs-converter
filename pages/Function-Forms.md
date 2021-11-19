

Next: [Macro Forms](Macro-Forms.html), Previous: [Function Indirection](Function-Indirection.html), Up: [Forms](Forms.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 10.2.5 Evaluation of Function Forms

If the first element of a list being evaluated is a Lisp function object, byte-code object or primitive function object, then that list is a *function call*. For example, here is a call to the function `+`:

```lisp
(+ 1 x)
```

The first step in evaluating a function call is to evaluate the remaining elements of the list from left to right. The results are the actual argument values, one value for each list element. The next step is to call the function with this list of arguments, effectively using the function `apply` (see [Calling Functions](Calling-Functions.html)). If the function is written in Lisp, the arguments are used to bind the argument variables of the function (see [Lambda Expressions](Lambda-Expressions.html)); then the forms in the function body are evaluated in order, and the value of the last body form becomes the value of the function call.
