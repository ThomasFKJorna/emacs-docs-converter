

Next: [Minibuffer Completion](Minibuffer-Completion.html), Up: [Completion](Completion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 20.6.1 Basic Completion Functions

The following completion functions have nothing in themselves to do with minibuffers. We describe them here to keep them near the higher-level completion features that do use the minibuffer.

*   Function: **try-completion** *string collection \&optional predicate*

    This function returns the longest common substring of all possible completions of `string` in `collection`.

    `collection` is called the *completion table*. Its value must be a list of strings or cons cells, an obarray, a hash table, or a completion function.

    `try-completion` compares `string` against each of the permissible completions specified by the completion table. If no permissible completions match, it returns `nil`. If there is just one matching completion, and the match is exact, it returns `t`. Otherwise, it returns the longest initial sequence common to all possible matching completions.

    If `collection` is a list, the permissible completions are specified by the elements of the list, each of which should be either a string, or a cons cell whose CAR is either a string or a symbol (a symbol is converted to a string using `symbol-name`). If the list contains elements of any other type, those are ignored.

    If `collection` is an obarray (see [Creating Symbols](Creating-Symbols.html)), the names of all symbols in the obarray form the set of permissible completions.

    If `collection` is a hash table, then the keys that are strings or symbols are the possible completions. Other keys are ignored.

    You can also use a function as `collection`. Then the function is solely responsible for performing completion; `try-completion` returns whatever this function returns. The function is called with three arguments: `string`, `predicate` and `nil` (the third argument is so that the same function can be used in `all-completions` and do the appropriate thing in either case). See [Programmed Completion](Programmed-Completion.html).

    If the argument `predicate` is non-`nil`, then it must be a function of one argument, unless `collection` is a hash table, in which case it should be a function of two arguments. It is used to test each possible match, and the match is accepted only if `predicate` returns non-`nil`. The argument given to `predicate` is either a string or a cons cell (the CAR of which is a string) from the alist, or a symbol (*not* a symbol name) from the obarray. If `collection` is a hash table, `predicate` is called with two arguments, the string key and the associated value.

    In addition, to be acceptable, a completion must also match all the regular expressions in `completion-regexp-list`. (Unless `collection` is a function, in which case that function has to handle `completion-regexp-list` itself.)

    In the first of the following examples, the string ‘`foo`’ is matched by three of the alist CARs. All of the matches begin with the characters ‘`fooba`’, so that is the result. In the second example, there is only one possible match, and it is exact, so the return value is `t`.

    ```lisp
    (try-completion
     "foo"
     '(("foobar1" 1) ("barfoo" 2) ("foobaz" 3) ("foobar2" 4)))
         ⇒ "fooba"
    ```

    ```lisp
    ```

    ```lisp
    (try-completion "foo" '(("barfoo" 2) ("foo" 3)))
         ⇒ t
    ```

    In the following example, numerous symbols begin with the characters ‘`forw`’, and all of them begin with the word ‘`forward`’. In most of the symbols, this is followed with a ‘`-`’, but not in all, so no more than ‘`forward`’ can be completed.

    ```lisp
    (try-completion "forw" obarray)
         ⇒ "forward"
    ```

    Finally, in the following example, only two of the three possible matches pass the predicate `test` (the string ‘`foobaz`’ is too short). Both of those begin with the string ‘`foobar`’.

    ```lisp
    (defun test (s)
      (> (length (car s)) 6))
         ⇒ test
    ```

    ```lisp
    (try-completion
     "foo"
     '(("foobar1" 1) ("barfoo" 2) ("foobaz" 3) ("foobar2" 4))
     'test)
         ⇒ "foobar"
    ```

<!---->

*   Function: **all-completions** *string collection \&optional predicate*

    This function returns a list of all possible completions of `string`. The arguments to this function are the same as those of `try-completion`, and it uses `completion-regexp-list` in the same way that `try-completion` does.

    If `collection` is a function, it is called with three arguments: `string`, `predicate` and `t`; then `all-completions` returns whatever the function returns. See [Programmed Completion](Programmed-Completion.html).

    Here is an example, using the function `test` shown in the example for `try-completion`:

    ```lisp
    (defun test (s)
      (> (length (car s)) 6))
         ⇒ test
    ```

    ```lisp
    ```

    ```lisp
    (all-completions
     "foo"
     '(("foobar1" 1) ("barfoo" 2) ("foobaz" 3) ("foobar2" 4))
     'test)
         ⇒ ("foobar1" "foobar2")
    ```

<!---->

*   Function: **test-completion** *string collection \&optional predicate*

    This function returns non-`nil` if `string` is a valid completion alternative specified by `collection` and `predicate`. The arguments are the same as in `try-completion`. For instance, if `collection` is a list of strings, this is true if `string` appears in the list and `predicate` is satisfied.

    This function uses `completion-regexp-list` in the same way that `try-completion` does.

    If `predicate` is non-`nil` and if `collection` contains several strings that are equal to each other, as determined by `compare-strings` according to `completion-ignore-case`, then `predicate` should accept either all or none of them. Otherwise, the return value of `test-completion` is essentially unpredictable.

    If `collection` is a function, it is called with three arguments, the values `string`, `predicate` and `lambda`; whatever it returns, `test-completion` returns in turn.

<!---->

*   Function: **completion-boundaries** *string collection predicate suffix*

    This function returns the boundaries of the field on which `collection` will operate, assuming that `string` holds the text before point and `suffix` holds the text after point.

    Normally completion operates on the whole string, so for all normal collections, this will always return `(0 . (length suffix))`. But more complex completion such as completion on files is done one field at a time. For example, completion of `"/usr/sh"` will include `"/usr/share/"` but not `"/usr/share/doc"` even if `"/usr/share/doc"` exists. Also `all-completions` on `"/usr/sh"` will not include `"/usr/share/"` but only `"share/"`. So if `string` is `"/usr/sh"` and `suffix` is `"e/doc"`, `completion-boundaries` will return `(5 . 1)` which tells us that the `collection` will only return completion information that pertains to the area after `"/usr/"` and before `"/doc"`.

If you store a completion alist in a variable, you should mark the variable as risky by giving it a non-`nil` `risky-local-variable` property. See [File Local Variables](File-Local-Variables.html).

*   Variable: **completion-ignore-case**

    If the value of this variable is non-`nil`, case is not considered significant in completion. Within `read-file-name`, this variable is overridden by `read-file-name-completion-ignore-case` (see [Reading File Names](Reading-File-Names.html)); within `read-buffer`, it is overridden by `read-buffer-completion-ignore-case` (see [High-Level Completion](High_002dLevel-Completion.html)).

<!---->

*   Variable: **completion-regexp-list**

    This is a list of regular expressions. The completion functions only consider a completion acceptable if it matches all regular expressions in this list, with `case-fold-search` (see [Searching and Case](Searching-and-Case.html)) bound to the value of `completion-ignore-case`.

<!---->

*   Macro: **lazy-completion-table** *var fun*

    This macro provides a way to initialize the variable `var` as a collection for completion in a lazy way, not computing its actual contents until they are first needed. You use this macro to produce a value that you store in `var`. The actual computation of the proper value is done the first time you do completion using `var`. It is done by calling `fun` with no arguments. The value `fun` returns becomes the permanent value of `var`.

    Here is an example:

    ```lisp
    (defvar foo (lazy-completion-table foo make-my-alist))
    ```

There are several functions that take an existing completion table and return a modified version. `completion-table-case-fold` returns a case-insensitive table. `completion-table-in-turn` and `completion-table-merge` combine multiple input tables in different ways. `completion-table-subvert` alters a table to use a different initial prefix. `completion-table-with-quoting` returns a table suitable for operating on quoted text. `completion-table-with-predicate` filters a table with a predicate function. `completion-table-with-terminator` adds a terminating string.

Next: [Minibuffer Completion](Minibuffer-Completion.html), Up: [Completion](Completion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
