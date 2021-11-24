

### 32.12 Margins for Filling

### User Option: **fill-prefix**

This buffer-local variable, if non-`nil`, specifies a string of text that appears at the beginning of normal text lines and should be disregarded when filling them. Any line that fails to start with the fill prefix is considered the start of a paragraph; so is any line that starts with the fill prefix followed by additional whitespace. Lines that start with the fill prefix but no additional whitespace are ordinary text lines that can be filled together. The resulting filled lines also start with the fill prefix.

The fill prefix follows the left margin whitespace, if any.

### User Option: **fill-column**

This buffer-local variable specifies the maximum width of filled lines. Its value should be an integer, which is a number of columns. All the filling, justification, and centering commands are affected by this variable, including Auto Fill mode (see [Auto Filling](Auto-Filling.html)).

As a practical matter, if you are writing text for other people to read, you should set `fill-column` to no more than 70. Otherwise the line will be too long for people to read comfortably, and this can make the text seem clumsy.

The default value for `fill-column` is 70.

### Command: **set-left-margin** *from to margin*

This sets the `left-margin` property on the text from `from` to `to` to the value `margin`. If Auto Fill mode is enabled, this command also refills the region to fit the new margin.

### Command: **set-right-margin** *from to margin*

This sets the `right-margin` property on the text from `from` to `to` to the value `margin`. If Auto Fill mode is enabled, this command also refills the region to fit the new margin.

### Function: **current-left-margin**

This function returns the proper left margin value to use for filling the text around point. The value is the sum of the `left-margin` property of the character at the start of the current line (or zero if none), and the value of the variable `left-margin`.

### Function: **current-fill-column**

This function returns the proper fill column value to use for filling the text around point. The value is the value of the `fill-column` variable, minus the value of the `right-margin` property of the character after point.

### Command: **move-to-left-margin** *\&optional n force*

This function moves point to the left margin of the current line. The column moved to is determined by calling the function `current-left-margin`. If the argument `n` is non-`nil`, `move-to-left-margin` moves forward `n`-1 lines first.

If `force` is non-`nil`, that says to fix the line’s indentation if that doesn’t match the left margin value.

### Function: **delete-to-left-margin** *\&optional from to*

This function removes left margin indentation from the text between `from` and `to`. The amount of indentation to delete is determined by calling `current-left-margin`. In no case does this function delete non-whitespace. If `from` and `to` are omitted, they default to the whole buffer.

### Function: **indent-to-left-margin**

This function adjusts the indentation at the beginning of the current line to the value specified by the variable `left-margin`. (That may involve either inserting or deleting whitespace.) This function is value of `indent-line-function` in Paragraph-Indent Text mode.

### User Option: **left-margin**

This variable specifies the base left margin column. In Fundamental mode, `RET` indents to this column. This variable automatically becomes buffer-local when set in any fashion.

### User Option: **fill-nobreak-predicate**

This variable gives major modes a way to specify not to break a line at certain places. Its value should be a list of functions. Whenever filling considers breaking the line at a certain place in the buffer, it calls each of these functions with no arguments and with point located at that place. If any of the functions returns non-`nil`, then the line won’t be broken there.
