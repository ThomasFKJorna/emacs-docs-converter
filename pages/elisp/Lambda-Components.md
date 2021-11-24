

#### 13.2.1 Components of a Lambda Expression

A lambda expression is a list that looks like this:

```lisp
(lambda (arg-variables…)
  [documentation-string]
  [interactive-declaration]
  body-forms…)
```

The first element of a lambda expression is always the symbol `lambda`. This indicates that the list represents a function. The reason functions are defined to start with `lambda` is so that other lists, intended for other uses, will not accidentally be valid as functions.

The second element is a list of symbols—the argument variable names (see [Argument List](Argument-List.html)). This is called the *lambda list*. When a Lisp function is called, the argument values are matched up against the variables in the lambda list, which are given local bindings with the values provided. See [Local Variables](Local-Variables.html).

The documentation string is a Lisp string object placed within the function definition to describe the function for the Emacs help facilities. See [Function Documentation](Function-Documentation.html).

The interactive declaration is a list of the form `(interactive code-string)`. This declares how to provide arguments if the function is used interactively. Functions with this declaration are called *commands*; they can be called using `M-x` or bound to a key. Functions not intended to be called in this way should not have interactive declarations. See [Defining Commands](Defining-Commands.html), for how to write an interactive declaration.

The rest of the elements are the *body* of the function: the Lisp code to do the work of the function (or, as a Lisp programmer would say, “a list of Lisp forms to evaluate”). The value returned by the function is the value returned by the last element of the body.
