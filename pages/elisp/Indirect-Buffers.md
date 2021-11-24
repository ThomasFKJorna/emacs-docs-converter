

### 27.11 Indirect Buffers

An *indirect buffer* shares the text of some other buffer, which is called the *base buffer* of the indirect buffer. In some ways it is the analogue, for buffers, of a symbolic link among files. The base buffer may not itself be an indirect buffer.

The text of the indirect buffer is always identical to the text of its base buffer; changes made by editing either one are visible immediately in the other. This includes the text properties as well as the characters themselves.

In all other respects, the indirect buffer and its base buffer are completely separate. They have different names, independent values of point, independent narrowing, independent markers and overlays (though inserting or deleting text in either buffer relocates the markers and overlays for both), independent major modes, and independent buffer-local variable bindings.

An indirect buffer cannot visit a file, but its base buffer can. If you try to save the indirect buffer, that actually saves the base buffer.

Killing an indirect buffer has no effect on its base buffer. Killing the base buffer effectively kills the indirect buffer in that it cannot ever again be the current buffer.

### Command: **make-indirect-buffer** *base-buffer name \&optional clone*

This creates and returns an indirect buffer named `name` whose base buffer is `base-buffer`. The argument `base-buffer` may be a live buffer or the name (a string) of an existing buffer. If `name` is the name of an existing buffer, an error is signaled.

If `clone` is non-`nil`, then the indirect buffer originally shares the state of `base-buffer` such as major mode, minor modes, buffer local variables and so on. If `clone` is omitted or `nil` the indirect buffer’s state is set to the default state for new buffers.

If `base-buffer` is an indirect buffer, its base buffer is used as the base for the new buffer. If, in addition, `clone` is non-`nil`, the initial state is copied from the actual base buffer, not from `base-buffer`.

### Command: **clone-indirect-buffer** *newname display-flag \&optional norecord*

This function creates and returns a new indirect buffer that shares the current buffer’s base buffer and copies the rest of the current buffer’s attributes. (If the current buffer is not indirect, it is used as the base buffer.)

If `display-flag` is non-`nil`, as it always is in interactive calls, that means to display the new buffer by calling `pop-to-buffer`. If `norecord` is non-`nil`, that means not to put the new buffer to the front of the buffer list.

### Function: **buffer-base-buffer** *\&optional buffer*

This function returns the base buffer of `buffer`, which defaults to the current buffer. If `buffer` is not indirect, the value is `nil`. Otherwise, the value is another buffer, which is never an indirect buffer.
