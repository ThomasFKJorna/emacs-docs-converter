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

Next: [Columns](Columns.html), Previous: [Auto Filling](Auto-Filling.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.15 Sorting Text

The sorting functions described in this section all rearrange text in a buffer. This is in contrast to the function `sort`, which rearranges the order of the elements of a list (see [Rearrangement](Rearrangement.html)). The values returned by these functions are not meaningful.

*   Function: **sort-subr** *reverse nextrecfun endrecfun \&optional startkeyfun endkeyfun predicate*

    This function is the general text-sorting routine that subdivides a buffer into records and then sorts them. Most of the commands in this section use this function.

    To understand how `sort-subr` works, consider the whole accessible portion of the buffer as being divided into disjoint pieces called *sort records*. The records may or may not be contiguous, but they must not overlap. A portion of each sort record (perhaps all of it) is designated as the sort key. Sorting rearranges the records in order by their sort keys.

    Usually, the records are rearranged in order of ascending sort key. If the first argument to the `sort-subr` function, `reverse`, is non-`nil`, the sort records are rearranged in order of descending sort key.

    The next four arguments to `sort-subr` are functions that are called to move point across a sort record. They are called many times from within `sort-subr`.

    1.  `nextrecfun` is called with point at the end of a record. This function moves point to the start of the next record. The first record is assumed to start at the position of point when `sort-subr` is called. Therefore, you should usually move point to the beginning of the buffer before calling `sort-subr`.

        This function can indicate there are no more sort records by leaving point at the end of the buffer.

    2.  `endrecfun` is called with point within a record. It moves point to the end of the record.

    3.  `startkeyfun` is called to move point from the start of a record to the start of the sort key. This argument is optional; if it is omitted, the whole record is the sort key. If supplied, the function should either return a non-`nil` value to be used as the sort key, or return `nil` to indicate that the sort key is in the buffer starting at point. In the latter case, `endkeyfun` is called to find the end of the sort key.

    4.  `endkeyfun` is called to move point from the start of the sort key to the end of the sort key. This argument is optional. If `startkeyfun` returns `nil` and this argument is omitted (or `nil`), then the sort key extends to the end of the record. There is no need for `endkeyfun` if `startkeyfun` returns a non-`nil` value.

    The argument `predicate` is the function to use to compare keys. It is called with two arguments, the keys to compare, and should return non-`nil` if the first key should come before the second in the sorting order. What exactly are the key arguments depends on what `startkeyfun` and `endkeyfun` return. If `predicate` is omitted or `nil`, it defaults to `<` if the keys are numbers, to `compare-buffer-substrings` if the keys are cons cells (whose `car` and `cdr` are start and end buffer positions of the key), and to `string<` otherwise (with keys assumed to be strings).

    As an example of `sort-subr`, here is the complete function definition for `sort-lines`:

        ;; Note that the first two lines of doc string
        ;; are effectively one line when viewed by a user.
        (defun sort-lines (reverse beg end)
          "Sort lines in region alphabetically;\
         argument means descending order.
        Called from a program, there are three arguments:

    <!---->

        REVERSE (non-nil means reverse order),\
         BEG and END (region to sort).
        The variable `sort-fold-case' determines\
         whether alphabetic case affects
        the sort order."

    <!---->

          (interactive "P\nr")
          (save-excursion
            (save-restriction
              (narrow-to-region beg end)
              (goto-char (point-min))
              (let ((inhibit-field-text-motion t))
                (sort-subr reverse 'forward-line 'end-of-line)))))

    Here `forward-line` moves point to the start of the next record, and `end-of-line` moves point to the end of record. We do not pass the arguments `startkeyfun` and `endkeyfun`, because the entire record is used as the sort key.

    The `sort-paragraphs` function is very much the same, except that its `sort-subr` call looks like this:

        (sort-subr reverse
                   (lambda ()
                     (while (and (not (eobp))
                                 (looking-at paragraph-separate))
                       (forward-line 1)))
                   'forward-paragraph)

    Markers pointing into any sort records are left with no useful position after `sort-subr` returns.

<!---->

*   User Option: **sort-fold-case**

    If this variable is non-`nil`, `sort-subr` and the other buffer sorting functions ignore case when comparing strings.

<!---->

*   Command: **sort-regexp-fields** *reverse record-regexp key-regexp start end*

    This command sorts the region between `start` and `end` alphabetically as specified by `record-regexp` and `key-regexp`. If `reverse` is a negative integer, then sorting is in reverse order.

    Alphabetical sorting means that two sort keys are compared by comparing the first characters of each, the second characters of each, and so on. If a mismatch is found, it means that the sort keys are unequal; the sort key whose character is less at the point of first mismatch is the lesser sort key. The individual characters are compared according to their numerical character codes in the Emacs character set.

    The value of the `record-regexp` argument specifies how to divide the buffer into sort records. At the end of each record, a search is done for this regular expression, and the text that matches it is taken as the next record. For example, the regular expression ‘`^.+$`’, which matches lines with at least one character besides a newline, would make each such line into a sort record. See [Regular Expressions](Regular-Expressions.html), for a description of the syntax and meaning of regular expressions.

    The value of the `key-regexp` argument specifies what part of each record is the sort key. The `key-regexp` could match the whole record, or only a part. In the latter case, the rest of the record has no effect on the sorted order of records, but it is carried along when the record moves to its new position.

    The `key-regexp` argument can refer to the text matched by a subexpression of `record-regexp`, or it can be a regular expression on its own.

    If `key-regexp` is:

    *   ‘`\digit`’

        then the text matched by the `digit`th ‘`\(...\)`’ parenthesis grouping in `record-regexp` is the sort key.

    *   ‘`\&`’

        then the whole record is the sort key.

    *   a regular expression

        then `sort-regexp-fields` searches for a match for the regular expression within the record. If such a match is found, it is the sort key. If there is no match for `key-regexp` within a record then that record is ignored, which means its position in the buffer is not changed. (The other records may move around it.)

    For example, if you plan to sort all the lines in the region by the first word on each line starting with the letter ‘`f`’, you should set `record-regexp` to ‘`^.*$`’ and set `key-regexp` to ‘`\<f\w*\>`’. The resulting expression looks like this:

        (sort-regexp-fields nil "^.*$" "\\<f\\w*\\>"
                            (region-beginning)
                            (region-end))

    If you call `sort-regexp-fields` interactively, it prompts for `record-regexp` and `key-regexp` in the minibuffer.

<!---->

*   Command: **sort-lines** *reverse start end*

    This command alphabetically sorts lines in the region between `start` and `end`. If `reverse` is non-`nil`, the sort is in reverse order.

<!---->

*   Command: **sort-paragraphs** *reverse start end*

    This command alphabetically sorts paragraphs in the region between `start` and `end`. If `reverse` is non-`nil`, the sort is in reverse order.

<!---->

*   Command: **sort-pages** *reverse start end*

    This command alphabetically sorts pages in the region between `start` and `end`. If `reverse` is non-`nil`, the sort is in reverse order.

<!---->

*   Command: **sort-fields** *field start end*

    This command sorts lines in the region between `start` and `end`, comparing them alphabetically by the `field`th field of each line. Fields are separated by whitespace and numbered starting from 1. If `field` is negative, sorting is by the -`field`th<!-- /@w --> field from the end of the line. This command is useful for sorting tables.

<!---->

*   Command: **sort-numeric-fields** *field start end*

    This command sorts lines in the region between `start` and `end`, comparing them numerically by the `field`th field of each line. Fields are separated by whitespace and numbered starting from 1. The specified field must contain a number in each line of the region. Numbers starting with 0 are treated as octal, and numbers starting with ‘`0x`’ are treated as hexadecimal.

    If `field` is negative, sorting is by the -`field`th<!-- /@w --> field from the end of the line. This command is useful for sorting tables.

<!---->

*   User Option: **sort-numeric-base**

    This variable specifies the default radix for `sort-numeric-fields` to parse numbers.

<!---->

*   Command: **sort-columns** *reverse \&optional beg end*

    This command sorts the lines in the region between `beg` and `end`, comparing them alphabetically by a certain range of columns. The column positions of `beg` and `end` bound the range of columns to sort on.

    If `reverse` is non-`nil`, the sort is in reverse order.

    One unusual thing about this command is that the entire line containing position `beg`, and the entire line containing position `end`, are included in the region sorted.

    Note that `sort-columns` rejects text that contains tabs, because tabs could be split across the specified columns. Use `M-x untabify` to convert tabs to spaces before sorting.

    When possible, this command actually works by calling the `sort` utility program.

Next: [Columns](Columns.html), Previous: [Auto Filling](Auto-Filling.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
