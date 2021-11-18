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

Next: [POSIX Regexps](POSIX-Regexps.html), Previous: [Regular Expressions](Regular-Expressions.html), Up: [Searching and Matching](Searching-and-Matching.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 34.4 Regular Expression Searching

In GNU Emacs, you can search for the next match for a regular expression (see [Syntax of Regexps](Syntax-of-Regexps.html)) either incrementally or not. For incremental search commands, see [Regular Expression Search](https://www.gnu.org/software/emacs/manual/html_node/emacs/Regexp-Search.html#Regexp-Search) in The GNU Emacs Manual. Here we describe only the search functions useful in programs. The principal one is `re-search-forward`.

These search functions convert the regular expression to multibyte if the buffer is multibyte; they convert the regular expression to unibyte if the buffer is unibyte. See [Text Representations](Text-Representations.html).

*   Command: **re-search-forward** *regexp \&optional limit noerror count*

    This function searches forward in the current buffer for a string of text that is matched by the regular expression `regexp`. The function skips over any amount of text that is not matched by `regexp`, and leaves point at the end of the first match found. It returns the new value of point.

    If `limit` is non-`nil`, it must be a position in the current buffer. It specifies the upper bound to the search. No match extending after that position is accepted. If `limit` is omitted or `nil`, it defaults to the end of the accessible portion of the buffer.

    What `re-search-forward` does when the search fails depends on the value of `noerror`:

    *   `nil`

        Signal a `search-failed` error.

    *   `t`

        Do nothing and return `nil`.

    *   anything else

        Move point to `limit` (or the end of the accessible portion of the buffer) and return `nil`.

    The argument `noerror` only affects valid searches which fail to find a match. Invalid arguments cause errors regardless of `noerror`.

    If `count` is a positive number `n`, the search is done `n` times; each successive search starts at the end of the previous match. If all these successive searches succeed, the function call succeeds, moving point and returning its new value. Otherwise the function call fails, with results depending on the value of `noerror`, as described above. If `count` is a negative number -`n`, the search is done `n` times in the opposite (backward) direction.

    In the following example, point is initially before the ‘`T`’. Evaluating the search call moves point to the end of that line (between the ‘`t`’ of ‘`hat`’ and the newline).

        ---------- Buffer: foo ----------
        I read "∗The cat in the hat
        comes back" twice.
        ---------- Buffer: foo ----------

    ```
    ```

        (re-search-forward "[a-z]+" nil t 5)
             ⇒ 27

        ---------- Buffer: foo ----------
        I read "The cat in the hat∗
        comes back" twice.
        ---------- Buffer: foo ----------

<!---->

*   Command: **re-search-backward** *regexp \&optional limit noerror count*

    This function searches backward in the current buffer for a string of text that is matched by the regular expression `regexp`, leaving point at the beginning of the first text found.

    This function is analogous to `re-search-forward`, but they are not simple mirror images. `re-search-forward` finds the match whose beginning is as close as possible to the starting point. If `re-search-backward` were a perfect mirror image, it would find the match whose end is as close as possible. However, in fact it finds the match whose beginning is as close as possible (and yet ends before the starting point). The reason for this is that matching a regular expression at a given spot always works from beginning to end, and starts at a specified beginning position.

    A true mirror-image of `re-search-forward` would require a special feature for matching regular expressions from end to beginning. It’s not worth the trouble of implementing that.

<!---->

*   Function: **string-match** *regexp string \&optional start*

    This function returns the index of the start of the first match for the regular expression `regexp` in `string`, or `nil` if there is no match. If `start` is non-`nil`, the search starts at that index in `string`.

    For example,

        (string-match
         "quick" "The quick brown fox jumped quickly.")
             ⇒ 4

    <!---->

        (string-match
         "quick" "The quick brown fox jumped quickly." 8)
             ⇒ 27

    The index of the first character of the string is 0, the index of the second character is 1, and so on.

    If this function finds a match, the index of the first character beyond the match is available as `(match-end 0)`. See [Match Data](Match-Data.html).

        (string-match
         "quick" "The quick brown fox jumped quickly." 8)
             ⇒ 27

    ```
    ```

        (match-end 0)
             ⇒ 32

<!---->

*   Function: **string-match-p** *regexp string \&optional start*

    This predicate function does what `string-match` does, but it avoids modifying the match data.

<!---->

*   Function: **looking-at** *regexp*

    This function determines whether the text in the current buffer directly following point matches the regular expression `regexp`. “Directly following” means precisely that: the search is “anchored” and it can succeed only starting with the first character following point. The result is `t` if so, `nil` otherwise.

    This function does not move point, but it does update the match data. See [Match Data](Match-Data.html). If you need to test for a match without modifying the match data, use `looking-at-p`, described below.

    In this example, point is located directly before the ‘`T`’. If it were anywhere else, the result would be `nil`.

        ---------- Buffer: foo ----------
        I read "∗The cat in the hat
        comes back" twice.
        ---------- Buffer: foo ----------

        (looking-at "The cat in the hat$")
             ⇒ t

<!---->

*   Function: **looking-back** *regexp limit \&optional greedy*

    This function returns `t` if `regexp` matches the text immediately before point (i.e., ending at point), and `nil` otherwise.

    Because regular expression matching works only going forward, this is implemented by searching backwards from point for a match that ends at point. That can be quite slow if it has to search a long distance. You can bound the time required by specifying a non-`nil` value for `limit`, which says not to search before `limit`. In this case, the match that is found must begin at or after `limit`. Here’s an example:

        ---------- Buffer: foo ----------
        I read "∗The cat in the hat
        comes back" twice.
        ---------- Buffer: foo ----------

        (looking-back "read \"" 3)
             ⇒ t
        (looking-back "read \"" 4)
             ⇒ nil

    If `greedy` is non-`nil`, this function extends the match backwards as far as possible, stopping when a single additional previous character cannot be part of a match for `regexp`. When the match is extended, its starting position is allowed to occur before `limit`.

    As a general recommendation, try to avoid using `looking-back` wherever possible, since it is slow. For this reason, there are no plans to add a `looking-back-p` function.

<!---->

*   Function: **looking-at-p** *regexp*

    This predicate function works like `looking-at`, but without updating the match data.

<!---->

*   Variable: **search-spaces-regexp**

    If this variable is non-`nil`, it should be a regular expression that says how to search for whitespace. In that case, any group of spaces in a regular expression being searched for stands for use of this regular expression. However, spaces inside of constructs such as ‘`[…]`’ and ‘`*`’, ‘`+`’, ‘`?`’ are not affected by `search-spaces-regexp`.

    Since this variable affects all regular expression search and match constructs, you should bind it temporarily for as small as possible a part of the code.

Next: [POSIX Regexps](POSIX-Regexps.html), Previous: [Regular Expressions](Regular-Expressions.html), Up: [Searching and Matching](Searching-and-Matching.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
