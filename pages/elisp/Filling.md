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

Next: [Margins](Margins.html), Previous: [Maintaining Undo](Maintaining-Undo.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.11 Filling

*Filling* means adjusting the lengths of lines (by moving the line breaks) so that they are nearly (but no greater than) a specified maximum width. Additionally, lines can be *justified*, which means inserting spaces to make the left and/or right margins line up precisely. The width is controlled by the variable `fill-column`. For ease of reading, lines should be no longer than 70 or so columns.

You can use Auto Fill mode (see [Auto Filling](Auto-Filling.html)) to fill text automatically as you insert it, but changes to existing text may leave it improperly filled. Then you must fill the text explicitly.

Most of the commands in this section return values that are not meaningful. All the functions that do filling take note of the current left margin, current right margin, and current justification style (see [Margins](Margins.html)). If the current justification style is `none`, the filling functions don’t actually do anything.

Several of the filling functions have an argument `justify`. If it is non-`nil`, that requests some kind of justification. It can be `left`, `right`, `full`, or `center`, to request a specific style of justification. If it is `t`, that means to use the current justification style for this part of the text (see `current-justification`, below). Any other value is treated as `full`.

When you call the filling functions interactively, using a prefix argument implies the value `full` for `justify`.

*   Command: **fill-paragraph** *\&optional justify region*

    This command fills the paragraph at or after point. If `justify` is non-`nil`, each line is justified as well. It uses the ordinary paragraph motion commands to find paragraph boundaries. See [Paragraphs](https://www.gnu.org/software/emacs/manual/html_node/emacs/Paragraphs.html#Paragraphs) in The GNU Emacs Manual.

    When `region` is non-`nil`, then if Transient Mark mode is enabled and the mark is active, this command calls `fill-region` to fill all the paragraphs in the region, instead of filling only the current paragraph. When this command is called interactively, `region` is `t`.

<!---->

*   Command: **fill-region** *start end \&optional justify nosqueeze to-eop*

    This command fills each of the paragraphs in the region from `start` to `end`. It justifies as well if `justify` is non-`nil`.

    If `nosqueeze` is non-`nil`, that means to leave whitespace other than line breaks untouched. If `to-eop` is non-`nil`, that means to keep filling to the end of the paragraph—or the next hard newline, if `use-hard-newlines` is enabled (see below).

    The variable `paragraph-separate` controls how to distinguish paragraphs. See [Standard Regexps](Standard-Regexps.html).

<!---->

*   Command: **fill-individual-paragraphs** *start end \&optional justify citation-regexp*

    This command fills each paragraph in the region according to its individual fill prefix. Thus, if the lines of a paragraph were indented with spaces, the filled paragraph will remain indented in the same fashion.

    The first two arguments, `start` and `end`, are the beginning and end of the region to be filled. The third and fourth arguments, `justify` and `citation-regexp`, are optional. If `justify` is non-`nil`, the paragraphs are justified as well as filled. If `citation-regexp` is non-`nil`, it means the function is operating on a mail message and therefore should not fill the header lines. If `citation-regexp` is a string, it is used as a regular expression; if it matches the beginning of a line, that line is treated as a citation marker.

    Ordinarily, `fill-individual-paragraphs` regards each change in indentation as starting a new paragraph. If `fill-individual-varying-indent` is non-`nil`, then only separator lines separate paragraphs. That mode can handle indented paragraphs with additional indentation on the first line.

<!---->

*   User Option: **fill-individual-varying-indent**

    This variable alters the action of `fill-individual-paragraphs` as described above.

<!---->

*   Command: **fill-region-as-paragraph** *start end \&optional justify nosqueeze squeeze-after*

    This command considers a region of text as a single paragraph and fills it. If the region was made up of many paragraphs, the blank lines between paragraphs are removed. This function justifies as well as filling when `justify` is non-`nil`.

    If `nosqueeze` is non-`nil`, that means to leave whitespace other than line breaks untouched. If `squeeze-after` is non-`nil`, it specifies a position in the region, and means don’t canonicalize spaces before that position.

    In Adaptive Fill mode, this command calls `fill-context-prefix` to choose a fill prefix by default. See [Adaptive Fill](Adaptive-Fill.html).

<!---->

*   Command: **justify-current-line** *\&optional how eop nosqueeze*

    This command inserts spaces between the words of the current line so that the line ends exactly at `fill-column`. It returns `nil`.

    The argument `how`, if non-`nil` specifies explicitly the style of justification. It can be `left`, `right`, `full`, `center`, or `none`. If it is `t`, that means to follow specified justification style (see `current-justification`, below). `nil` means to do full justification.

    If `eop` is non-`nil`, that means do only left-justification if `current-justification` specifies full justification. This is used for the last line of a paragraph; even if the paragraph as a whole is fully justified, the last line should not be.

    If `nosqueeze` is non-`nil`, that means do not change interior whitespace.

<!---->

*   User Option: **default-justification**

    This variable’s value specifies the style of justification to use for text that doesn’t specify a style with a text property. The possible values are `left`, `right`, `full`, `center`, or `none`. The default value is `left`.

<!---->

*   Function: **current-justification**

    This function returns the proper justification style to use for filling the text around point.

    This returns the value of the `justification` text property at point, or the variable `default-justification` if there is no such text property. However, it returns `nil` rather than `none` to mean “don’t justify”.

<!---->

*   User Option: **sentence-end-double-space**

    If this variable is non-`nil`, a period followed by just one space does not count as the end of a sentence, and the filling functions avoid breaking the line at such a place.

<!---->

*   User Option: **sentence-end-without-period**

    If this variable is non-`nil`, a sentence can end without a period. This is used for languages like Thai, where sentences end with a double space but without a period.

<!---->

*   User Option: **sentence-end-without-space**

    If this variable is non-`nil`, it should be a string of characters that can end a sentence without following spaces.

<!---->

*   User Option: **fill-separate-heterogeneous-words-with-space**

    If this variable is non-`nil`, two words of different kind (e.g., English and CJK) will be separated with a space when concatenating one that is in the end of a line and the other that is in the beginning of the next line for filling.

<!---->

*   Variable: **fill-paragraph-function**

    This variable provides a way to override the filling of paragraphs. If its value is non-`nil`, `fill-paragraph` calls this function to do the work. If the function returns a non-`nil` value, `fill-paragraph` assumes the job is done, and immediately returns that value.

    The usual use of this feature is to fill comments in programming language modes. If the function needs to fill a paragraph in the usual way, it can do so as follows:

        (let ((fill-paragraph-function nil))
          (fill-paragraph arg))

<!---->

*   Variable: **fill-forward-paragraph-function**

    This variable provides a way to override how the filling functions, such as `fill-region` and `fill-paragraph`, move forward to the next paragraph. Its value should be a function, which is called with a single argument `n`, the number of paragraphs to move, and should return the difference between `n` and the number of paragraphs actually moved. The default value of this variable is `forward-paragraph`. See [Paragraphs](https://www.gnu.org/software/emacs/manual/html_node/emacs/Paragraphs.html#Paragraphs) in The GNU Emacs Manual.

<!---->

*   Variable: **use-hard-newlines**

    If this variable is non-`nil`, the filling functions do not delete newlines that have the `hard` text property. These hard newlines act as paragraph separators. See [Hard and Soft Newlines](https://www.gnu.org/software/emacs/manual/html_node/emacs/Hard-and-Soft-Newlines.html#Hard-and-Soft-Newlines) in The GNU Emacs Manual.

Next: [Margins](Margins.html), Previous: [Maintaining Undo](Maintaining-Undo.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
