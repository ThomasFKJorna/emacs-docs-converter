

Next: [Buffer File Name](Buffer-File-Name.html), Previous: [Current Buffer](Current-Buffer.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 27.3 Buffer Names

Each buffer has a unique name, which is a string. Many of the functions that work on buffers accept either a buffer or a buffer name as an argument. Any argument called `buffer-or-name` is of this sort, and an error is signaled if it is neither a string nor a buffer. Any argument called `buffer` must be an actual buffer object, not a name.

Buffers that are ephemeral and generally uninteresting to the user have names starting with a space, so that the `list-buffers` and `buffer-menu` commands don’t mention them (but if such a buffer visits a file, it **is** mentioned). A name starting with space also initially disables recording undo information; see [Undo](Undo.html).

*   Function: **buffer-name** *\&optional buffer*

    This function returns the name of `buffer` as a string. `buffer` defaults to the current buffer.

    If `buffer-name` returns `nil`, it means that `buffer` has been killed. See [Killing Buffers](Killing-Buffers.html).

    ```lisp
    (buffer-name)
         ⇒ "buffers.texi"
    ```

    ```lisp
    ```

    ```lisp
    (setq foo (get-buffer "temp"))
         ⇒ #<buffer temp>
    ```

    ```lisp
    (kill-buffer foo)
         ⇒ nil
    ```

    ```lisp
    (buffer-name foo)
         ⇒ nil
    ```

    ```lisp
    foo
         ⇒ #<killed buffer>
    ```

<!---->

*   Command: **rename-buffer** *newname \&optional unique*

    This function renames the current buffer to `newname`. An error is signaled if `newname` is not a string.

    Ordinarily, `rename-buffer` signals an error if `newname` is already in use. However, if `unique` is non-`nil`, it modifies `newname` to make a name that is not in use. Interactively, you can make `unique` non-`nil` with a numeric prefix argument. (This is how the command `rename-uniquely` is implemented.)

    This function returns the name actually given to the buffer.

<!---->

*   Function: **get-buffer** *buffer-or-name*

    This function returns the buffer specified by `buffer-or-name`. If `buffer-or-name` is a string and there is no buffer with that name, the value is `nil`. If `buffer-or-name` is a buffer, it is returned as given; that is not very useful, so the argument is usually a name. For example:

    ```lisp
    (setq b (get-buffer "lewis"))
         ⇒ #<buffer lewis>
    ```

    ```lisp
    (get-buffer b)
         ⇒ #<buffer lewis>
    ```

    ```lisp
    (get-buffer "Frazzle-nots")
         ⇒ nil
    ```

    See also the function `get-buffer-create` in [Creating Buffers](Creating-Buffers.html).

<!---->

*   Function: **generate-new-buffer-name** *starting-name \&optional ignore*

    This function returns a name that would be unique for a new buffer—but does not create the buffer. It starts with `starting-name`, and produces a name not currently in use for any buffer by appending a number inside of ‘`<…>`’. It starts at 2 and keeps incrementing the number until it is not the name of an existing buffer.

    If the optional second argument `ignore` is non-`nil`, it should be a string, a potential buffer name. It means to consider that potential buffer acceptable, if it is tried, even it is the name of an existing buffer (which would normally be rejected). Thus, if buffers named ‘`foo`’, ‘`foo<2>`’, ‘`foo<3>`’ and ‘`foo<4>`’ exist,

    ```lisp
    (generate-new-buffer-name "foo")
         ⇒ "foo<5>"
    (generate-new-buffer-name "foo" "foo<3>")
         ⇒ "foo<3>"
    (generate-new-buffer-name "foo" "foo<6>")
         ⇒ "foo<5>"
    ```

    See the related function `generate-new-buffer` in [Creating Buffers](Creating-Buffers.html).

Next: [Buffer File Name](Buffer-File-Name.html), Previous: [Current Buffer](Current-Buffer.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
