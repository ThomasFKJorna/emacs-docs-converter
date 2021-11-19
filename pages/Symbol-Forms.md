

Next: [Classifying Lists](Classifying-Lists.html), Previous: [Self-Evaluating Forms](Self_002dEvaluating-Forms.html), Up: [Forms](Forms.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 10.2.2 Symbol Forms

When a symbol is evaluated, it is treated as a variable. The result is the variable’s value, if it has one. If the symbol has no value as a variable, the Lisp interpreter signals an error. For more information on the use of variables, see [Variables](Variables.html).

In the following example, we set the value of a symbol with `setq`. Then we evaluate the symbol, and get back the value that `setq` stored.

```lisp
(setq a 123)
     ⇒ 123
```

```lisp
(eval 'a)
     ⇒ 123
```

```lisp
a
     ⇒ 123
```

The symbols `nil` and `t` are treated specially, so that the value of `nil` is always `nil`, and the value of `t` is always `t`; you cannot set or bind them to any other values. Thus, these two symbols act like self-evaluating forms, even though `eval` treats them like any other symbol. A symbol whose name starts with ‘`:`’ also self-evaluates in the same way; likewise, its value ordinarily cannot be changed. See [Constant Variables](Constant-Variables.html).
