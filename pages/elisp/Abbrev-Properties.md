

### 36.6 Abbrev Properties

Abbrevs have properties, some of which influence the way they work. You can provide them as arguments to `define-abbrev`, and manipulate them with the following functions:

### Function: **abbrev-put** *abbrev prop val*

Set the property `prop` of `abbrev` to value `val`.

### Function: **abbrev-get** *abbrev prop*

Return the property `prop` of `abbrev`, or `nil` if the abbrev has no such property.

The following properties have special meanings:

### `:count`

This property counts the number of times the abbrev has been expanded. If not explicitly set, it is initialized to 0 by `define-abbrev`.

### `:system`

If non-`nil`, this property marks the abbrev as a system abbrev. Such abbrevs are not saved (see [Abbrev Files](Abbrev-Files.html)).

### `:enable-function`

If non-`nil`, this property should be a function of no arguments which returns `nil` if the abbrev should not be used and `t` otherwise.

### `:case-fixed`

If non-`nil`, this property indicates that the case of the abbrev’s name is significant and should only match a text with the same pattern of capitalization. It also disables the code that modifies the capitalization of the expansion.
