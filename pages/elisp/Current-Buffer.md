

### 27.2 The Current Buffer

There are, in general, many buffers in an Emacs session. At any time, one of them is designated the *current buffer*—the buffer in which most editing takes place. Most of the primitives for examining or changing text operate implicitly on the current buffer (see [Text](Text.html)).

Normally, the buffer displayed in the selected window is the current buffer, but this is not always so: a Lisp program can temporarily designate any buffer as current in order to operate on its contents, without changing what is displayed on the screen. The most basic function for designating a current buffer is `set-buffer`.

### Function: **current-buffer**

This function returns the current buffer.

```lisp
(current-buffer)
     ⇒ #<buffer buffers.texi>
```

### Function: **set-buffer** *buffer-or-name*

This function makes `buffer-or-name` the current buffer. `buffer-or-name` must be an existing buffer or the name of an existing buffer. The return value is the buffer made current.

This function does not display the buffer in any window, so the user cannot necessarily see the buffer. But Lisp programs will now operate on it.

When an editing command returns to the editor command loop, Emacs automatically calls `set-buffer` on the buffer shown in the selected window. This is to prevent confusion: it ensures that the buffer that the cursor is in, when Emacs reads a command, is the buffer to which that command applies (see [Command Loop](Command-Loop.html)). Thus, you should not use `set-buffer` to switch visibly to a different buffer; for that, use the functions described in [Switching Buffers](Switching-Buffers.html).

When writing a Lisp function, do *not* rely on this behavior of the command loop to restore the current buffer after an operation. Editing commands can also be called as Lisp functions by other programs, not just from the command loop; it is convenient for the caller if the subroutine does not change which buffer is current (unless, of course, that is the subroutine’s purpose).

To operate temporarily on another buffer, put the `set-buffer` within a `save-current-buffer` form. Here, as an example, is a simplified version of the command `append-to-buffer`:

```lisp
(defun append-to-buffer (buffer start end)
  "Append the text of the region to BUFFER."
  (interactive "BAppend to buffer: \nr")
  (let ((oldbuf (current-buffer)))
    (save-current-buffer
      (set-buffer (get-buffer-create buffer))
      (insert-buffer-substring oldbuf start end))))
```

Here, we bind a local variable to record the current buffer, and then `save-current-buffer` arranges to make it current again later. Next, `set-buffer` makes the specified buffer current, and `insert-buffer-substring` copies the string from the original buffer to the specified (and now current) buffer.

Alternatively, we can use the `with-current-buffer` macro:

```lisp
(defun append-to-buffer (buffer start end)
  "Append the text of the region to BUFFER."
  (interactive "BAppend to buffer: \nr")
  (let ((oldbuf (current-buffer)))
    (with-current-buffer (get-buffer-create buffer)
      (insert-buffer-substring oldbuf start end))))
```

In either case, if the buffer appended to happens to be displayed in some window, the next redisplay will show how its text has changed. If it is not displayed in any window, you will not see the change immediately on the screen. The command causes the buffer to become current temporarily, but does not cause it to be displayed.

If you make local bindings (with `let` or function arguments) for a variable that may also have buffer-local bindings, make sure that the same buffer is current at the beginning and at the end of the local binding’s scope. Otherwise you might bind it in one buffer and unbind it in another!

Do not rely on using `set-buffer` to change the current buffer back, because that won’t do the job if a quit happens while the wrong buffer is current. For instance, in the previous example, it would have been wrong to do this:

```lisp
  (let ((oldbuf (current-buffer)))
    (set-buffer (get-buffer-create buffer))
    (insert-buffer-substring oldbuf start end)
    (set-buffer oldbuf))
```

Using `save-current-buffer` or `with-current-buffer`, as we did, correctly handles quitting, errors, and `throw`, as well as ordinary evaluation.

### Special Form: **save-current-buffer** *body…*

The `save-current-buffer` special form saves the identity of the current buffer, evaluates the `body` forms, and finally restores that buffer as current. The return value is the value of the last form in `body`. The current buffer is restored even in case of an abnormal exit via `throw` or error (see [Nonlocal Exits](Nonlocal-Exits.html)).

If the buffer that used to be current has been killed by the time of exit from `save-current-buffer`, then it is not made current again, of course. Instead, whichever buffer was current just before exit remains current.

### Macro: **with-current-buffer** *buffer-or-name body…*

The `with-current-buffer` macro saves the identity of the current buffer, makes `buffer-or-name` current, evaluates the `body` forms, and finally restores the current buffer. `buffer-or-name` must specify an existing buffer or the name of an existing buffer.

The return value is the value of the last form in `body`. The current buffer is restored even in case of an abnormal exit via `throw` or error (see [Nonlocal Exits](Nonlocal-Exits.html)).

### Macro: **with-temp-buffer** *body…*

The `with-temp-buffer` macro evaluates the `body` forms with a temporary buffer as the current buffer. It saves the identity of the current buffer, creates a temporary buffer and makes it current, evaluates the `body` forms, and finally restores the previous current buffer while killing the temporary buffer. By default, undo information (see [Undo](Undo.html)) is not recorded in the buffer created by this macro (but `body` can enable that, if needed).

The return value is the value of the last form in `body`. You can return the contents of the temporary buffer by using `(buffer-string)` as the last form.

The current buffer is restored even in case of an abnormal exit via `throw` or error (see [Nonlocal Exits](Nonlocal-Exits.html)).

See also `with-temp-file` in [Writing to Files](Writing-to-Files.html#Definition-of-with_002dtemp_002dfile).
