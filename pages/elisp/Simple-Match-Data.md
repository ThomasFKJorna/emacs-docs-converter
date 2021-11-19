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

Next: [Entire Match Data](Entire-Match-Data.html), Previous: [Replacing Match](Replacing-Match.html), Up: [Match Data](Match-Data.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 34.6.2 Simple Match Data Access

This section explains how to use the match data to find out what was matched by the last search or match operation, if it succeeded.

You can ask about the entire matching text, or about a particular parenthetical subexpression of a regular expression. The `count` argument in the functions below specifies which. If `count` is zero, you are asking about the entire match. If `count` is positive, it specifies which subexpression you want.

Recall that the subexpressions of a regular expression are those expressions grouped with escaped parentheses, ‘`\(…\)`’. The `count`th subexpression is found by counting occurrences of ‘`\(`’ from the beginning of the whole regular expression. The first subexpression is numbered 1, the second 2, and so on. Only regular expressions can have subexpressions—after a simple string search, the only information available is about the entire match.

Every successful search sets the match data. Therefore, you should query the match data immediately after searching, before calling any other function that might perform another search. Alternatively, you may save and restore the match data (see [Saving Match Data](Saving-Match-Data.html)) around the call to functions that could perform another search. Or use the functions that explicitly do not modify the match data; e.g., `string-match-p`.

A search which fails may or may not alter the match data. In the current implementation, it does not, but we may change it in the future. Don’t try to rely on the value of the match data after a failing search.

*   Function: **match-string** *count \&optional in-string*

    This function returns, as a string, the text matched in the last search or match operation. It returns the entire text if `count` is zero, or just the portion corresponding to the `count`th parenthetical subexpression, if `count` is positive.

    If the last such operation was done against a string with `string-match`, then you should pass the same string as the argument `in-string`. After a buffer search or match, you should omit `in-string` or pass `nil` for it; but you should make sure that the current buffer when you call `match-string` is the one in which you did the searching or matching. Failure to follow this advice will lead to incorrect results.

    The value is `nil` if `count` is out of range, or for a subexpression inside a ‘`\|`’ alternative that wasn’t used or a repetition that repeated zero times.

<!---->

*   Function: **match-string-no-properties** *count \&optional in-string*

    This function is like `match-string` except that the result has no text properties.

<!---->

*   Function: **match-beginning** *count*

    If the last regular expression search found a match, this function returns the position of the start of the matching text or of a subexpression of it.

    If `count` is zero, then the value is the position of the start of the entire match. Otherwise, `count` specifies a subexpression in the regular expression, and the value of the function is the starting position of the match for that subexpression.

    The value is `nil` for a subexpression inside a ‘`\|`’ alternative that wasn’t used or a repetition that repeated zero times.

<!---->

*   Function: **match-end** *count*

    This function is like `match-beginning` except that it returns the position of the end of the match, rather than the position of the beginning.

Here is an example of using the match data, with a comment showing the positions within the text:

    (string-match "\\(qu\\)\\(ick\\)"
                  "The quick fox jumped quickly.")
                  ;0123456789
         ⇒ 4

```
```

    (match-string 0 "The quick fox jumped quickly.")
         ⇒ "quick"
    (match-string 1 "The quick fox jumped quickly.")
         ⇒ "qu"
    (match-string 2 "The quick fox jumped quickly.")
         ⇒ "ick"

```
```

    (match-beginning 1)       ; The beginning of the match
         ⇒ 4                 ;   with ‘qu’ is at index 4.

```
```

    (match-beginning 2)       ; The beginning of the match
         ⇒ 6                 ;   with ‘ick’ is at index 6.

```
```

    (match-end 1)             ; The end of the match
         ⇒ 6                 ;   with ‘qu’ is at index 6.

    (match-end 2)             ; The end of the match
         ⇒ 9                 ;   with ‘ick’ is at index 9.

Here is another example. Point is initially located at the beginning of the line. Searching moves point to between the space and the word ‘`in`’. The beginning of the entire match is at the 9th character of the buffer (‘`T`’), and the beginning of the match for the first subexpression is at the 13th character (‘`c`’).

    (list
      (re-search-forward "The \\(cat \\)")
      (match-beginning 0)
      (match-beginning 1))
        ⇒ (17 9 13)

```
```

    ---------- Buffer: foo ----------
    I read "The cat ∗in the hat comes back" twice.
            ^   ^
            9  13
    ---------- Buffer: foo ----------

(In this case, the index returned is a buffer position; the first character of the buffer counts as 1.)

Next: [Entire Match Data](Entire-Match-Data.html), Previous: [Replacing Match](Replacing-Match.html), Up: [Match Data](Match-Data.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
