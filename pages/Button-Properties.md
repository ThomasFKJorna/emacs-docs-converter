

Next: [Button Types](Button-Types.html), Up: [Buttons](Buttons.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.19.1 Button Properties

Each button has an associated list of properties defining its appearance and behavior, and other arbitrary properties may be used for application specific purposes. The following properties have special meaning to the Button package:

*   `action`

    The function to call when the user invokes the button, which is passed the single argument `button`. By default this is `ignore`, which does nothing.

*   `mouse-action`

    This is similar to `action`, and when present, will be used instead of `action` for button invocations resulting from mouse-clicks (instead of the user hitting `RET`). If not present, mouse-clicks use `action` instead.

*   `face`

    This is an Emacs face controlling how buttons of this type are displayed; by default this is the `button` face.

*   `mouse-face`

    This is an additional face which controls appearance during mouse-overs (merged with the usual button face); by default this is the usual Emacs `highlight` face.

*   `keymap`

    The button’s keymap, defining bindings active within the button region. By default this is the usual button region keymap, stored in the variable `button-map`, which defines `RET` and `mouse-2` to invoke the button.

*   `type`

    The button type. See [Button Types](Button-Types.html).

*   `help-echo`

    A string displayed by the Emacs tooltip help system; by default, `"mouse-2, RET: Push this button"`. Alternatively, a function that returns, or a form that evaluates to, a string to be displayed or `nil`. For details see [Text help-echo](Special-Properties.html#Text-help_002decho).

    The function is called with three arguments, `window`, `object`, and `pos`. The second argument, `object`, is either the overlay that had the property (for overlay buttons), or the buffer containing the button (for text property buttons). The other arguments have the same meaning as for the special text property `help-echo`.

*   `follow-link`

    The `follow-link` property, defining how a `mouse-1` click behaves on this button, See [Clickable Text](Clickable-Text.html).

*   `button`

    All buttons have a non-`nil` `button` property, which may be useful in finding regions of text that comprise buttons (which is what the standard button functions do).

There are other properties defined for the regions of text in a button, but these are not generally interesting for typical uses.

Next: [Button Types](Button-Types.html), Up: [Buttons](Buttons.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
