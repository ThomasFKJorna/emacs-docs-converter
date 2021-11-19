

Next: [Abbrev Files](Abbrev-Files.html), Previous: [Abbrev Tables](Abbrev-Tables.html), Up: [Abbrevs](Abbrevs.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 36.2 Defining Abbrevs

`define-abbrev` is the low-level basic function for defining an abbrev in an abbrev table.

When a major mode defines a system abbrev, it should call `define-abbrev` and specify `t` for the `:system` property. Be aware that any saved non-system abbrevs are restored at startup, i.e., before some major modes are loaded. Therefore, major modes should not assume that their abbrev tables are empty when they are first loaded.

*   Function: **define-abbrev** *abbrev-table name expansion \&optional hook \&rest props*

    This function defines an abbrev named `name`, in `abbrev-table`, to expand to `expansion` and call `hook`, with properties `props` (see [Abbrev Properties](Abbrev-Properties.html)). The return value is `name`. The `:system` property in `props` is treated specially here: if it has the value `force`, then it will overwrite an existing definition even for a non-system abbrev of the same name.

    `name` should be a string. The argument `expansion` is normally the desired expansion (a string), or `nil` to undefine the abbrev. If it is anything but a string or `nil`, then the abbreviation expands solely by running `hook`.

    The argument `hook` is a function or `nil`. If `hook` is non-`nil`, then it is called with no arguments after the abbrev is replaced with `expansion`; point is located at the end of `expansion` when `hook` is called.

    If `hook` is a non-`nil` symbol whose `no-self-insert` property is non-`nil`, `hook` can explicitly control whether to insert the self-inserting input character that triggered the expansion. If `hook` returns non-`nil` in this case, that inhibits insertion of the character. By contrast, if `hook` returns `nil`, `expand-abbrev` (or `abbrev-insert`) also returns `nil`, as if expansion had not really occurred.

    Normally, `define-abbrev` sets the variable `abbrevs-changed` to `t`, if it actually changes the abbrev. This is so that some commands will offer to save the abbrevs. It does not do this for a system abbrev, since those aren’t saved anyway.

<!---->

*   User Option: **only-global-abbrevs**

    If this variable is non-`nil`, it means that the user plans to use global abbrevs only. This tells the commands that define mode-specific abbrevs to define global ones instead. This variable does not alter the behavior of the functions in this section; it is examined by their callers.

Next: [Abbrev Files](Abbrev-Files.html), Previous: [Abbrev Tables](Abbrev-Tables.html), Up: [Abbrevs](Abbrevs.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
