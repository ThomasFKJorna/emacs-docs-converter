

#### 21.7.6 Button-Down Events

Click and drag events happen when the user releases a mouse button. They cannot happen earlier, because there is no way to distinguish a click from a drag until the button is released.

If you want to take action as soon as a button is pressed, you need to handle *button-down* events.[13](#FOOT13) These occur as soon as a button is pressed. They are represented by lists that look exactly like click events (see [Click Events](Click-Events.html)), except that the `event-type` symbol name contains the prefix ‘`down-`’. The ‘`down-`’ prefix follows modifier key prefixes such as ‘`C-`’ and ‘`M-`’.

The function `read-key-sequence` ignores any button-down events that don’t have command bindings; therefore, the Emacs command loop ignores them too. This means that you need not worry about defining button-down events unless you want them to do something. The usual reason to define a button-down event is so that you can track mouse motion (by reading motion events) until the button is released. See [Motion Events](Motion-Events.html).

***

#### Footnotes

##### [(13)](#DOCF13)

Button-down is the conservative antithesis of drag.
