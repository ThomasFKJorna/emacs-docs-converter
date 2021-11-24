

### 10.3 Quoting

The special form `quote` returns its single argument, as written, without evaluating it. This provides a way to include constant symbols and lists, which are not self-evaluating objects, in a program. (It is not necessary to quote self-evaluating objects such as numbers, strings, and vectors.)

### Special Form: **quote** *object*

This special form returns `object`, without evaluating it. The returned value might be shared and should not be modified. See [Self-Evaluating Forms](Self_002dEvaluating-Forms.html).

Because `quote` is used so often in programs, Lisp provides a convenient read syntax for it. An apostrophe character (‘`'`’) followed by a Lisp object (in read syntax) expands to a list whose first element is `quote`, and whose second element is the object. Thus, the read syntax `'x` is an abbreviation for `(quote x)`.

Here are some examples of expressions that use `quote`:

```lisp
(quote (+ 1 2))
     ⇒ (+ 1 2)
```

```lisp
(quote foo)
     ⇒ foo
```

```lisp
'foo
     ⇒ foo
```

```lisp
''foo
     ⇒ 'foo
```

```lisp
'(quote foo)
     ⇒ 'foo
```

```lisp
['foo]
     ⇒ ['foo]
```

Although the expressions `(list '+ 1 2)` and `'(+ 1 2)` both yield lists equal to `(+ 1 2)`, the former yields a freshly-minted mutable list whereas the latter yields a list built from conses that might be shared and should not be modified. See [Self-Evaluating Forms](Self_002dEvaluating-Forms.html).

Other quoting constructs include `function` (see [Anonymous Functions](Anonymous-Functions.html)), which causes an anonymous lambda expression written in Lisp to be compiled, and ‘`` ` ``’ (see [Backquote](Backquote.html)), which is used to quote only part of a list, while computing and substituting other parts.
