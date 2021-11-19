

Previous: [Visiting Functions](Visiting-Functions.html), Up: [Visiting Files](Visiting-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.1.2 Subroutines of Visiting

The `find-file-noselect` function uses two important subroutines which are sometimes useful in user Lisp code: `create-file-buffer` and `after-find-file`. This section explains how to use them.

*   Function: **create-file-buffer** *filename*

    This function creates a suitably named buffer for visiting `filename`, and returns it. It uses `filename` (sans directory) as the name if that name is free; otherwise, it appends a string such as ‘`<2>`’ to get an unused name. See also [Creating Buffers](Creating-Buffers.html). Note that the `uniquify` library affects the result of this function. See [Uniquify](https://www.gnu.org/software/emacs/manual/html_node/emacs/Uniquify.html#Uniquify) in The GNU Emacs Manual.

    **Please note:** `create-file-buffer` does *not* associate the new buffer with a file and does not select the buffer. It also does not use the default major mode.

    ```lisp
    (create-file-buffer "foo")
         ⇒ #<buffer foo>
    ```

    ```lisp
    (create-file-buffer "foo")
         ⇒ #<buffer foo<2>>
    ```

    ```lisp
    (create-file-buffer "foo")
         ⇒ #<buffer foo<3>>
    ```

    This function is used by `find-file-noselect`. It uses `generate-new-buffer` (see [Creating Buffers](Creating-Buffers.html)).

<!---->

*   Function: **after-find-file** *\&optional error warn noauto after-find-file-from-revert-buffer nomodes*

    This function sets the buffer major mode, and parses local variables (see [Auto Major Mode](Auto-Major-Mode.html)). It is called by `find-file-noselect` and by the default revert function (see [Reverting](Reverting.html)).

    If reading the file got an error because the file does not exist, but its directory does exist, the caller should pass a non-`nil` value for `error`. In that case, `after-find-file` issues a warning: ‘`(New file)`’. For more serious errors, the caller should usually not call `after-find-file`.

    If `warn` is non-`nil`, then this function issues a warning if an auto-save file exists and is more recent than the visited file.

    If `noauto` is non-`nil`, that says not to enable or disable Auto-Save mode. The mode remains enabled if it was enabled before.

    If `after-find-file-from-revert-buffer` is non-`nil`, that means this call was from `revert-buffer`. This has no direct effect, but some mode functions and hook functions check the value of this variable.

    If `nomodes` is non-`nil`, that means don’t alter the buffer’s major mode, don’t process local variables specifications in the file, and don’t run `find-file-hook`. This feature is used by `revert-buffer` in some cases.

    The last thing `after-find-file` does is call all the functions in the list `find-file-hook`.

Previous: [Visiting Functions](Visiting-Functions.html), Up: [Visiting Files](Visiting-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
