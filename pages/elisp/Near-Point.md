<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Buffer Contents](Buffer-Contents.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.1 Examining Text Near Point

Many functions are provided to look at the characters around point. Several simple functions are described here. See also `looking-at` in [Regexp Search](Regexp-Search.html).

In the following four functions, “beginning” or “end” of buffer refers to the beginning or end of the accessible portion.

*   Function: **char-after** *\&optional position*

    This function returns the character in the current buffer at (i.e., immediately after) position `position`. If `position` is out of range for this purpose, either before the beginning of the buffer, or at or beyond the end, then the value is `nil`. The default for `position` is point.

    In the following example, assume that the first character in the buffer is ‘`@`’:

        (string (char-after 1))
             ⇒ "@"

<!---->

*   Function: **char-before** *\&optional position*

    This function returns the character in the current buffer immediately before position `position`. If `position` is out of range for this purpose, either at or before the beginning of the buffer, or beyond the end, then the value is `nil`. The default for `position` is point.

<!---->

*   Function: **following-char**

    This function returns the character following point in the current buffer. This is similar to `(char-after (point))`. However, if point is at the end of the buffer, then `following-char` returns 0.

    Remember that point is always between characters, and the cursor normally appears over the character following point. Therefore, the character returned by `following-char` is the character the cursor is over.

    In this example, point is between the ‘`a`’ and the ‘`c`’.

        ---------- Buffer: foo ----------
        Gentlemen may cry ``Pea∗ce! Peace!,''
        but there is no peace.
        ---------- Buffer: foo ----------

    ```
    ```

        (string (preceding-char))
             ⇒ "a"
        (string (following-char))
             ⇒ "c"

<!---->

*   Function: **preceding-char**

    This function returns the character preceding point in the current buffer. See above, under `following-char`, for an example. If point is at the beginning of the buffer, `preceding-char` returns 0.

<!---->

*   Function: **bobp**

    This function returns `t` if point is at the beginning of the buffer. If narrowing is in effect, this means the beginning of the accessible portion of the text. See also `point-min` in [Point](Point.html).

<!---->

*   Function: **eobp**

    This function returns `t` if point is at the end of the buffer. If narrowing is in effect, this means the end of accessible portion of the text. See also `point-max` in See [Point](Point.html).

<!---->

*   Function: **bolp**

    This function returns `t` if point is at the beginning of a line. See [Text Lines](Text-Lines.html). The beginning of the buffer (or of its accessible portion) always counts as the beginning of a line.

<!---->

*   Function: **eolp**

    This function returns `t` if point is at the end of a line. The end of the buffer (or of its accessible portion) is always considered the end of a line.

Next: [Buffer Contents](Buffer-Contents.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
