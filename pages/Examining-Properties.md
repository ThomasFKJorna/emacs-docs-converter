

Next: [Changing Properties](Changing-Properties.html), Up: [Text Properties](Text-Properties.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 32.19.1 Examining Text Properties

The simplest way to examine text properties is to ask for the value of a particular property of a particular character. For that, use `get-text-property`. Use `text-properties-at` to get the entire property list of a character. See [Property Search](Property-Search.html), for functions to examine the properties of a number of characters at once.

These functions handle both strings and buffers. Keep in mind that positions in a string start from 0, whereas positions in a buffer start from 1. Passing a buffer other than the current buffer may be slow.

*   Function: **get-text-property** *pos prop \&optional object*

    This function returns the value of the `prop` property of the character after position `pos` in `object` (a buffer or string). The argument `object` is optional and defaults to the current buffer.

    If there is no `prop` property strictly speaking, but the character has a property category that is a symbol, then `get-text-property` returns the `prop` property of that symbol.

<!---->

*   Function: **get-char-property** *position prop \&optional object*

    This function is like `get-text-property`, except that it checks overlays first and then text properties. See [Overlays](Overlays.html).

    The argument `object` may be a string, a buffer, or a window. If it is a window, then the buffer displayed in that window is used for text properties and overlays, but only the overlays active for that window are considered. If `object` is a buffer, then overlays in that buffer are considered first, in order of decreasing priority, followed by the text properties. If `object` is a string, only text properties are considered, since strings never have overlays.

<!---->

*   Function: **get-pos-property** *position prop \&optional object*

    This function is like `get-char-property`, except that it pays attention to properties’ stickiness and overlays’ advancement settings instead of the property of the character at (i.e., right after) `position`.

<!---->

*   Function: **get-char-property-and-overlay** *position prop \&optional object*

    This is like `get-char-property`, but gives extra information about the overlay that the property value comes from.

    Its value is a cons cell whose CAR is the property value, the same value `get-char-property` would return with the same arguments. Its CDR is the overlay in which the property was found, or `nil`, if it was found as a text property or not found at all.

    If `position` is at the end of `object`, both the CAR and the CDR of the value are `nil`.

<!---->

*   Variable: **char-property-alias-alist**

    This variable holds an alist which maps property names to a list of alternative property names. If a character does not specify a direct value for a property, the alternative property names are consulted in order; the first non-`nil` value is used. This variable takes precedence over `default-text-properties`, and `category` properties take precedence over this variable.

<!---->

*   Function: **text-properties-at** *position \&optional object*

    This function returns the entire property list of the character at `position` in the string or buffer `object`. If `object` is `nil`, it defaults to the current buffer.

<!---->

*   Variable: **default-text-properties**

    This variable holds a property list giving default values for text properties. Whenever a character does not specify a value for a property, neither directly, through a category symbol, or through `char-property-alias-alist`, the value stored in this list is used instead. Here is an example:

    ```lisp
    (setq default-text-properties '(foo 69)
          char-property-alias-alist nil)
    ;; Make sure character 1 has no properties of its own.
    (set-text-properties 1 2 nil)
    ;; What we get, when we ask, is the default value.
    (get-text-property 1 'foo)
         ⇒ 69
    ```

Next: [Changing Properties](Changing-Properties.html), Up: [Text Properties](Text-Properties.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
