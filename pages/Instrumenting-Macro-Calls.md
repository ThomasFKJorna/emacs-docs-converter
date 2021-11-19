

Next: [Specification List](Specification-List.html), Up: [Edebug and Macros](Edebug-and-Macros.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.2.15.1 Instrumenting Macro Calls

When Edebug instruments an expression that calls a Lisp macro, it needs additional information about the macro to do the job properly. This is because there is no a-priori way to tell which subexpressions of the macro call are forms to be evaluated. (Evaluation may occur explicitly in the macro body, or when the resulting expansion is evaluated, or any time later.)

Therefore, you must define an Edebug specification for each macro that Edebug will encounter, to explain the format of calls to that macro. To do this, add a `debug` declaration to the macro definition. Here is a simple example that shows the specification for the `for` example macro (see [Argument Evaluation](Argument-Evaluation.html)).

```lisp
(defmacro for (var from init to final do &rest body)
  "Execute a simple \"for\" loop.
For example, (for i from 1 to 10 do (print i))."
  (declare (debug (symbolp "from" form "to" form "do" &rest form)))
  ...)
```

The Edebug specification says which parts of a call to the macro are forms to be evaluated. For simple macros, the specification often looks very similar to the formal argument list of the macro definition, but specifications are much more general than macro arguments. See [Defining Macros](Defining-Macros.html), for more explanation of the `declare` form.

Take care to ensure that the specifications are known to Edebug when you instrument code. If you are instrumenting a function which uses a macro defined in another file, you may first need to either evaluate the `require` forms in the file containing your function, or explicitly load the file containing the macro. If the definition of a macro is wrapped by `eval-when-compile`, you may need to evaluate it.

You can also define an edebug specification for a macro separately from the macro definition with `def-edebug-spec`. Adding `debug` declarations is preferred, and more convenient, for macro definitions in Lisp, but `def-edebug-spec` makes it possible to define Edebug specifications for special forms implemented in C.

*   Macro: **def-edebug-spec** *macro specification*

    Specify which expressions of a call to macro `macro` are forms to be evaluated. `specification` should be the edebug specification. Neither argument is evaluated.

    The `macro` argument can actually be any symbol, not just a macro name.

Here is a table of the possibilities for `specification` and how each directs processing of arguments.

*   `t`

    All arguments are instrumented for evaluation.

*   `0`

    None of the arguments is instrumented.

*   a symbol

    The symbol must have an Edebug specification, which is used instead. This indirection is repeated until another kind of specification is found. This allows you to inherit the specification from another macro.

*   a list

    The elements of the list describe the types of the arguments of a calling form. The possible elements of a specification list are described in the following sections.

If a macro has no Edebug specification, neither through a `debug` declaration nor through a `def-edebug-spec` call, the variable `edebug-eval-macro-args` comes into play.

*   User Option: **edebug-eval-macro-args**

    This controls the way Edebug treats macro arguments with no explicit Edebug specification. If it is `nil` (the default), none of the arguments is instrumented for evaluation. Otherwise, all arguments are instrumented.

Next: [Specification List](Specification-List.html), Up: [Edebug and Macros](Edebug-and-Macros.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
