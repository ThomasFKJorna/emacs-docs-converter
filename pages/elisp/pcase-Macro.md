

#### 11.4.1 The `pcase` macro

For background, See [Pattern-Matching Conditional](Pattern_002dMatching-Conditional.html).

### Macro: **pcase** *expression \&rest clauses*

Each clause in `clauses` has the form: `(pattern body-forms…)`.

Evaluate `expression` to determine its value, `expval`. Find the first clause in `clauses` whose `pattern` matches `expval` and pass control to that clause’s `body-forms`.

If there is a match, the value of `pcase` is the value of the last of `body-forms` in the successful clause. Otherwise, `pcase` evaluates to `nil`.

Each `pattern` has to be a *pcase pattern*, which can use either one of the core patterns defined below, or one of the patterns defined via `pcase-defmacro` (see [Extending pcase](Extending-pcase.html)).

The rest of this subsection describes different forms of core patterns, presents some examples, and concludes with important caveats on using the let-binding facility provided by some pattern forms. A core pattern can have the following forms:

`_`

Matches any `expval`. This is also known as *don’t care* or *wildcard*.

`'val`

Matches if `expval` equals `val`. The comparison is done as if by `equal` (see [Equality Predicates](Equality-Predicates.html)).

*   `keyword`
*   `integer`
*   `string`

Matches if `expval` equals the literal object. This is a special case of `'val`, above, possible because literal objects of these types are self-quoting.

`symbol`

Matches any `expval`, and additionally let-binds `symbol` to `expval`, such that this binding is available to `body-forms` (see [Dynamic Binding](Dynamic-Binding.html)).

