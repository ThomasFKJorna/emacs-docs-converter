<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Progress](Progress.html), Up: [The Echo Area](The-Echo-Area.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.4.1 Displaying Messages in the Echo Area

This section describes the standard functions for displaying messages in the echo area.

*   Function: **message** *format-string \&rest arguments*

    This function displays a message in the echo area. `format-string` is a format string, and `arguments` are the objects for its format specifications, like in the `format-message` function (see [Formatting Strings](Formatting-Strings.html)). The resulting formatted string is displayed in the echo area; if it contains `face` text properties, it is displayed with the specified faces (see [Faces](Faces.html)). The string is also added to the `*Messages*` buffer, but without text properties (see [Logging Messages](Logging-Messages.html)).

    Typically grave accent and apostrophe in the format translate to matching curved quotes, e.g., ``"Missing `%s'"`` might result in `"Missing ‘foo’"`. See [Text Quoting Style](Text-Quoting-Style.html), for how to influence or inhibit this translation.

    In batch mode, the message is printed to the standard error stream, followed by a newline.

    When `inhibit-message` is non-`nil`, no message will be displayed in the echo area, it will only be logged to ‘`*Messages*`’.

    If `format-string` is `nil` or the empty string, `message` clears the echo area; if the echo area has been expanded automatically, this brings it back to its normal size. If the minibuffer is active, this brings the minibuffer contents back onto the screen immediately.

        (message "Reverting `%s'..." (buffer-name))
         -| Reverting ‘subr.el’...
        ⇒ "Reverting ‘subr.el’..."

    ```
    ```

        ---------- Echo Area ----------
        Reverting ‘subr.el’...
        ---------- Echo Area ----------

    To automatically display a message in the echo area or in a pop-buffer, depending on its size, use `display-message-or-buffer` (see below).

    **Warning:** If you want to use your own string as a message verbatim, don’t just write `(message string)`. If `string` contains ‘`%`’, ‘`` ` ``’, or ‘`'`’ it may be reformatted, with undesirable results. Instead, use `(message "%s" string)`.

<!---->

*   Variable: **set-message-function**

    If this variable is non-`nil`, it should be a function of one argument, the text of a message to display in the echo area. This function will be called by `message` and related functions. If the function returns `nil`, the message is displayed in the echo area as usual. If this function returns a string, that string is displayed in the echo area instead of the original one. If this function returns other non-`nil` values, that means the message was already handled, so `message` will not display anything in the echo area. See also `clear-message-function` that can be used to clear the message displayed by this function.

    The default value is the function that displays the message at the end of the minibuffer when the minibuffer is active. However, if the text shown in the active minibuffer has the `minibuffer-message` text property (see [Special Properties](Special-Properties.html)) on some character, the message will be displayed before the first character having that property.

<!---->

*   Variable: **clear-message-function**

    If this variable is non-`nil`, `message` and related functions call it with no arguments when their argument message is `nil` or the empty string.

    Usually this function is called when the next input event arrives after displaying an echo-area message. The function is expected to clear the message displayed by its counterpart function specified by `set-message-function`.

    The default value is the function that clears the message displayed in an active minibuffer.

<!---->

*   Variable: **inhibit-message**

    When this variable is non-`nil`, `message` and related functions will not use the Echo Area to display messages.

<!---->

*   Macro: **with-temp-message** *message \&rest body*

    This construct displays a message in the echo area temporarily, during the execution of `body`. It displays `message`, executes `body`, then returns the value of the last body form while restoring the previous echo area contents.

<!---->

*   Function: **message-or-box** *format-string \&rest arguments*

    This function displays a message like `message`, but may display it in a dialog box instead of the echo area. If this function is called in a command that was invoked using the mouse—more precisely, if `last-nonmenu-event` (see [Command Loop Info](Command-Loop-Info.html)) is either `nil` or a list—then it uses a dialog box or pop-up menu to display the message. Otherwise, it uses the echo area. (This is the same criterion that `y-or-n-p` uses to make a similar decision; see [Yes-or-No Queries](Yes_002dor_002dNo-Queries.html).)

    You can force use of the mouse or of the echo area by binding `last-nonmenu-event` to a suitable value around the call.

<!---->

*   Function: **message-box** *format-string \&rest arguments*

    This function displays a message like `message`, but uses a dialog box (or a pop-up menu) whenever that is possible. If it is impossible to use a dialog box or pop-up menu, because the terminal does not support them, then `message-box` uses the echo area, like `message`.

<!---->

*   Function: **display-message-or-buffer** *message \&optional buffer-name action frame*

    This function displays the message `message`, which may be either a string or a buffer. If it is shorter than the maximum height of the echo area, as defined by `max-mini-window-height`, it is displayed in the echo area, using `message`. Otherwise, `display-buffer` is used to show it in a pop-up buffer.

    Returns either the string shown in the echo area, or when a pop-up buffer is used, the window used to display it.

    If `message` is a string, then the optional argument `buffer-name` is the name of the buffer used to display it when a pop-up buffer is used, defaulting to `*Message*`. In the case where `message` is a string and displayed in the echo area, it is not specified whether the contents are inserted into the buffer anyway.

    The optional arguments `action` and `frame` are as for `display-buffer`, and only used if a buffer is displayed.

<!---->

*   Function: **current-message**

    This function returns the message currently being displayed in the echo area, or `nil` if there is none.

Next: [Progress](Progress.html), Up: [The Echo Area](The-Echo-Area.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
