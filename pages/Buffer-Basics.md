

Next: [Current Buffer](Current-Buffer.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 27.1 Buffer Basics

A *buffer* is a Lisp object containing text to be edited. Buffers are used to hold the contents of files that are being visited; there may also be buffers that are not visiting files. Although several buffers normally exist, only one buffer is designated the *current buffer* at any time. Most editing commands act on the contents of the current buffer. Each buffer, including the current buffer, may or may not be displayed in any windows.

Buffers in Emacs editing are objects that have distinct names and hold text that can be edited. Buffers appear to Lisp programs as a special data type. You can think of the contents of a buffer as a string that you can extend; insertions and deletions may occur in any part of the buffer. See [Text](Text.html).

A Lisp buffer object contains numerous pieces of information. Some of this information is directly accessible to the programmer through variables, while other information is accessible only through special-purpose functions. For example, the visited file name is directly accessible through a variable, while the value of point is accessible only through a primitive function.

Buffer-specific information that is directly accessible is stored in *buffer-local* variable bindings, which are variable values that are effective only in a particular buffer. This feature allows each buffer to override the values of certain variables. Most major modes override variables such as `fill-column` or `comment-column` in this way. For more information about buffer-local variables and functions related to them, see [Buffer-Local Variables](Buffer_002dLocal-Variables.html).

For functions and variables related to visiting files in buffers, see [Visiting Files](Visiting-Files.html) and [Saving Buffers](Saving-Buffers.html). For functions and variables related to the display of buffers in windows, see [Buffers and Windows](Buffers-and-Windows.html).

*   Function: **bufferp** *object*

    This function returns `t` if `object` is a buffer, `nil` otherwise.

Next: [Current Buffer](Current-Buffer.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
