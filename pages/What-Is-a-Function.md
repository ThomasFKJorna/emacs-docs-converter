

Next: [Lambda Expressions](Lambda-Expressions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 13.1 What Is a Function?

In a general sense, a function is a rule for carrying out a computation given input values called *arguments*. The result of the computation is called the *value* or *return value* of the function. The computation can also have side effects, such as lasting changes in the values of variables or the contents of data structures (see [Definition of side effect](Intro-Eval.html#Definition-of-side-effect)). A *pure function* is a function which, in addition to having no side effects, always returns the same value for the same combination of arguments, regardless of external factors such as machine type or system state.

In most computer languages, every function has a name. But in Lisp, a function in the strictest sense has no name: it is an object which can *optionally* be associated with a symbol (e.g., `car`) that serves as the function name. See [Function Names](Function-Names.html). When a function has been given a name, we usually also refer to that symbol as a “function” (e.g., we refer to “the function `car`”). In this manual, the distinction between a function name and the function object itself is usually unimportant, but we will take note wherever it is relevant.

Certain function-like objects, called *special forms* and *macros*, also accept arguments to carry out computations. However, as explained below, these are not considered functions in Emacs Lisp.

Here are important terms for functions and function-like objects:

*   *lambda expression*

    A function (in the strict sense, i.e., a function object) which is written in Lisp. These are described in the following section. See [Lambda Expressions](Lambda-Expressions.html).

*   *primitive*

    A function which is callable from Lisp but is actually written in C. Primitives are also called *built-in functions*, or *subrs*. Examples include functions like `car` and `append`. In addition, all special forms (see below) are also considered primitives.

    Usually, a function is implemented as a primitive because it is a fundamental part of Lisp (e.g., `car`), or because it provides a low-level interface to operating system services, or because it needs to run fast. Unlike functions defined in Lisp, primitives can be modified or added only by changing the C sources and recompiling Emacs. See [Writing Emacs Primitives](Writing-Emacs-Primitives.html).

*   *special form*

    A primitive that is like a function but does not evaluate all of its arguments in the usual way. It may evaluate only some of the arguments, or may evaluate them in an unusual order, or several times. Examples include `if`, `and`, and `while`. See [Special Forms](Special-Forms.html).

*   *macro*

    A construct defined in Lisp, which differs from a function in that it translates a Lisp expression into another expression which is to be evaluated instead of the original expression. Macros enable Lisp programmers to do the sorts of things that special forms can do. See [Macros](Macros.html).

*   *command*

    An object which can be invoked via the `command-execute` primitive, usually due to the user typing in a key sequence *bound* to that command. See [Interactive Call](Interactive-Call.html). A command is usually a function; if the function is written in Lisp, it is made into a command by an `interactive` form in the function definition (see [Defining Commands](Defining-Commands.html)). Commands that are functions can also be called from Lisp expressions, just like other functions.

    Keyboard macros (strings and vectors) are commands also, even though they are not functions. See [Keyboard Macros](Keyboard-Macros.html). We say that a symbol is a command if its function cell contains a command (see [Symbol Components](Symbol-Components.html)); such a *named command* can be invoked with `M-x`.

*   *closure*

    A function object that is much like a lambda expression, except that it also encloses an environment of lexical variable bindings. See [Closures](Closures.html).

*   *byte-code function*

    A function that has been compiled by the byte compiler. See [Byte-Code Type](Byte_002dCode-Type.html).

*   *autoload object*

    A place-holder for a real function. If the autoload object is called, Emacs loads the file containing the definition of the real function, and then calls the real function. See [Autoload](Autoload.html).

You can use the function `functionp` to test if an object is a function:

*   Function: **functionp** *object*

    This function returns `t` if `object` is any kind of function, i.e., can be passed to `funcall`. Note that `functionp` returns `t` for symbols that are function names, and returns `nil` for special forms.

It is also possible to find out how many arguments an arbitrary function expects:

*   Function: **func-arity** *function*

    This function provides information about the argument list of the specified `function`. The returned value is a cons cell of the form `(min . max)`, where `min` is the minimum number of arguments, and `max` is either the maximum number of arguments, or the symbol `many` for functions with `&rest` arguments, or the symbol `unevalled` if `function` is a special form.

    Note that this function might return inaccurate results in some situations, such as the following:

    *   \- Functions defined using `apply-partially` (see [apply-partially](Calling-Functions.html)).
    *   \- Functions that are advised using `advice-add` (see [Advising Named Functions](Advising-Named-Functions.html)).
    *   \- Functions that determine the argument list dynamically, as part of their code.

Unlike `functionp`, the next three functions do *not* treat a symbol as its function definition.

*   Function: **subrp** *object*

    This function returns `t` if `object` is a built-in function (i.e., a Lisp primitive).

    ```lisp
    (subrp 'message)            ; message is a symbol,
         ⇒ nil                 ;   not a subr object.
    ```

    ```lisp
    (subrp (symbol-function 'message))
         ⇒ t
    ```

<!---->

*   Function: **byte-code-function-p** *object*

    This function returns `t` if `object` is a byte-code function. For example:

    ```lisp
    (byte-code-function-p (symbol-function 'next-line))
         ⇒ t
    ```

<!---->

*   Function: **subr-arity** *subr*

    This works like `func-arity`, but only for built-in functions and without symbol indirection. It signals an error for non-built-in functions. We recommend to use `func-arity` instead.

Next: [Lambda Expressions](Lambda-Expressions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
