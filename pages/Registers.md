

Next: [Transposition](Transposition.html), Previous: [Substitution](Substitution.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.21 Registers

A register is a sort of variable used in Emacs editing that can hold a variety of different kinds of values. Each register is named by a single character. All ASCII characters and their meta variants (but with the exception of `C-g`) can be used to name registers. Thus, there are 255 possible registers. A register is designated in Emacs Lisp by the character that is its name.

*   Variable: **register-alist**

    This variable is an alist of elements of the form `(name . contents)`. Normally, there is one element for each Emacs register that has been used.

    The object `name` is a character (an integer) identifying the register.

The `contents` of a register can have several possible types:

*   a number

    A number stands for itself. If `insert-register` finds a number in the register, it converts the number to decimal.

*   a marker

    A marker represents a buffer position to jump to.

*   a string

    A string is text saved in the register.

*   a rectangle

    A rectangle is represented by a list of strings.

*   `(window-configuration position)`

    This represents a window configuration to restore in one frame, and a position to jump to in the current buffer.

*   `(frame-configuration position)`

    This represents a frame configuration to restore, and a position to jump to in the current buffer.

*   (file `filename`)

    This represents a file to visit; jumping to this value visits file `filename`.

*   (file-query `filename` `position`)

    This represents a file to visit and a position in it; jumping to this value visits file `filename` and goes to buffer position `position`. Restoring this type of position asks the user for confirmation first.

The functions in this section return unpredictable values unless otherwise stated.

*   Function: **get-register** *reg*

    This function returns the contents of the register `reg`, or `nil` if it has no contents.

<!---->

*   Function: **set-register** *reg value*

    This function sets the contents of register `reg` to `value`. A register can be set to any value, but the other register functions expect only certain data types. The return value is `value`.

<!---->

*   Command: **view-register** *reg*

    This command displays what is contained in register `reg`.

<!---->

*   Command: **insert-register** *reg \&optional beforep*

    This command inserts contents of register `reg` into the current buffer.

    Normally, this command puts point before the inserted text, and the mark after it. However, if the optional second argument `beforep` is non-`nil`, it puts the mark before and point after.

    When called interactively, the command defaults to putting point after text, and a prefix argument inverts this behavior.

    If the register contains a rectangle, then the rectangle is inserted with its upper left corner at point. This means that text is inserted in the current line and underneath it on successive lines.

    If the register contains something other than saved text (a string) or a rectangle (a list), currently useless things happen. This may be changed in the future.

<!---->

*   Function: **register-read-with-preview** *prompt*

    This function reads and returns a register name, prompting with `prompt` and possibly showing a preview of the existing registers and their contents. The preview is shown in a temporary window, after the delay specified by the user option `register-preview-delay`, if its value and `register-alist` are both non-`nil`. The preview is also shown if the user requests help (e.g., by typing the help character). We recommend that all interactive commands which read register names use this function.

Next: [Transposition](Transposition.html), Previous: [Substitution](Substitution.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
