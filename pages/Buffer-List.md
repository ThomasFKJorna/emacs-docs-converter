

Next: [Creating Buffers](Creating-Buffers.html), Previous: [Read Only Buffers](Read-Only-Buffers.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 27.8 The Buffer List

The *buffer list* is a list of all live buffers. The order of the buffers in this list is based primarily on how recently each buffer has been displayed in a window. Several functions, notably `other-buffer`, use this ordering. A buffer list displayed for the user also follows this order.

Creating a buffer adds it to the end of the buffer list, and killing a buffer removes it from that list. A buffer moves to the front of this list whenever it is chosen for display in a window (see [Switching Buffers](Switching-Buffers.html)) or a window displaying it is selected (see [Selecting Windows](Selecting-Windows.html)). A buffer moves to the end of the list when it is buried (see `bury-buffer`, below). There are no functions available to the Lisp programmer which directly manipulate the buffer list.

In addition to the fundamental buffer list just described, Emacs maintains a local buffer list for each frame, in which the buffers that have been displayed (or had their windows selected) in that frame come first. (This order is recorded in the frame’s `buffer-list` frame parameter; see [Buffer Parameters](Buffer-Parameters.html).) Buffers never displayed in that frame come afterward, ordered according to the fundamental buffer list.

*   Function: **buffer-list** *\&optional frame*

    This function returns the buffer list, including all buffers, even those whose names begin with a space. The elements are actual buffers, not their names.

    If `frame` is a frame, this returns `frame`’s local buffer list. If `frame` is `nil` or omitted, the fundamental buffer list is used: the buffers appear in order of most recent display or selection, regardless of which frames they were displayed on.

    ```lisp
    (buffer-list)
         ⇒ (#<buffer buffers.texi>
             #<buffer  *Minibuf-1*> #<buffer buffer.c>
             #<buffer *Help*> #<buffer TAGS>)
    ```

    ```lisp
    ```

    ```lisp
    ;; Note that the name of the minibuffer
    ;;   begins with a space!
    (mapcar #'buffer-name (buffer-list))
        ⇒ ("buffers.texi" " *Minibuf-1*"
            "buffer.c" "*Help*" "TAGS")
    ```

The list returned by `buffer-list` is constructed specifically; it is not an internal Emacs data structure, and modifying it has no effect on the order of buffers. If you want to change the order of buffers in the fundamental buffer list, here is an easy way:

```lisp
(defun reorder-buffer-list (new-list)
  (while new-list
    (bury-buffer (car new-list))
    (setq new-list (cdr new-list))))
```

With this method, you can specify any order for the list, but there is no danger of losing a buffer or adding something that is not a valid live buffer.

To change the order or value of a specific frame’s buffer list, set that frame’s `buffer-list` parameter with `modify-frame-parameters` (see [Parameter Access](Parameter-Access.html)).

*   Function: **other-buffer** *\&optional buffer visible-ok frame*

    This function returns the first buffer in the buffer list other than `buffer`. Usually, this is the buffer appearing in the most recently selected window (in frame `frame` or else the selected frame, see [Input Focus](Input-Focus.html)), aside from `buffer`. Buffers whose names start with a space are not considered at all.

    If `buffer` is not supplied (or if it is not a live buffer), then `other-buffer` returns the first buffer in the selected frame’s local buffer list. (If `frame` is non-`nil`, it returns the first buffer in `frame`’s local buffer list instead.)

    If `frame` has a non-`nil` `buffer-predicate` parameter, then `other-buffer` uses that predicate to decide which buffers to consider. It calls the predicate once for each buffer, and if the value is `nil`, that buffer is ignored. See [Buffer Parameters](Buffer-Parameters.html).

    If `visible-ok` is `nil`, `other-buffer` avoids returning a buffer visible in any window on any visible frame, except as a last resort. If `visible-ok` is non-`nil`, then it does not matter whether a buffer is displayed somewhere or not.

    If no suitable buffer exists, the buffer `*scratch*` is returned (and created, if necessary).

<!---->

*   Function: **last-buffer** *\&optional buffer visible-ok frame*

    This function returns the last buffer in `frame`’s buffer list other than `buffer`. If `frame` is omitted or `nil`, it uses the selected frame’s buffer list.

    The argument `visible-ok` is handled as with `other-buffer`, see above. If no suitable buffer can be found, the buffer `*scratch*` is returned.

<!---->

*   Command: **bury-buffer** *\&optional buffer-or-name*

    This command puts `buffer-or-name` at the end of the buffer list, without changing the order of any of the other buffers on the list. This buffer therefore becomes the least desirable candidate for `other-buffer` to return. The argument can be either a buffer itself or the name of one.

    This function operates on each frame’s `buffer-list` parameter as well as the fundamental buffer list; therefore, the buffer that you bury will come last in the value of `(buffer-list frame)` and in the value of `(buffer-list)`. In addition, it also puts the buffer at the end of the list of buffers of the selected window (see [Window History](Window-History.html)) provided it is shown in that window.

    If `buffer-or-name` is `nil` or omitted, this means to bury the current buffer. In addition, if the current buffer is displayed in the selected window, this makes sure that the window is either deleted or another buffer is shown in it. More precisely, if the selected window is dedicated (see [Dedicated Windows](Dedicated-Windows.html)) and there are other windows on its frame, the window is deleted. If it is the only window on its frame and that frame is not the only frame on its terminal, the frame is dismissed by calling the function specified by `frame-auto-hide-function` (see [Quitting Windows](Quitting-Windows.html)). Otherwise, it calls `switch-to-prev-buffer` (see [Window History](Window-History.html)) to show another buffer in that window. If `buffer-or-name` is displayed in some other window, it remains displayed there.

    To replace a buffer in all the windows that display it, use `replace-buffer-in-windows`, See [Buffers and Windows](Buffers-and-Windows.html).

<!---->

*   Command: **unbury-buffer**

    This command switches to the last buffer in the local buffer list of the selected frame. More precisely, it calls the function `switch-to-buffer` (see [Switching Buffers](Switching-Buffers.html)), to display the buffer returned by `last-buffer` (see above), in the selected window.

<!---->

*   Variable: **buffer-list-update-hook**

    This is a normal hook run whenever the buffer list changes. Functions (implicitly) running this hook are `get-buffer-create` (see [Creating Buffers](Creating-Buffers.html)), `rename-buffer` (see [Buffer Names](Buffer-Names.html)), `kill-buffer` (see [Killing Buffers](Killing-Buffers.html)), `bury-buffer` (see above) and `select-window` (see [Selecting Windows](Selecting-Windows.html)).

    Functions run by this hook should avoid calling `select-window` with a nil `norecord` argument or `with-temp-buffer` since either may lead to infinite recursion.

Next: [Creating Buffers](Creating-Buffers.html), Previous: [Read Only Buffers](Read-Only-Buffers.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
