

Next: [Warnings](Warnings.html), Previous: [Truncation](Truncation.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.4 The Echo Area

The *echo area* is used for displaying error messages (see [Errors](Errors.html)), for messages made with the `message` primitive, and for echoing keystrokes. It is not the same as the minibuffer, despite the fact that the minibuffer appears (when active) in the same place on the screen as the echo area. See [The Minibuffer](https://www.gnu.org/software/emacs/manual/html_node/emacs/Minibuffer.html#Minibuffer) in The GNU Emacs Manual.

Apart from the functions documented in this section, you can print Lisp objects to the echo area by specifying `t` as the output stream. See [Output Streams](Output-Streams.html).

|                                                           |    |                                                    |
| :-------------------------------------------------------- | -- | :------------------------------------------------- |
| • [Displaying Messages](Displaying-Messages.html)         |    | Explicitly displaying text in the echo area.       |
| • [Progress](Progress.html)                               |    | Informing user about progress of a long operation. |
| • [Logging Messages](Logging-Messages.html)               |    | Echo area messages are logged for the user.        |
| • [Echo Area Customization](Echo-Area-Customization.html) |    | Controlling the echo area.                         |
