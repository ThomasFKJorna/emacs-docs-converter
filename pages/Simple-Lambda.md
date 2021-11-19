

Next: [Argument List](Argument-List.html), Previous: [Lambda Components](Lambda-Components.html), Up: [Lambda Expressions](Lambda-Expressions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 13.2.2 A Simple Lambda Expression Example

Consider the following example:

```lisp
(lambda (a b c) (+ a b c))
```

We can call this function by passing it to `funcall`, like this:

```lisp
(funcall (lambda (a b c) (+ a b c))
         1 2 3)
```

This call evaluates the body of the lambda expression with the variable `a` bound to 1, `b` bound to 2, and `c` bound to 3. Evaluation of the body adds these three numbers, producing the result 6; therefore, this call to the function returns the value 6.

Note that the arguments can be the results of other function calls, as in this example:

```lisp
(funcall (lambda (a b c) (+ a b c))
         1 (* 2 3) (- 5 4))
```

This evaluates the arguments `1`, `(* 2 3)`, and `(- 5 4)` from left to right. Then it applies the lambda expression to the argument values 1, 6 and 1 to produce the value 8.

As these examples show, you can use a form with a lambda expression as its CAR to make local variables and give them values. In the old days of Lisp, this technique was the only way to bind and initialize local variables. But nowadays, it is clearer to use the special form `let` for this purpose (see [Local Variables](Local-Variables.html)). Lambda expressions are mainly used as anonymous functions for passing as arguments to other functions (see [Anonymous Functions](Anonymous-Functions.html)), or stored as symbol function definitions to produce named functions (see [Function Names](Function-Names.html)).
