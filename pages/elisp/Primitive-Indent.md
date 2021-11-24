

#### 32.17.1 Indentation Primitives

This section describes the primitive functions used to count and insert indentation. The functions in the following sections use these primitives. See [Size of Displayed Text](Size-of-Displayed-Text.html), for related functions.

### Function: **current-indentation**

This function returns the indentation of the current line, which is the horizontal position of the first nonblank character. If the contents are entirely blank, then this is the horizontal position of the end of the line.

### Command: **indent-to** *column \&optional minimum*

This function indents from point with tabs and spaces until `column` is reached. If `minimum` is specified and non-`nil`, then at least that many spaces are inserted even if this requires going beyond `column`. Otherwise the function does nothing if point is already beyond `column`. The value is the column at which the inserted indentation ends.

The inserted whitespace characters inherit text properties from the surrounding text (usually, from the preceding text only). See [Sticky Properties](Sticky-Properties.html).

### User Option: **indent-tabs-mode**

If this variable is non-`nil`, indentation functions can insert tabs as well as spaces. Otherwise, they insert only spaces. Setting this variable automatically makes it buffer-local in the current buffer.
