

#### 23.2.3 Getting Help about a Major Mode

The `describe-mode` function provides information about major modes. It is normally bound to `C-h m`. It uses the value of the variable `major-mode` (see [Major Modes](Major-Modes.html)), which is why every major mode command needs to set that variable.

### Command: **describe-mode** *\&optional buffer*

This command displays the documentation of the current buffer’s major mode and minor modes. It uses the `documentation` function to retrieve the documentation strings of the major and minor mode commands (see [Accessing Documentation](Accessing-Documentation.html)).

If called from Lisp with a non-`nil` `buffer` argument, this function displays the documentation for that buffer’s major and minor modes, rather than those of the current buffer.
