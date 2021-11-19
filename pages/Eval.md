

Next: [Deferred Eval](Deferred-Eval.html), Previous: [Backquote](Backquote.html), Up: [Evaluation](Evaluation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 10.5 Eval

Most often, forms are evaluated automatically, by virtue of their occurrence in a program being run. On rare occasions, you may need to write code that evaluates a form that is computed at run time, such as after reading a form from text being edited or getting one from a property list. On these occasions, use the `eval` function. Often `eval` is not needed and something else should be used instead. For example, to get the value of a variable, while `eval` works, `symbol-value` is preferable; or rather than store expressions in a property list that then need to go through `eval`, it is better to store functions instead that are then passed to `funcall`.

The functions and variables described in this section evaluate forms, specify limits to the evaluation process, or record recently returned values. Loading a file also does evaluation (see [Loading](Loading.html)).

It is generally cleaner and more flexible to store a function in a data structure, and call it with `funcall` or `apply`, than to store an expression in the data structure and evaluate it. Using functions provides the ability to pass information to them as arguments.

*   Function: **eval** *form \&optional lexical*

    This is the basic function for evaluating an expression. It evaluates `form` in the current environment, and returns the result. The type of the `form` object determines how it is evaluated. See [Forms](Forms.html).

    The argument `lexical` specifies the scoping rule for local variables (see [Variable Scoping](Variable-Scoping.html)). If it is omitted or `nil`, that means to evaluate `form` using the default dynamic scoping rule. If it is `t`, that means to use the lexical scoping rule. The value of `lexical` can also be a non-empty alist specifying a particular *lexical environment* for lexical bindings; however, this feature is only useful for specialized purposes, such as in Emacs Lisp debuggers. See [Lexical Binding](Lexical-Binding.html).

    Since `eval` is a function, the argument expression that appears in a call to `eval` is evaluated twice: once as preparation before `eval` is called, and again by the `eval` function itself. Here is an example:

    ```lisp
    (setq foo 'bar)
         ⇒ bar
    ```

    ```lisp
    (setq bar 'baz)
         ⇒ baz
    ;; Here eval receives argument foo
    (eval 'foo)
         ⇒ bar
    ;; Here eval receives argument bar, which is the value of foo
    (eval foo)
         ⇒ baz
    ```

    The number of currently active calls to `eval` is limited to `max-lisp-eval-depth` (see below).

<!---->

*   Command: **eval-region** *start end \&optional stream read-function*

    This function evaluates the forms in the current buffer in the region defined by the positions `start` and `end`. It reads forms from the region and calls `eval` on them until the end of the region is reached, or until an error is signaled and not handled.

    By default, `eval-region` does not produce any output. However, if `stream` is non-`nil`, any output produced by output functions (see [Output Functions](Output-Functions.html)), as well as the values that result from evaluating the expressions in the region are printed using `stream`. See [Output Streams](Output-Streams.html).

    If `read-function` is non-`nil`, it should be a function, which is used instead of `read` to read expressions one by one. This function is called with one argument, the stream for reading input. You can also use the variable `load-read-function` (see [How Programs Do Loading](How-Programs-Do-Loading.html#Definition-of-load_002dread_002dfunction)) to specify this function, but it is more robust to use the `read-function` argument.

    `eval-region` does not move point. It always returns `nil`.

<!---->

*   Command: **eval-buffer** *\&optional buffer-or-name stream filename unibyte print*

    This is similar to `eval-region`, but the arguments provide different optional features. `eval-buffer` operates on the entire accessible portion of buffer `buffer-or-name` (see [Narrowing](https://www.gnu.org/software/emacs/manual/html_node/emacs/Narrowing.html#Narrowing) in The GNU Emacs Manual). `buffer-or-name` can be a buffer, a buffer name (a string), or `nil` (or omitted), which means to use the current buffer. `stream` is used as in `eval-region`, unless `stream` is `nil` and `print` non-`nil`. In that case, values that result from evaluating the expressions are still discarded, but the output of the output functions is printed in the echo area. `filename` is the file name to use for `load-history` (see [Unloading](Unloading.html)), and defaults to `buffer-file-name` (see [Buffer File Name](Buffer-File-Name.html)). If `unibyte` is non-`nil`, `read` converts strings to unibyte whenever possible.

    `eval-current-buffer` is an alias for this command.

<!---->

*   User Option: **max-lisp-eval-depth**

    This variable defines the maximum depth allowed in calls to `eval`, `apply`, and `funcall` before an error is signaled (with error message `"Lisp nesting exceeds max-lisp-eval-depth"`).

    This limit, with the associated error when it is exceeded, is one way Emacs Lisp avoids infinite recursion on an ill-defined function. If you increase the value of `max-lisp-eval-depth` too much, such code can cause stack overflow instead. On some systems, this overflow can be handled. In that case, normal Lisp evaluation is interrupted and control is transferred back to the top level command loop (`top-level`). Note that there is no way to enter Emacs Lisp debugger in this situation. See [Error Debugging](Error-Debugging.html).

    The depth limit counts internal uses of `eval`, `apply`, and `funcall`, such as for calling the functions mentioned in Lisp expressions, and recursive evaluation of function call arguments and function body forms, as well as explicit calls in Lisp code.

    The default value of this variable is 800. If you set it to a value less than 100, Lisp will reset it to 100 if the given value is reached. Entry to the Lisp debugger increases the value, if there is little room left, to make sure the debugger itself has room to execute.

    `max-specpdl-size` provides another limit on nesting. See [Local Variables](Local-Variables.html#Definition-of-max_002dspecpdl_002dsize).

<!---->

*   Variable: **values**

    The value of this variable is a list of the values returned by all the expressions that were read, evaluated, and printed from buffers (including the minibuffer) by the standard Emacs commands which do this. (Note that this does *not* include evaluation in `*ielm*` buffers, nor evaluation using `C-j`, `C-x C-e`, and similar evaluation commands in `lisp-interaction-mode`.) The elements are ordered most recent first.

    ```lisp
    (setq x 1)
         ⇒ 1
    ```

    ```lisp
    (list 'A (1+ 2) auto-save-default)
         ⇒ (A 3 t)
    ```

    ```lisp
    values
         ⇒ ((A 3 t) 1 …)
    ```

    This variable is useful for referring back to values of forms recently evaluated. It is generally a bad idea to print the value of `values` itself, since this may be very long. Instead, examine particular elements, like this:

    ```lisp
    ;; Refer to the most recent evaluation result.
    (nth 0 values)
         ⇒ (A 3 t)
    ```

    ```lisp
    ;; That put a new element on,
    ;;   so all elements move back one.
    (nth 1 values)
         ⇒ (A 3 t)
    ```

    ```lisp
    ;; This gets the element that was next-to-most-recent
    ;;   before this example.
    (nth 3 values)
         ⇒ 1
    ```

Next: [Deferred Eval](Deferred-Eval.html), Previous: [Backquote](Backquote.html), Up: [Evaluation](Evaluation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
