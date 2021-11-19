

Previous: [Other Display Specs](Other-Display-Specs.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.16.5 Displaying in the Margins

A buffer can have blank areas called *display margins* on the left and on the right. Ordinary text never appears in these areas, but you can put things into the display margins using the `display` property. There is currently no way to make text or images in the margin mouse-sensitive.

The way to display something in the margins is to specify it in a margin display specification in the `display` property of some text. This is a replacing display specification, meaning that the text you put it on does not get displayed; the margin display appears, but that text does not.

A margin display specification looks like `((margin right-margin) spec)` or `((margin left-margin) spec)`. Here, `spec` is another display specification that says what to display in the margin. Typically it is a string of text to display, or an image descriptor.

To display something in the margin *in association with* certain buffer text, without altering or preventing the display of that text, put a `before-string` property on the text and put the margin display specification on the contents of the before-string.

Note that if the string to be displayed in the margin doesn’t specify a face, its face is determined using the same rules and priorities as it is for strings displayed in the text area (see [Displaying Faces](Displaying-Faces.html)). If this results in undesirable “leaking” of faces into the margin, make sure the string has an explicit face specified for it.

Before the display margins can display anything, you must give them a nonzero width. The usual way to do that is to set these variables:

*   Variable: **left-margin-width**

    This variable specifies the width of the left margin, in character cell (a.k.a. “column”) units. It is buffer-local in all buffers. A value of `nil` means no left marginal area.

<!---->

*   Variable: **right-margin-width**

    This variable specifies the width of the right margin, in character cell units. It is buffer-local in all buffers. A value of `nil` means no right marginal area.

Setting these variables does not immediately affect the window. These variables are checked when a new buffer is displayed in the window. Thus, you can make changes take effect by calling `set-window-buffer`. Do not use these variables to try to determine the current width of the left or right margin. Instead, use the function `window-margins`.

You can also set the margin widths immediately.

*   Function: **set-window-margins** *window left \&optional right*

    This function specifies the margin widths for window `window`, in character cell units. The argument `left` controls the left margin, and `right` controls the right margin (default `0`).

    If `window` is not large enough to accommodate margins of the desired width, this leaves the margins of `window` unchanged.

    The values specified here may be later overridden by invoking `set-window-buffer` (see [Buffers and Windows](Buffers-and-Windows.html)) on `window` with its `keep-margins` argument `nil` or omitted.

<!---->

*   Function: **window-margins** *\&optional window*

    This function returns the width of the left and right margins of `window` as a cons cell of the form `(left . right)`. If one of the two marginal areas does not exist, its width is returned as `nil`; if neither of the two margins exist, the function returns `(nil)`. If `window` is `nil`, the selected window is used.

Previous: [Other Display Specs](Other-Display-Specs.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