If `symbol` is part of a sequencing pattern `seqpat` (e.g., by using `and`, below), the binding is also available to the portion of `seqpat` following the appearance of `symbol`. This usage has some caveats, see [caveats](#pcase_002dsymbol_002dcaveats).

Two symbols to avoid are `t`, which behaves like `_` (above) and is deprecated, and `nil`, which signals an error. Likewise, it makes no sense to bind keyword symbols (see [Constant Variables](Constant-Variables.html)).

`(pred function)`

Matches if the predicate `function` returns non-`nil` when called on `expval`. the predicate `function` can have one of the following forms:

*   function name (a symbol)

    Call the named function with one argument, `expval`.

    Example: `integerp`

*   lambda expression

    Call the anonymous function with one argument, `expval` (see [Lambda Expressions](Lambda-Expressions.html)).

    Example: `(lambda (n) (= 42 n))`

*   function call with `n` args

    Call the function (the first element of the function call) with `n` arguments (the other elements) and an additional `n`+1-th argument that is `expval`.

    Example: `(= 42)`\
    In this example, the function is `=`, `n` is one, and the actual function call becomes: `(= 42 expval)`.

`(app function pattern)`

Matches if `function` called on `expval` returns a value that matches `pattern`. `function` can take one of the forms described for `pred`, above. Unlike `pred`, however, `app` tests the result against `pattern`, rather than against a boolean truth value.

`(guard boolean-expression)`

Matches if `boolean-expression` evaluates to non-`nil`.

`(let pattern expr)`

Evaluates `expr` to get `exprval` and matches if `exprval` matches `pattern`. (It is called `let` because `pattern` can bind symbols to values using `symbol`.)

A *sequencing pattern* (also known as `seqpat`) is a pattern that processes its sub-pattern arguments in sequence. There are two for `pcase`: `and` and `or`. They behave in a similar manner to the special forms that share their name (see [Combining Conditions](Combining-Conditions.html)), but instead of processing values, they process sub-patterns.

`(and pattern1…)`

Attempts to match `pattern1`…, in order, until one of them fails to match. In that case, `and` likewise fails to match, and the rest of the sub-patterns are not tested. If all sub-patterns match, `and` matches.

`(or pattern1 pattern2…)`

Attempts to match `pattern1`, `pattern2`, …, in order, until one of them succeeds. In that case, `or` likewise matches, and the rest of the sub-patterns are not tested. (Note that there must be at least two sub-patterns. Simply `(or pattern1)` signals error.)

To present a consistent environment (see [Intro Eval](Intro-Eval.html)) to `body-forms` (thus avoiding an evaluation error on match), if any of the sub-patterns let-binds a set of symbols, they *must* all bind the same set of symbols.

`(rx rx-expr…)`

Matches strings against the regexp `rx-expr`…, using the `rx` regexp notation (see [Rx Notation](Rx-Notation.html)), as if by `string-match`.

In addition to the usual `rx` syntax, `rx-expr`… can contain the following constructs:

*   `(let ref rx-expr…)`

    Bind the symbol `ref` to a submatch that matches `rx-expr`.... `ref` is bound in `body-forms` to the string of the submatch or nil, but can also be used in `backref`.

*   `(backref ref)`

    Like the standard `backref` construct, but `ref` can here also be a name introduced by a previous `(let ref …)` construct.

#### Example: Advantage Over `cl-case`

Here’s an example that highlights some advantages `pcase` has over `cl-case` (see [Conditionals](https://www.gnu.org/software/emacs/manual/html_node/cl/Conditionals.html#Conditionals) in Common Lisp Extensions).

```lisp
(pcase (get-return-code x)
  ;; string
  ((and (pred stringp) msg)
   (message "%s" msg))
```

```lisp
  ;; symbol
  ('success       (message "Done!"))
  ('would-block   (message "Sorry, can't do it now"))
  ('read-only     (message "The shmliblick is read-only"))
  ('access-denied (message "You do not have the needed rights"))
```

```lisp
  ;; default
  (code           (message "Unknown return code %S" code)))
```

With `cl-case`, you would need to explicitly declare a local variable `code` to hold the return value of `get-return-code`. Also `cl-case` is difficult to use with strings because it uses `eql` for comparison.

#### Example: Using `and`

A common idiom is to write a pattern starting with `and`, with one or more `symbol` sub-patterns providing bindings to the sub-patterns that follow (as well as to the body forms). For example, the following pattern matches single-digit integers.

```lisp
(and
  (pred integerp)
  n                     ; bind n to expval
  (guard (<= -9 n 9)))
```

First, `pred` matches if `(integerp expval)` evaluates to non-`nil`. Next, `n` is a `symbol` pattern that matches anything and binds `n` to `expval`. Lastly, `guard` matches if the boolean expression `(<= -9 n 9)` (note the reference to `n`) evaluates to non-`nil`. If all these sub-patterns match, `and` matches.

#### Example: Reformulation with `pcase`

Here is another example that shows how to reformulate a simple matching task from its traditional implementation (function `grok/traditional`) to one using `pcase` (function `grok/pcase`). The docstring for both these functions is: “If OBJ is a string of the form "key:NUMBER", return NUMBER (a string). Otherwise, return the list ("149" default).” First, the traditional implementation (see [Regular Expressions](Regular-Expressions.html)):

```lisp
(defun grok/traditional (obj)
  (if (and (stringp obj)
           (string-match "^key:\\([[:digit:]]+\\)$" obj))
      (match-string 1 obj)
    (list "149" 'default)))
```

```lisp
```

```lisp
(grok/traditional "key:0")   ⇒ "0"
(grok/traditional "key:149") ⇒ "149"
(grok/traditional 'monolith) ⇒ ("149" default)
```

The reformulation demonstrates `symbol` binding as well as `or`, `and`, `pred`, `app` and `let`.

```lisp
(defun grok/pcase (obj)
  (pcase obj
    ((or                                     ; line 1
      (and                                   ; line 2
       (pred stringp)                        ; line 3
       (pred (string-match                   ; line 4
              "^key:\\([[:digit:]]+\\)$"))   ; line 5
       (app (match-string 1)                 ; line 6
            val))                            ; line 7
      (let val (list "149" 'default)))       ; line 8
     val)))                                  ; line 9
```

```lisp
```

```lisp
(grok/pcase "key:0")   ⇒ "0"
(grok/pcase "key:149") ⇒ "149"
(grok/pcase 'monolith) ⇒ ("149" default)
```

The bulk of `grok/pcase` is a single clause of a `pcase` form, the pattern on lines 1-8, the (single) body form on line 9. The pattern is `or`, which tries to match in turn its argument sub-patterns, first `and` (lines 2-7), then `let` (line 8), until one of them succeeds.

As in the previous example (see [Example 1](#pcase_002dexample_002d1)), `and` begins with a `pred` sub-pattern to ensure the following sub-patterns work with an object of the correct type (string, in this case). If `(stringp expval)` returns `nil`, `pred` fails, and thus `and` fails, too.

The next `pred` (lines 4-5) evaluates `(string-match RX expval)` and matches if the result is non-`nil`, which means that `expval` has the desired form: `key:NUMBER`. Again, failing this, `pred` fails and `and`, too.

Lastly (in this series of `and` sub-patterns), `app` evaluates `(match-string 1 expval)` (line 6) to get a temporary value `tmp` (i.e., the “NUMBER” substring) and tries to match `tmp` against pattern `val` (line 7). Since that is a `symbol` pattern, it matches unconditionally and additionally binds `val` to `tmp`.

Now that `app` has matched, all `and` sub-patterns have matched, and so `and` matches. Likewise, once `and` has matched, `or` matches and does not proceed to try sub-pattern `let` (line 8).

Let’s consider the situation where `obj` is not a string, or it is a string but has the wrong form. In this case, one of the `pred` (lines 3-5) fails to match, thus `and` (line 2) fails to match, thus `or` (line 1) proceeds to try sub-pattern `let` (line 8).

First, `let` evaluates `(list "149" 'default)` to get `("149" default)`, the `exprval`, and then tries to match `exprval` against pattern `val`. Since that is a `symbol` pattern, it matches unconditionally and additionally binds `val` to `exprval`. Now that `let` has matched, `or` matches.

Note how both `and` and `let` sub-patterns finish in the same way: by trying (always successfully) to match against the `symbol` pattern `val`, in the process binding `val`. Thus, `or` always matches and control always passes to the body form (line 9). Because that is the last body form in a successfully matched `pcase` clause, it is the value of `pcase` and likewise the return value of `grok/pcase` (see [What Is a Function](What-Is-a-Function.html)).

#### Caveats for `symbol` in Sequencing Patterns

The preceding examples all use sequencing patterns which include the `symbol` sub-pattern in some way. Here are some important details about that usage.

1.  When `symbol` occurs more than once in `seqpat`, the second and subsequent occurrences do not expand to re-binding, but instead expand to an equality test using `eq`.

    The following example features a `pcase` form with two clauses and two `seqpat`, A and B. Both A and B first check that `expval` is a pair (using `pred`), and then bind symbols to the `car` and `cdr` of `expval` (using one `app` each).

    For A, because symbol `st` is mentioned twice, the second mention becomes an equality test using `eq`. On the other hand, B uses two separate symbols, `s1` and `s2`, both of which become independent bindings.

    ```lisp
    (defun grok (object)
      (pcase object
        ((and (pred consp)        ; seqpat A
              (app car st)        ; first mention: st
              (app cdr st))       ; second mention: st
         (list 'eq st))
    ```

    ```lisp
        ((and (pred consp)        ; seqpat B
              (app car s1)        ; first mention: s1
              (app cdr s2))       ; first mention: s2
         (list 'not-eq s1 s2))))
    ```

    ```lisp
    ```

    ```lisp
    (let ((s "yow!"))
      (grok (cons s s)))      ⇒ (eq "yow!")
    (grok (cons "yo!" "yo!")) ⇒ (not-eq "yo!" "yo!")
    (grok '(4 2))             ⇒ (not-eq 4 (2))
    ```

2.  Side-effecting code referencing `symbol` is undefined. Avoid. For example, here are two similar functions. Both use `and`, `symbol` and `guard`:

    ```lisp
    (defun square-double-digit-p/CLEAN (integer)
      (pcase (* integer integer)
        ((and n (guard (< 9 n 100))) (list 'yes n))
        (sorry (list 'no sorry))))

    (square-double-digit-p/CLEAN 9) ⇒ (yes 81)
    (square-double-digit-p/CLEAN 3) ⇒ (no 9)
    ```

    ```lisp
    ```

    ```lisp
    (defun square-double-digit-p/MAYBE (integer)
      (pcase (* integer integer)
        ((and n (guard (< 9 (incf n) 100))) (list 'yes n))
        (sorry (list 'no sorry))))

    (square-double-digit-p/MAYBE 9) ⇒ (yes 81)
    (square-double-digit-p/MAYBE 3) ⇒ (yes 9)  ; WRONG!
    ```

    The difference is in `boolean-expression` in `guard`: `CLEAN` references `n` simply and directly, while `MAYBE` references `n` with a side-effect, in the expression `(incf n)`. When `integer` is 3, here’s what happens:

    *   The first `n` binds it to `expval`, i.e., the result of evaluating `(* 3 3)`, or 9.

    *   `boolean-expression` is evaluated:

        ```lisp
        start:   (< 9 (incf n)        100)
        becomes: (< 9 (setq n (1+ n)) 100)
        becomes: (< 9 (setq n (1+ 9)) 100)
        ```

        ```lisp
        becomes: (< 9 (setq n 10)     100)
                                           ; side-effect here!
        becomes: (< 9       n         100) ; n now bound to 10
        becomes: (< 9      10         100)
        becomes: t
        ```

    *   Because the result of the evaluation is non-`nil`, `guard` matches, `and` matches, and control passes to that clause’s body forms.

    Aside from the mathematical incorrectness of asserting that 9 is a double-digit integer, there is another problem with `MAYBE`. The body form references `n` once more, yet we do not see the updated value—10—at all. What happened to it?

    To sum up, it’s best to avoid side-effecting references to `symbol` patterns entirely, not only in `boolean-expression` (in `guard`), but also in `expr` (in `let`) and `function` (in `pred` and `app`).

3.  On match, the clause’s body forms can reference the set of symbols the pattern let-binds. When `seqpat` is `and`, this set is the union of all the symbols each of its sub-patterns let-binds. This makes sense because, for `and` to match, all the sub-patterns must match.

    When `seqpat` is `or`, things are different: `or` matches at the first sub-pattern that matches; the rest of the sub-patterns are ignored. It makes no sense for each sub-pattern to let-bind a different set of symbols because the body forms have no way to distinguish which sub-pattern matched and choose among the different sets. For example, the following is invalid:

    ```lisp
    (require 'cl-lib)
    (pcase (read-number "Enter an integer: ")
      ((or (and (pred cl-evenp)
                e-num)      ; bind e-num to expval
           o-num)           ; bind o-num to expval
       (list e-num o-num)))
    ```

    ```lisp
    ```

    ```lisp
    Enter an integer: 42
    error→ Symbol’s value as variable is void: o-num
    ```

    ```lisp
    Enter an integer: 149
    error→ Symbol’s value as variable is void: e-num
    ```

    Evaluating body form `(list e-num o-num)` signals error. To distinguish between sub-patterns, you can use another symbol, identical in name in all sub-patterns but differing in value. Reworking the above example:

    ```lisp
    (require 'cl-lib)
    (pcase (read-number "Enter an integer: ")
      ((and num                                ; line 1
            (or (and (pred cl-evenp)           ; line 2
                     (let spin 'even))         ; line 3
                (let spin 'odd)))              ; line 4
       (list spin num)))                       ; line 5
    ```

    ```lisp
    ```

    ```lisp
    Enter an integer: 42
    ⇒ (even 42)
    ```

    ```lisp
    Enter an integer: 149
    ⇒ (odd 149)
    ```

    Line 1 “factors out” the `expval` binding with `and` and `symbol` (in this case, `num`). On line 2, `or` begins in the same way as before, but instead of binding different symbols, uses `let` twice (lines 3-4) to bind the same symbol `spin` in both sub-patterns. The value of `spin` distinguishes the sub-patterns. The body form references both symbols (line 5).
