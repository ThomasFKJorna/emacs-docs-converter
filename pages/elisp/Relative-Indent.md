

#### 32.17.4 Indentation Relative to Previous Lines

This section describes two commands that indent the current line based on the contents of previous lines.

### Command: **indent-relative** *\&optional unindented-ok*

This command inserts whitespace at point, extending to the same column as the next *indent point* of the previous nonblank line. An indent point is a non-whitespace character following whitespace. The next indent point is the first one at a column greater than the current column of point. For example, if point is underneath and to the left of the first non-blank character of a line of text, it moves to that column by inserting whitespace.

If the previous nonblank line has no next indent point (i.e., none at a great enough column position), `indent-relative` either does nothing (if `unindented-ok` is non-`nil`) or calls `tab-to-tab-stop`. Thus, if point is underneath and to the right of the last column of a short line of text, this command ordinarily moves point to the next tab stop by inserting whitespace.

The return value of `indent-relative` is unpredictable.

In the following example, point is at the beginning of the second line:

```lisp
            This line is indented twelve spaces.
∗The quick brown fox jumped.
```

Evaluation of the expression `(indent-relative nil)` produces the following:

```lisp
            This line is indented twelve spaces.
            ∗The quick brown fox jumped.
```

In this next example, point is between the ‘`m`’ and ‘`p`’ of ‘`jumped`’:

```lisp
            This line is indented twelve spaces.
The quick brown fox jum∗ped.
```

Evaluation of the expression `(indent-relative nil)` produces the following:

```lisp
            This line is indented twelve spaces.
The quick brown fox jum  ∗ped.
```

### Command: **indent-relative-first-indent-point**

This command indents the current line like the previous nonblank line, by calling `indent-relative` with `t` as the `first-only` argument. The return value is unpredictable.

If the previous nonblank line has no indent points beyond the current column, this command does nothing.
