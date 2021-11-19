

Next: [Standard File Names](Standard-File-Names.html), Previous: [Unique File Names](Unique-File-Names.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.9.6 File Name Completion

This section describes low-level subroutines for completing a file name. For higher level functions, see [Reading File Names](Reading-File-Names.html).

*   Function: **file-name-all-completions** *partial-filename directory*

    This function returns a list of all possible completions for a file whose name starts with `partial-filename` in directory `directory`. The order of the completions is the order of the files in the directory, which is unpredictable and conveys no useful information.

    The argument `partial-filename` must be a file name containing no directory part and no slash (or backslash on some systems). The current buffer’s default directory is prepended to `directory`, if `directory` is not absolute.

    In the following example, suppose that `~rms/lewis` is the current default directory, and has five files whose names begin with ‘`f`’: `foo`, `file~`, `file.c`, `file.c.~1~`, and `file.c.~2~`.

    ```lisp
    (file-name-all-completions "f" "")
         ⇒ ("foo" "file~" "file.c.~2~"
                    "file.c.~1~" "file.c")
    ```

    ```lisp
    ```

    ```lisp
    (file-name-all-completions "fo" "")
         ⇒ ("foo")
    ```

<!---->

*   Function: **file-name-completion** *filename directory \&optional predicate*

    This function completes the file name `filename` in directory `directory`. It returns the longest prefix common to all file names in directory `directory` that start with `filename`. If `predicate` is non-`nil` then it ignores possible completions that don’t satisfy `predicate`, after calling that function with one argument, the expanded absolute file name.

    If only one match exists and `filename` matches it exactly, the function returns `t`. The function returns `nil` if directory `directory` contains no name starting with `filename`.

    In the following example, suppose that the current default directory has five files whose names begin with ‘`f`’: `foo`, `file~`, `file.c`, `file.c.~1~`, and `file.c.~2~`.

    ```lisp
    (file-name-completion "fi" "")
         ⇒ "file"
    ```

    ```lisp
    ```

    ```lisp
    (file-name-completion "file.c.~1" "")
         ⇒ "file.c.~1~"
    ```

    ```lisp
    ```

    ```lisp
    (file-name-completion "file.c.~1~" "")
         ⇒ t
    ```

    ```lisp
    ```

    ```lisp
    (file-name-completion "file.c.~3" "")
         ⇒ nil
    ```

<!---->

*   User Option: **completion-ignored-extensions**

    `file-name-completion` usually ignores file names that end in any string in this list. It does not ignore them when all the possible completions end in one of these suffixes. This variable has no effect on `file-name-all-completions`.

    A typical value might look like this:

    ```lisp
    completion-ignored-extensions
         ⇒ (".o" ".elc" "~" ".dvi")
    ```

    If an element of `completion-ignored-extensions` ends in a slash ‘`/`’, it signals a directory. The elements which do *not* end in a slash will never match a directory; thus, the above value will not filter out a directory named `foo.elc`.

Next: [Standard File Names](Standard-File-Names.html), Previous: [Unique File Names](Unique-File-Names.html), Up: [File Names](File-Names.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
