

### 13.3 Naming a Function

A symbol can serve as the name of a function. This happens when the symbol’s *function cell* (see [Symbol Components](Symbol-Components.html)) contains a function object (e.g., a lambda expression). Then the symbol itself becomes a valid, callable function, equivalent to the function object in its function cell.

The contents of the function cell are also called the symbol’s *function definition*. The procedure of using a symbol’s function definition in place of the symbol is called *symbol function indirection*; see [Function Indirection](Function-Indirection.html). If you have not given a symbol a function definition, its function cell is said to be *void*, and it cannot be used as a function.

In practice, nearly all functions have names, and are referred to by their names. You can create a named Lisp function by defining a lambda expression and putting it in a function cell (see [Function Cells](Function-Cells.html)). However, it is more common to use the `defun` special form, described in the next section. See [Defining Functions](Defining-Functions.html).

We give functions names because it is convenient to refer to them by their names in Lisp expressions. Also, a named Lisp function can easily refer to itself—it can be recursive. Furthermore, primitives can only be referred to textually by their names, since primitive function objects (see [Primitive Function Type](Primitive-Function-Type.html)) have no read syntax.

A function need not have a unique name. A given function object *usually* appears in the function cell of only one symbol, but this is just a convention. It is easy to store it in several symbols using `fset`; then each of the symbols is a valid name for the same function.

Note that a symbol used as a function name may also be used as a variable; these two uses of a symbol are independent and do not conflict. (This is not the case in some dialects of Lisp, like Scheme.)

By convention, if a function’s symbol consists of two names separated by ‘`--`’, the function is intended for internal use and the first part names the file defining the function. For example, a function named `vc-git--rev-parse` is an internal function defined in `vc-git.el`. Internal-use functions written in C have names ending in ‘`-internal`’, e.g., `bury-buffer-internal`. Emacs code contributed before 2018 may follow other internal-use naming conventions, which are being phased out.
