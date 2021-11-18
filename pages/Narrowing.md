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

Previous: [Excursions](Excursions.html), Up: [Positions](Positions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 30.4 Narrowing

*Narrowing* means limiting the text addressable by Emacs editing commands to a limited range of characters in a buffer. The text that remains addressable is called the *accessible portion* of the buffer.

Narrowing is specified with two buffer positions, which become the beginning and end of the accessible portion. For most editing commands and primitives, these positions replace the values of the beginning and end of the buffer. While narrowing is in effect, no text outside the accessible portion is displayed, and point cannot move outside the accessible portion. Note that narrowing does not alter actual buffer positions (see [Point](Point.html)); it only determines which positions are considered the accessible portion of the buffer. Most functions refuse to operate on text that is outside the accessible portion.

Commands for saving buffers are unaffected by narrowing; they save the entire buffer regardless of any narrowing.

If you need to display in a single buffer several very different types of text, consider using an alternative facility described in [Swapping Text](Swapping-Text.html).

*   Command: **narrow-to-region** *start end*

    This function sets the accessible portion of the current buffer to start at `start` and end at `end`. Both arguments should be character positions.

    In an interactive call, `start` and `end` are set to the bounds of the current region (point and the mark, with the smallest first).

<!---->

*   Command: **narrow-to-page** *\&optional move-count*

    This function sets the accessible portion of the current buffer to include just the current page. An optional first argument `move-count` non-`nil` means to move forward or backward by `move-count` pages and then narrow to one page. The variable `page-delimiter` specifies where pages start and end (see [Standard Regexps](Standard-Regexps.html)).

    In an interactive call, `move-count` is set to the numeric prefix argument.

<!---->

*   Command: **widen**

    This function cancels any narrowing in the current buffer, so that the entire contents are accessible. This is called *widening*. It is equivalent to the following expression:

        (narrow-to-region 1 (1+ (buffer-size)))

<!---->

*   Function: **buffer-narrowed-p**

    This function returns non-`nil` if the buffer is narrowed, and `nil` otherwise.

<!---->

*   Special Form: **save-restriction** *body…*

    This special form saves the current bounds of the accessible portion, evaluates the `body` forms, and finally restores the saved bounds, thus restoring the same state of narrowing (or absence thereof) formerly in effect. The state of narrowing is restored even in the event of an abnormal exit via `throw` or error (see [Nonlocal Exits](Nonlocal-Exits.html)). Therefore, this construct is a clean way to narrow a buffer temporarily.

    The value returned by `save-restriction` is that returned by the last form in `body`, or `nil` if no body forms were given.

    **Caution:** it is easy to make a mistake when using the `save-restriction` construct. Read the entire description here before you try it.

    If `body` changes the current buffer, `save-restriction` still restores the restrictions on the original buffer (the buffer whose restrictions it saved from), but it does not restore the identity of the current buffer.

    `save-restriction` does *not* restore point; use `save-excursion` for that. If you use both `save-restriction` and `save-excursion` together, `save-excursion` should come first (on the outside). Otherwise, the old point value would be restored with temporary narrowing still in effect. If the old point value were outside the limits of the temporary narrowing, this would fail to restore it accurately.

    Here is a simple example of correct use of `save-restriction`:

        ---------- Buffer: foo ----------
        This is the contents of foo
        This is the contents of foo
        This is the contents of foo∗
        ---------- Buffer: foo ----------

    ```
    ```

        (save-excursion
          (save-restriction
            (goto-char 1)
            (forward-line 2)
            (narrow-to-region 1 (point))
            (goto-char (point-min))
            (replace-string "foo" "bar")))

        ---------- Buffer: foo ----------
        This is the contents of bar
        This is the contents of bar
        This is the contents of foo∗
        ---------- Buffer: foo ----------

Previous: [Excursions](Excursions.html), Up: [Positions](Positions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
