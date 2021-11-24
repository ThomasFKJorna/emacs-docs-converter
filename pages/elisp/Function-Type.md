

#### 2.4.13 Function Type

Lisp functions are executable code, just like functions in other programming languages. In Lisp, unlike most languages, functions are also Lisp objects. A non-compiled function in Lisp is a lambda expression: that is, a list whose first element is the symbol `lambda` (see [Lambda Expressions](Lambda-Expressions.html)).

In most programming languages, it is impossible to have a function without a name. In Lisp, a function has no intrinsic name. A lambda expression can be called as a function even though it has no name; to emphasize this, we also call it an *anonymous function* (see [Anonymous Functions](Anonymous-Functions.html)). A named function in Lisp is just a symbol with a valid function in its function cell (see [Defining Functions](Defining-Functions.html)).

Most of the time, functions are called when their names are written in Lisp expressions in Lisp programs. However, you can construct or obtain a function object at run time and then call it with the primitive functions `funcall` and `apply`. See [Calling Functions](Calling-Functions.html).
