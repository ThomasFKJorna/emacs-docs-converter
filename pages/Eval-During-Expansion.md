

Next: [Repeated Expansion](Repeated-Expansion.html), Previous: [Surprising Local Vars](Surprising-Local-Vars.html), Up: [Problems with Macros](Problems-with-Macros.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 14.5.4 Evaluating Macro Arguments in Expansion

Another problem can happen if the macro definition itself evaluates any of the macro argument expressions, such as by calling `eval` (see [Eval](Eval.html)). If the argument is supposed to refer to the user’s variables, you may have trouble if the user happens to use a variable with the same name as one of the macro arguments. Inside the macro body, the macro argument binding is the most local binding of this variable, so any references inside the form being evaluated do refer to it. Here is an example:

```lisp
(defmacro foo (a)
  (list 'setq (eval a) t))
```

```lisp
(setq x 'b)
(foo x) → (setq b t)
     ⇒ t                  ; and b has been set.
;; but
(setq a 'c)
(foo a) → (setq a t)
     ⇒ t                  ; but this set a, not c.
```

It makes a difference whether the user’s variable is named `a` or `x`, because `a` conflicts with the macro argument variable `a`.

Another problem with calling `eval` in a macro definition is that it probably won’t do what you intend in a compiled program. The byte compiler runs macro definitions while compiling the program, when the program’s own computations (which you might have wished to access with `eval`) don’t occur and its local variable bindings don’t exist.

To avoid these problems, **don’t evaluate an argument expression while computing the macro expansion**. Instead, substitute the expression into the macro expansion, so that its value will be computed as part of executing the expansion. This is how the other examples in this chapter work.

Next: [Repeated Expansion](Repeated-Expansion.html), Previous: [Surprising Local Vars](Surprising-Local-Vars.html), Up: [Problems with Macros](Problems-with-Macros.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
