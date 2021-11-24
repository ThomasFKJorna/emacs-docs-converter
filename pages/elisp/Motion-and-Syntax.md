

### 35.5 Motion and Syntax

This section describes functions for moving across characters that have certain syntax classes.

### Function: **skip-syntax-forward** *syntaxes \&optional limit*

This function moves point forward across characters having syntax classes mentioned in `syntaxes` (a string of syntax class characters). It stops when it encounters the end of the buffer, or position `limit` (if specified), or a character it is not supposed to skip.

If `syntaxes` starts with ‘`^`’, then the function skips characters whose syntax is *not* in `syntaxes`.

The return value is the distance traveled, which is a nonnegative integer.

### Function: **skip-syntax-backward** *syntaxes \&optional limit*

This function moves point backward across characters whose syntax classes are mentioned in `syntaxes`. It stops when it encounters the beginning of the buffer, or position `limit` (if specified), or a character it is not supposed to skip.

If `syntaxes` starts with ‘`^`’, then the function skips characters whose syntax is *not* in `syntaxes`.

The return value indicates the distance traveled. It is an integer that is zero or less.

### Function: **backward-prefix-chars**

This function moves point backward over any number of characters with expression prefix syntax. This includes both characters in the expression prefix syntax class, and characters with the ‘`p`’ flag.
