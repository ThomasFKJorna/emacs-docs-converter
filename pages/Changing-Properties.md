

Next: [Property Search](Property-Search.html), Previous: [Examining Properties](Examining-Properties.html), Up: [Text Properties](Text-Properties.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 32.19.2 Changing Text Properties

The primitives for changing properties apply to a specified range of text in a buffer or string. The function `set-text-properties` (see end of section) sets the entire property list of the text in that range; more often, it is useful to add, change, or delete just certain properties specified by name.

Since text properties are considered part of the contents of the buffer (or string), and can affect how a buffer looks on the screen, any change in buffer text properties marks the buffer as modified. Buffer text property changes are undoable also (see [Undo](Undo.html)). Positions in a string start from 0, whereas positions in a buffer start from 1.

*   Function: **put-text-property** *start end prop value \&optional object*

    This function sets the `prop` property to `value` for the text between `start` and `end` in the string or buffer `object`. If `object` is `nil`, it defaults to the current buffer.

<!---->

*   Function: **add-text-properties** *start end props \&optional object*

    This function adds or overrides text properties for the text between `start` and `end` in the string or buffer `object`. If `object` is `nil`, it defaults to the current buffer.

    The argument `props` specifies which properties to add. It should have the form of a property list (see [Property Lists](Property-Lists.html)): a list whose elements include the property names followed alternately by the corresponding values.

    The return value is `t` if the function actually changed some property’s value; `nil` otherwise (if `props` is `nil` or its values agree with those in the text).

    For example, here is how to set the `comment` and `face` properties of a range of text:

    ```lisp
    (add-text-properties start end
                         '(comment t face highlight))
    ```

<!---->

*   Function: **remove-text-properties** *start end props \&optional object*

    This function deletes specified text properties from the text between `start` and `end` in the string or buffer `object`. If `object` is `nil`, it defaults to the current buffer.

    The argument `props` specifies which properties to delete. It should have the form of a property list (see [Property Lists](Property-Lists.html)): a list whose elements are property names alternating with corresponding values. But only the names matter—the values that accompany them are ignored. For example, here’s how to remove the `face` property.

    ```lisp
    (remove-text-properties start end '(face nil))
    ```

    The return value is `t` if the function actually changed some property’s value; `nil` otherwise (if `props` is `nil` or if no character in the specified text had any of those properties).

    To remove all text properties from certain text, use `set-text-properties` and specify `nil` for the new property list.

<!---->

*   Function: **remove-list-of-text-properties** *start end list-of-properties \&optional object*

    Like `remove-text-properties` except that `list-of-properties` is a list of property names only, not an alternating list of property names and values.

<!---->

*   Function: **set-text-properties** *start end props \&optional object*

    This function completely replaces the text property list for the text between `start` and `end` in the string or buffer `object`. If `object` is `nil`, it defaults to the current buffer.

    The argument `props` is the new property list. It should be a list whose elements are property names alternating with corresponding values.

    After `set-text-properties` returns, all the characters in the specified range have identical properties.

    If `props` is `nil`, the effect is to get rid of all properties from the specified range of text. Here’s an example:

    ```lisp
    (set-text-properties start end nil)
    ```

    Do not rely on the return value of this function.

<!---->

*   Function: **add-face-text-property** *start end face \&optional appendp object*

    This function acts on the text between `start` and `end`, adding the face `face` to the `face` text property. `face` should be a valid value for the `face` property (see [Special Properties](Special-Properties.html)), such as a face name or an anonymous face (see [Faces](Faces.html)).

    If any text in the region already has a non-`nil` `face` property, those face(s) are retained. This function sets the `face` property to a list of faces, with `face` as the first element (by default) and the pre-existing faces as the remaining elements. If the optional argument `appendp` is non-`nil`, `face` is appended to the end of the list instead. Note that in a face list, the first occurring value for each attribute takes precedence.

    For example, the following code would assign an italicized green face to the text between `start` and `end`:

    ```lisp
    (add-face-text-property start end 'italic)
    (add-face-text-property start end '(:foreground "red"))
    (add-face-text-property start end '(:foreground "green"))
    ```

    The optional argument `object`, if non-`nil`, specifies a buffer or string to act on, rather than the current buffer. If `object` is a string, then `start` and `end` are zero-based indices into the string.

The easiest way to make a string with text properties is with `propertize`:

*   Function: **propertize** *string \&rest properties*

    This function returns a copy of `string` with the text properties `properties` added. These properties apply to all the characters in the string that is returned. Here is an example that constructs a string with a `face` property and a `mouse-face` property:

    ```lisp
    (propertize "foo" 'face 'italic
                'mouse-face 'bold-italic)
         ⇒ #("foo" 0 3 (mouse-face bold-italic face italic))
    ```

    To put different properties on various parts of a string, you can construct each part with `propertize` and then combine them with `concat`:

    ```lisp
    (concat
     (propertize "foo" 'face 'italic
                 'mouse-face 'bold-italic)
     " and "
     (propertize "bar" 'face 'italic
                 'mouse-face 'bold-italic))
         ⇒ #("foo and bar"
                     0 3 (face italic mouse-face bold-italic)
                     3 8 nil
                     8 11 (face italic mouse-face bold-italic))
    ```

See [Buffer Contents](Buffer-Contents.html), for the function `buffer-substring-no-properties`, which copies text from the buffer but does not copy its properties.

If you wish to add text properties to a buffer or remove them without marking the buffer as modified, you can wrap the calls above in the `with-silent-modifications` macro. See [Buffer Modification](Buffer-Modification.html).

Next: [Property Search](Property-Search.html), Previous: [Examining Properties](Examining-Properties.html), Up: [Text Properties](Text-Properties.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
