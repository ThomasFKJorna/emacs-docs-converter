

Next: [Defining Abbrevs](Defining-Abbrevs.html), Up: [Abbrevs](Abbrevs.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 36.1 Abbrev Tables

This section describes how to create and manipulate abbrev tables.

*   Function: **make-abbrev-table** *\&optional props*

    This function creates and returns a new, empty abbrev table—an obarray containing no symbols. It is a vector filled with zeros. `props` is a property list that is applied to the new table (see [Abbrev Table Properties](Abbrev-Table-Properties.html)).

<!---->

*   Function: **abbrev-table-p** *object*

    This function returns a non-`nil` value if `object` is an abbrev table.

<!---->

*   Function: **clear-abbrev-table** *abbrev-table*

    This function undefines all the abbrevs in `abbrev-table`, leaving it empty.

<!---->

*   Function: **copy-abbrev-table** *abbrev-table*

    This function returns a copy of `abbrev-table`—a new abbrev table containing the same abbrev definitions. It does *not* copy any property lists; only the names, values, and functions.

<!---->

*   Function: **define-abbrev-table** *tabname definitions \&optional docstring \&rest props*

    This function defines `tabname` (a symbol) as an abbrev table name, i.e., as a variable whose value is an abbrev table. It defines abbrevs in the table according to `definitions`, a list of elements of the form `(abbrevname expansion [hook] [props...])`. These elements are passed as arguments to `define-abbrev`.

    The optional string `docstring` is the documentation string of the variable `tabname`. The property list `props` is applied to the abbrev table (see [Abbrev Table Properties](Abbrev-Table-Properties.html)).

    If this function is called more than once for the same `tabname`, subsequent calls add the definitions in `definitions` to `tabname`, rather than overwriting the entire original contents. (A subsequent call only overrides abbrevs explicitly redefined or undefined in `definitions`.)

<!---->

*   Variable: **abbrev-table-name-list**

    This is a list of symbols whose values are abbrev tables. `define-abbrev-table` adds the new abbrev table name to this list.

<!---->

*   Function: **insert-abbrev-table-description** *name \&optional human*

    This function inserts before point a description of the abbrev table named `name`. The argument `name` is a symbol whose value is an abbrev table.

    If `human` is non-`nil`, the description is human-oriented. System abbrevs are listed and identified as such. Otherwise the description is a Lisp expression—a call to `define-abbrev-table` that would define `name` as it is currently defined, but without the system abbrevs. (The mode or package using `name` is supposed to add these to `name` separately.)

Next: [Defining Abbrevs](Defining-Abbrevs.html), Up: [Abbrevs](Abbrevs.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
