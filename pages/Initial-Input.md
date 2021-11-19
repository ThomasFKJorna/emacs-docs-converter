

Next: [Completion](Completion.html), Previous: [Minibuffer History](Minibuffer-History.html), Up: [Minibuffers](Minibuffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 20.5 Initial Input

Several of the functions for minibuffer input have an argument called `initial`. This is a mostly-deprecated feature for specifying that the minibuffer should start out with certain text, instead of empty as usual.

If `initial` is a string, the minibuffer starts out containing the text of the string, with point at the end, when the user starts to edit the text. If the user simply types `RET` to exit the minibuffer, it will use the initial input string to determine the value to return.

**We discourage use of a non-`nil` value for `initial`**, because initial input is an intrusive interface. History lists and default values provide a much more convenient method to offer useful default inputs to the user.

There is just one situation where you should specify a string for an `initial` argument. This is when you specify a cons cell for the `history` argument. See [Minibuffer History](Minibuffer-History.html).

`initial` can also be a cons cell of the form `(string . position)`. This means to insert `string` in the minibuffer but put point at `position` within the string’s text.

As a historical accident, `position` was implemented inconsistently in different functions. In `completing-read`, `position`’s value is interpreted as origin-zero; that is, a value of 0 means the beginning of the string, 1 means after the first character, etc. In `read-minibuffer`, and the other non-completion minibuffer input functions that support this argument, 1 means the beginning of the string, 2 means after the first character, etc.

Use of a cons cell as the value for `initial` arguments is deprecated.

Next: [Completion](Completion.html), Previous: [Minibuffer History](Minibuffer-History.html), Up: [Minibuffers](Minibuffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
