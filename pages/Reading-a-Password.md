

Next: [Minibuffer Commands](Minibuffer-Commands.html), Previous: [Multiple Queries](Multiple-Queries.html), Up: [Minibuffers](Minibuffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 20.9 Reading a Password

To read a password to pass to another program, you can use the function `read-passwd`.

*   Function: **read-passwd** *prompt \&optional confirm default*

    This function reads a password, prompting with `prompt`. It does not echo the password as the user types it; instead, it echoes ‘`*`’ for each character in the password. If you want to apply another character to hide the password, let-bind the variable `read-hide-char` with that character.

    The optional argument `confirm`, if non-`nil`, says to read the password twice and insist it must be the same both times. If it isn’t the same, the user has to type it over and over until the last two times match.

    The optional argument `default` specifies the default password to return if the user enters empty input. If `default` is `nil`, then `read-passwd` returns the null string in that case.
