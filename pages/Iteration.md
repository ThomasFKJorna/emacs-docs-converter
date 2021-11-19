

Next: [Generators](Generators.html), Previous: [Pattern-Matching Conditional](Pattern_002dMatching-Conditional.html), Up: [Control Structures](Control-Structures.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 11.5 Iteration

Iteration means executing part of a program repetitively. For example, you might want to repeat some computation once for each element of a list, or once for each integer from 0 to `n`. You can do this in Emacs Lisp with the special form `while`:

*   Special Form: **while** *condition forms…*

    `while` first evaluates `condition`. If the result is non-`nil`, it evaluates `forms` in textual order. Then it reevaluates `condition`, and if the result is non-`nil`, it evaluates `forms` again. This process repeats until `condition` evaluates to `nil`.

    There is no limit on the number of iterations that may occur. The loop will continue until either `condition` evaluates to `nil` or until an error or `throw` jumps out of it (see [Nonlocal Exits](Nonlocal-Exits.html)).

    The value of a `while` form is always `nil`.

    ```lisp
    (setq num 0)
         ⇒ 0
    ```

    ```lisp
    (while (< num 4)
      (princ (format "Iteration %d." num))
      (setq num (1+ num)))
         -| Iteration 0.
         -| Iteration 1.
         -| Iteration 2.
         -| Iteration 3.
         ⇒ nil
    ```

    To write a repeat-until loop, which will execute something on each iteration and then do the end-test, put the body followed by the end-test in a `progn` as the first argument of `while`, as shown here:

    ```lisp
    (while (progn
             (forward-line 1)
             (not (looking-at "^$"))))
    ```

    This moves forward one line and continues moving by lines until it reaches an empty line. It is peculiar in that the `while` has no body, just the end test (which also does the real work of moving point).

The `dolist` and `dotimes` macros provide convenient ways to write two common kinds of loops.

*   Macro: **dolist** *(var list \[result]) body…*

    This construct executes `body` once for each element of `list`, binding the variable `var` locally to hold the current element. Then it returns the value of evaluating `result`, or `nil` if `result` is omitted. For example, here is how you could use `dolist` to define the `reverse` function:

    ```lisp
    (defun reverse (list)
      (let (value)
        (dolist (elt list value)
          (setq value (cons elt value)))))
    ```

<!---->

*   Macro: **dotimes** *(var count \[result]) body…*

    This construct executes `body` once for each integer from 0 (inclusive) to `count` (exclusive), binding the variable `var` to the integer for the current iteration. Then it returns the value of evaluating `result`, or `nil` if `result` is omitted. Use of `result` is deprecated. Here is an example of using `dotimes` to do something 100 times:

    ```lisp
    (dotimes (i 100)
      (insert "I will not obey absurd orders\n"))
    ```

Next: [Generators](Generators.html), Previous: [Pattern-Matching Conditional](Pattern_002dMatching-Conditional.html), Up: [Control Structures](Control-Structures.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
