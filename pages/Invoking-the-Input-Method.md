

Next: [Quoted Character Input](Quoted-Character-Input.html), Previous: [Event Mod](Event-Mod.html), Up: [Reading Input](Reading-Input.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 21.8.4 Invoking the Input Method

The event-reading functions invoke the current input method, if any (see [Input Methods](Input-Methods.html)). If the value of `input-method-function` is non-`nil`, it should be a function; when `read-event` reads a printing character (including `SPC`) with no modifier bits, it calls that function, passing the character as an argument.

*   Variable: **input-method-function**

    If this is non-`nil`, its value specifies the current input method function.

    **Warning:** don’t bind this variable with `let`. It is often buffer-local, and if you bind it around reading input (which is exactly when you *would* bind it), switching buffers asynchronously while Emacs is waiting will cause the value to be restored in the wrong buffer.

The input method function should return a list of events which should be used as input. (If the list is `nil`, that means there is no input, so `read-event` waits for another event.) These events are processed before the events in `unread-command-events` (see [Event Input Misc](Event-Input-Misc.html)). Events returned by the input method function are not passed to the input method function again, even if they are printing characters with no modifier bits.

If the input method function calls `read-event` or `read-key-sequence`, it should bind `input-method-function` to `nil` first, to prevent recursion.

The input method function is not called when reading the second and subsequent events of a key sequence. Thus, these characters are not subject to input method processing. The input method function should test the values of `overriding-local-map` and `overriding-terminal-local-map`; if either of these variables is non-`nil`, the input method should put its argument into a list and return that list with no further processing.

Next: [Quoted Character Input](Quoted-Character-Input.html), Previous: [Event Mod](Event-Mod.html), Up: [Reading Input](Reading-Input.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
