

Next: [Defining New Types](Defining-New-Types.html), Previous: [Splicing into Lists](Splicing-into-Lists.html), Up: [Customization Types](Customization-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 15.4.4 Type Keywords

You can specify keyword-argument pairs in a customization type after the type name symbol. Here are the keywords you can use, and their meanings:

*   `:value default`

    Provide a default value.

    If `nil` is not a valid value for the alternative, then it is essential to specify a valid default with `:value`.

    If you use this for a type that appears as an alternative inside of `choice`; it specifies the default value to use, at first, if and when the user selects this alternative with the menu in the customization buffer.

    Of course, if the actual value of the option fits this alternative, it will appear showing the actual value, not `default`.

*   `:format format-string`

    This string will be inserted in the buffer to represent the value corresponding to the type. The following ‘`%`’ escapes are available for use in `format-string`:

    *   ‘`%[button%]`’

        Display the text `button` marked as a button. The `:action` attribute specifies what the button will do if the user invokes it; its value is a function which takes two arguments—the widget which the button appears in, and the event.

        There is no way to specify two different buttons with different actions.

    *   ‘`%{sample%}`’

        Show `sample` in a special face specified by `:sample-face`.

    *   ‘`%v`’

        Substitute the item’s value. How the value is represented depends on the kind of item, and (for variables) on the customization type.

    *   ‘`%d`’

        Substitute the item’s documentation string.

    *   ‘`%h`’

        Like ‘`%d`’, but if the documentation string is more than one line, add a button to control whether to show all of it or just the first line.

    *   ‘`%t`’

        Substitute the tag here. You specify the tag with the `:tag` keyword.

    *   ‘`%%`’

        Display a literal ‘`%`’.

*   `:action action`

    Perform `action` if the user clicks on a button.

*   `:button-face face`

    Use the face `face` (a face name or a list of face names) for button text displayed with ‘`%[…%]`’.

*   *   `:button-prefix prefix`
    *   `:button-suffix suffix`

    These specify the text to display before and after a button. Each can be:

    *   `nil`

        No text is inserted.

    *   a string

        The string is inserted literally.

    *   a symbol

        The symbol’s value is used.

*   `:tag tag`

    Use `tag` (a string) as the tag for the value (or part of the value) that corresponds to this type.

*   `:doc doc`

    Use `doc` as the documentation string for this value (or part of the value) that corresponds to this type. In order for this to work, you must specify a value for `:format`, and use ‘`%d`’ or ‘`%h`’ in that value.

    The usual reason to specify a documentation string for a type is to provide more information about the meanings of alternatives inside a `choice` type or the parts of some other composite type.

*   `:help-echo motion-doc`

    When you move to this item with `widget-forward` or `widget-backward`, it will display the string `motion-doc` in the echo area. In addition, `motion-doc` is used as the mouse `help-echo` string and may actually be a function or form evaluated to yield a help string. If it is a function, it is called with one argument, the widget.

*   `:match function`

    Specify how to decide whether a value matches the type. The corresponding value, `function`, should be a function that accepts two arguments, a widget and a value; it should return non-`nil` if the value is acceptable.

*   `:match-inline function`

    Specify how to decide whether an inline value matches the type. The corresponding value, `function`, should be a function that accepts two arguments, a widget and an inline value; it should return non-`nil` if the value is acceptable. See [Splicing into Lists](Splicing-into-Lists.html) for more information about inline values.

*   `:validate function`

    Specify a validation function for input. `function` takes a widget as an argument, and should return `nil` if the widget’s current value is valid for the widget. Otherwise, it should return the widget containing the invalid data, and set that widget’s `:error` property to a string explaining the error.

Next: [Defining New Types](Defining-New-Types.html), Previous: [Splicing into Lists](Splicing-into-Lists.html), Up: [Customization Types](Customization-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
