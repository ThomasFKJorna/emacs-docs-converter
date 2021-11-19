

Next: [Rx Notation](Rx-Notation.html), Previous: [Syntax of Regexps](Syntax-of-Regexps.html), Up: [Regular Expressions](Regular-Expressions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 34.3.2 Complex Regexp Example

Here is a complicated regexp which was formerly used by Emacs to recognize the end of a sentence together with any whitespace that follows. (Nowadays Emacs uses a similar but more complex default regexp constructed by the function `sentence-end`. See [Standard Regexps](Standard-Regexps.html).)

Below, we show first the regexp as a string in Lisp syntax (to distinguish spaces from tab characters), and then the result of evaluating it. The string constant begins and ends with a double-quote. ‘`\"`’ stands for a double-quote as part of the string, ‘`\\`’ for a backslash as part of the string, ‘`\t`’ for a tab and ‘`\n`’ for a newline.

```lisp
"[.?!][]\"')}]*\\($\\| $\\|\t\\|  \\)[ \t\n]*"
     ⇒ "[.?!][]\"')}]*\\($\\| $\\|  \\|  \\)[
]*"
```

In the output, tab and newline appear as themselves.

This regular expression contains four parts in succession and can be deciphered as follows:

*   `[.?!]`

    The first part of the pattern is a character alternative that matches any one of three characters: period, question mark, and exclamation mark. The match must begin with one of these three characters. (This is one point where the new default regexp used by Emacs differs from the old. The new value also allows some non-ASCII characters that end a sentence without any following whitespace.)

*   `[]\"')}]*`

    The second part of the pattern matches any closing braces and quotation marks, zero or more of them, that may follow the period, question mark or exclamation mark. The `\"` is Lisp syntax for a double-quote in a string. The ‘`*`’ at the end indicates that the immediately preceding regular expression (a character alternative, in this case) may be repeated zero or more times.

*   `\\($\\| $\\|\t\\|  \\)`

    The third part of the pattern matches the whitespace that follows the end of a sentence: the end of a line (optionally with a space), or a tab, or two spaces. The double backslashes mark the parentheses and vertical bars as regular expression syntax; the parentheses delimit a group and the vertical bars separate alternatives. The dollar sign is used to match the end of a line.

*   `[ \t\n]*`

    Finally, the last part of the pattern matches any additional whitespace beyond the minimum needed to end a sentence.

In the `rx` notation (see [Rx Notation](Rx-Notation.html)), the regexp could be written

```lisp
(rx (any ".?!")                    ; Punctuation ending sentence.
    (zero-or-more (any "\"')]}"))  ; Closing quotes or brackets.
    (or line-end
        (seq " " line-end)
        "\t"
        "  ")                      ; Two spaces.
    (zero-or-more (any "\t\n ")))  ; Optional extra whitespace.
```

Since `rx` regexps are just S-expressions, they can be formatted and commented as such.

Next: [Rx Notation](Rx-Notation.html), Previous: [Syntax of Regexps](Syntax-of-Regexps.html), Up: [Regular Expressions](Regular-Expressions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
