

Next: [Warning Options](Warning-Options.html), Previous: [Warning Basics](Warning-Basics.html), Up: [Warnings](Warnings.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.5.2 Warning Variables

Programs can customize how their warnings appear by binding the variables described in this section.

*   Variable: **warning-levels**

    This list defines the meaning and severity order of the warning severity levels. Each element defines one severity level, and they are arranged in order of decreasing severity.

    Each element has the form `(level string function)`, where `level` is the severity level it defines. `string` specifies the textual description of this level. `string` should use ‘`%s`’ to specify where to put the warning type information, or it can omit the ‘`%s`’ so as not to include that information.

    The optional `function`, if non-`nil`, is a function to call with no arguments, to get the user’s attention.

    Normally you should not change the value of this variable.

<!---->

*   Variable: **warning-prefix-function**

    If non-`nil`, the value is a function to generate prefix text for warnings. Programs can bind the variable to a suitable function. `display-warning` calls this function with the warnings buffer current, and the function can insert text in it. That text becomes the beginning of the warning message.

    The function is called with two arguments, the severity level and its entry in `warning-levels`. It should return a list to use as the entry (this value need not be an actual member of `warning-levels`). By constructing this value, the function can change the severity of the warning, or specify different handling for a given severity level.

    If the variable’s value is `nil` then there is no function to call.

<!---->

*   Variable: **warning-series**

    Programs can bind this variable to `t` to say that the next warning should begin a series. When several warnings form a series, that means to leave point on the first warning of the series, rather than keep moving it for each warning so that it appears on the last one. The series ends when the local binding is unbound and `warning-series` becomes `nil` again.

    The value can also be a symbol with a function definition. That is equivalent to `t`, except that the next warning will also call the function with no arguments with the warnings buffer current. The function can insert text which will serve as a header for the series of warnings.

    Once a series has begun, the value is a marker which points to the buffer position in the warnings buffer of the start of the series.

    The variable’s normal value is `nil`, which means to handle each warning separately.

<!---->

*   Variable: **warning-fill-prefix**

    When this variable is non-`nil`, it specifies a fill prefix to use for filling each warning’s text.

<!---->

*   Variable: **warning-fill-column**

    The column at which to fill warnings.

<!---->

*   Variable: **warning-type-format**

    This variable specifies the format for displaying the warning type in the warning message. The result of formatting the type this way gets included in the message under the control of the string in the entry in `warning-levels`. The default value is `" (%s)"`. If you bind it to `""` then the warning type won’t appear at all.

Next: [Warning Options](Warning-Options.html), Previous: [Warning Basics](Warning-Basics.html), Up: [Warnings](Warnings.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
