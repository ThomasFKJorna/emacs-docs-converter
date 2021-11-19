

Next: [Output Variables](Output-Variables.html), Previous: [Output Streams](Output-Streams.html), Up: [Read and Print](Read-and-Print.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 19.5 Output Functions

This section describes the Lisp functions for printing Lisp objects—converting objects into their printed representation.

Some of the Emacs printing functions add quoting characters to the output when necessary so that it can be read properly. The quoting characters used are ‘`"`’ and ‘`\`’; they distinguish strings from symbols, and prevent punctuation characters in strings and symbols from being taken as delimiters when reading. See [Printed Representation](Printed-Representation.html), for full details. You specify quoting or no quoting by the choice of printing function.

If the text is to be read back into Lisp, then you should print with quoting characters to avoid ambiguity. Likewise, if the purpose is to describe a Lisp object clearly for a Lisp programmer. However, if the purpose of the output is to look nice for humans, then it is usually better to print without quoting.

Lisp objects can refer to themselves. Printing a self-referential object in the normal way would require an infinite amount of text, and the attempt could cause infinite recursion. Emacs detects such recursion and prints ‘`#level`’ instead of recursively printing an object already being printed. For example, here ‘`#0`’ indicates a recursive reference to the object at level 0 of the current print operation:

```lisp
(setq foo (list nil))
     ⇒ (nil)
(setcar foo foo)
     ⇒ (#0)
```

In the functions below, `stream` stands for an output stream. (See the previous section for a description of output streams. Also See [external-debugging-output](Output-Streams.html#external_002ddebugging_002doutput), a useful stream value for debugging.) If `stream` is `nil` or omitted, it defaults to the value of `standard-output`.

*   Function: **print** *object \&optional stream*

    The `print` function is a convenient way of printing. It outputs the printed representation of `object` to `stream`, printing in addition one newline before `object` and another after it. Quoting characters are used. `print` returns `object`. For example:

    ```lisp
    (progn (print 'The\ cat\ in)
           (print "the hat")
           (print " came back"))
         -|
         -| The\ cat\ in
         -|
         -| "the hat"
         -|
         -| " came back"
         ⇒ " came back"
    ```

<!---->

*   Function: **prin1** *object \&optional stream*

    This function outputs the printed representation of `object` to `stream`. It does not print newlines to separate output as `print` does, but it does use quoting characters just like `print`. It returns `object`.

    ```lisp
    (progn (prin1 'The\ cat\ in)
           (prin1 "the hat")
           (prin1 " came back"))
         -| The\ cat\ in"the hat"" came back"
         ⇒ " came back"
    ```

<!---->

*   Function: **princ** *object \&optional stream*

    This function outputs the printed representation of `object` to `stream`. It returns `object`.

    This function is intended to produce output that is readable by people, not by `read`, so it doesn’t insert quoting characters and doesn’t put double-quotes around the contents of strings. It does not add any spacing between calls.

    ```lisp
    (progn
      (princ 'The\ cat)
      (princ " in the \"hat\""))
         -| The cat in the "hat"
         ⇒ " in the \"hat\""
    ```

<!---->

*   Function: **terpri** *\&optional stream ensure*

    This function outputs a newline to `stream`. The name stands for “terminate print”. If `ensure` is non-`nil` no newline is printed if `stream` is already at the beginning of a line. Note in this case `stream` can not be a function and an error is signaled if it is. This function returns `t` if a newline is printed.

<!---->

*   Function: **write-char** *character \&optional stream*

    This function outputs `character` to `stream`. It returns `character`.

<!---->

*   Function: **prin1-to-string** *object \&optional noescape*

    This function returns a string containing the text that `prin1` would have printed for the same argument.

    ```lisp
    (prin1-to-string 'foo)
         ⇒ "foo"
    ```

    ```lisp
    (prin1-to-string (mark-marker))
         ⇒ "#<marker at 2773 in strings.texi>"
    ```

    If `noescape` is non-`nil`, that inhibits use of quoting characters in the output. (This argument is supported in Emacs versions 19 and later.)

    ```lisp
    (prin1-to-string "foo")
         ⇒ "\"foo\""
    ```

    ```lisp
    (prin1-to-string "foo" t)
         ⇒ "foo"
    ```

    See `format`, in [Formatting Strings](Formatting-Strings.html), for other ways to obtain the printed representation of a Lisp object as a string.

<!---->

*   Macro: **with-output-to-string** *body…*

    This macro executes the `body` forms with `standard-output` set up to feed output into a string. Then it returns that string.

    For example, if the current buffer name is ‘`foo`’,

    ```lisp
    (with-output-to-string
      (princ "The buffer is ")
      (princ (buffer-name)))
    ```

    returns `"The buffer is foo"`.

<!---->

*   Function: **pp** *object \&optional stream*

    This function outputs `object` to `stream`, just like `prin1`, but does it in a prettier way. That is, it’ll indent and fill the object to make it more readable for humans.

If you need to use binary I/O in batch mode, e.g., use the functions described in this section to write out arbitrary binary data or avoid conversion of newlines on non-POSIX hosts, see [set-binary-mode](Input-Functions.html).

Next: [Output Variables](Output-Variables.html), Previous: [Output Streams](Output-Streams.html), Up: [Read and Print](Read-and-Print.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
