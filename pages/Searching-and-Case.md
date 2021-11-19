

Next: [Regular Expressions](Regular-Expressions.html), Previous: [String Search](String-Search.html), Up: [Searching and Matching](Searching-and-Matching.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 34.2 Searching and Case

By default, searches in Emacs ignore the case of the text they are searching through; if you specify searching for ‘`FOO`’, then ‘`Foo`’ or ‘`foo`’ is also considered a match. This applies to regular expressions, too; thus, ‘`[aB]`’ would match ‘`a`’ or ‘`A`’ or ‘`b`’ or ‘`B`’.

If you do not want this feature, set the variable `case-fold-search` to `nil`. Then all letters must match exactly, including case. This is a buffer-local variable; altering the variable affects only the current buffer. (See [Intro to Buffer-Local](Intro-to-Buffer_002dLocal.html).) Alternatively, you may change the default value. In Lisp code, you will more typically use `let` to bind `case-fold-search` to the desired value.

Note that the user-level incremental search feature handles case distinctions differently. When the search string contains only lower case letters, the search ignores case, but when the search string contains one or more upper case letters, the search becomes case-sensitive. But this has nothing to do with the searching functions used in Lisp code. See [Incremental Search](https://www.gnu.org/software/emacs/manual/html_node/emacs/Incremental-Search.html#Incremental-Search) in The GNU Emacs Manual.

*   User Option: **case-fold-search**

    This buffer-local variable determines whether searches should ignore case. If the variable is `nil` they do not ignore case; otherwise (and by default) they do ignore case.

<!---->

*   User Option: **case-replace**

    This variable determines whether the higher-level replacement functions should preserve case. If the variable is `nil`, that means to use the replacement text verbatim. A non-`nil` value means to convert the case of the replacement text according to the text being replaced.

    This variable is used by passing it as an argument to the function `replace-match`. See [Replacing Match](Replacing-Match.html).

Next: [Regular Expressions](Regular-Expressions.html), Previous: [String Search](String-Search.html), Up: [Searching and Matching](Searching-and-Matching.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
