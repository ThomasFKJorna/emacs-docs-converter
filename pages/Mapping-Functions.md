

Next: [Anonymous Functions](Anonymous-Functions.html), Previous: [Calling Functions](Calling-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 13.6 Mapping Functions

A *mapping function* applies a given function (*not* a special form or macro) to each element of a list or other collection. Emacs Lisp has several such functions; this section describes `mapcar`, `mapc`, `mapconcat`, and `mapcan`, which map over a list. See [Definition of mapatoms](Creating-Symbols.html#Definition-of-mapatoms), for the function `mapatoms` which maps over the symbols in an obarray. See [Definition of maphash](Hash-Access.html#Definition-of-maphash), for the function `maphash` which maps over key/value associations in a hash table.

These mapping functions do not allow char-tables because a char-table is a sparse array whose nominal range of indices is very large. To map over a char-table in a way that deals properly with its sparse nature, use the function `map-char-table` (see [Char-Tables](Char_002dTables.html)).

*   Function: **mapcar** *function sequence*

    `mapcar` applies `function` to each element of `sequence` in turn, and returns a list of the results.

    The argument `sequence` can be any kind of sequence except a char-table; that is, a list, a vector, a bool-vector, or a string. The result is always a list. The length of the result is the same as the length of `sequence`. For example:

    ```lisp
    (mapcar 'car '((a b) (c d) (e f)))
         ⇒ (a c e)
    (mapcar '1+ [1 2 3])
         ⇒ (2 3 4)
    (mapcar 'string "abc")
         ⇒ ("a" "b" "c")
    ```

    ```lisp
    ```

    ```lisp
    ;; Call each function in my-hooks.
    (mapcar 'funcall my-hooks)
    ```

    ```lisp
    ```

    ```lisp
    (defun mapcar* (function &rest args)
      "Apply FUNCTION to successive cars of all ARGS.
    Return the list of results."
      ;; If no list is exhausted,
      (if (not (memq nil args))
          ;; apply function to CARs.
          (cons (apply function (mapcar 'car args))
                (apply 'mapcar* function
                       ;; Recurse for rest of elements.
                       (mapcar 'cdr args)))))
    ```

    ```lisp
    ```

    ```lisp
    (mapcar* 'cons '(a b c) '(1 2 3 4))
         ⇒ ((a . 1) (b . 2) (c . 3))
    ```

<!---->

*   Function: **mapcan** *function sequence*

    This function applies `function` to each element of `sequence`, like `mapcar`, but instead of collecting the results into a list, it returns a single list with all the elements of the results (which must be lists), by altering the results (using `nconc`; see [Rearrangement](Rearrangement.html)). Like with `mapcar`, `sequence` can be of any type except a char-table.

    ```lisp
    ;; Contrast this:
    (mapcar 'list '(a b c d))
         ⇒ ((a) (b) (c) (d))
    ;; with this:
    (mapcan 'list '(a b c d))
         ⇒ (a b c d)
    ```

<!---->

*   Function: **mapc** *function sequence*

    `mapc` is like `mapcar` except that `function` is used for side-effects only—the values it returns are ignored, not collected into a list. `mapc` always returns `sequence`.

<!---->

*   Function: **mapconcat** *function sequence separator*

    `mapconcat` applies `function` to each element of `sequence`; the results, which must be sequences of characters (strings, vectors, or lists), are concatenated into a single string return value. Between each pair of result sequences, `mapconcat` inserts the characters from `separator`, which also must be a string, or a vector or list of characters. See [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html).

    The argument `function` must be a function that can take one argument and returns a sequence of characters: a string, a vector, or a list. The argument `sequence` can be any kind of sequence except a char-table; that is, a list, a vector, a bool-vector, or a string.

    ```lisp
    (mapconcat 'symbol-name
               '(The cat in the hat)
               " ")
         ⇒ "The cat in the hat"
    ```

    ```lisp
    ```

    ```lisp
    (mapconcat (lambda (x) (format "%c" (1+ x)))
               "HAL-8000"
               "")
         ⇒ "IBM.9111"
    ```

Next: [Anonymous Functions](Anonymous-Functions.html), Previous: [Calling Functions](Calling-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
