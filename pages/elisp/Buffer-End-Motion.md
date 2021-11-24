

#### 30.2.3 Motion to an End of the Buffer

To move point to the beginning of the buffer, write:

```lisp
(goto-char (point-min))
```

Likewise, to move to the end of the buffer, use:

```lisp
(goto-char (point-max))
```

Here are two commands that users use to do these things. They are documented here to warn you not to use them in Lisp programs, because they set the mark and display messages in the echo area.

### Command: **beginning-of-buffer** *\&optional n*

This function moves point to the beginning of the buffer (or the limits of the accessible portion, when narrowing is in effect), setting the mark at the previous position (except in Transient Mark mode, if the mark is already active, it does not set the mark.)

If `n` is non-`nil`, then it puts point `n` tenths of the way from the beginning of the accessible portion of the buffer. In an interactive call, `n` is the numeric prefix argument, if provided; otherwise `n` defaults to `nil`.

**Warning:** Don’t use this function in Lisp programs!

### Command: **end-of-buffer** *\&optional n*

This function moves point to the end of the buffer (or the limits of the accessible portion, when narrowing is in effect), setting the mark at the previous position (except in Transient Mark mode when the mark is already active). If `n` is non-`nil`, then it puts point `n` tenths of the way from the end of the accessible portion of the buffer.

In an interactive call, `n` is the numeric prefix argument, if provided; otherwise `n` defaults to `nil`.

**Warning:** Don’t use this function in Lisp programs!
