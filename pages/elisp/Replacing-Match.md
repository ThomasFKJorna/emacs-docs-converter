

#### 34.6.1 Replacing the Text that Matched

This function replaces all or part of the text matched by the last search. It works by means of the match data.

### Function: **replace-match** *replacement \&optional fixedcase literal string subexp*

This function performs a replacement operation on a buffer or string.

If you did the last search in a buffer, you should omit the `string` argument or specify `nil` for it, and make sure that the current buffer is the one in which you performed the last search. Then this function edits the buffer, replacing the matched text with `replacement`. It leaves point at the end of the replacement text.

If you performed the last search on a string, pass the same string as `string`. Then this function returns a new string, in which the matched text is replaced by `replacement`.

If `fixedcase` is non-`nil`, then `replace-match` uses the replacement text without case conversion; otherwise, it converts the replacement text depending upon the capitalization of the text to be replaced. If the original text is all upper case, this converts the replacement text to upper case. If all words of the original text are capitalized, this capitalizes all the words of the replacement text. If all the words are one-letter and they are all upper case, they are treated as capitalized words rather than all-upper-case words.

If `literal` is non-`nil`, then `replacement` is inserted exactly as it is, the only alterations being case changes as needed. If it is `nil` (the default), then the character ‘`\`’ is treated specially. If a ‘`\`’ appears in `replacement`, then it must be part of one of the following sequences:

*   ‘`\&`’

    This stands for the entire text being replaced.

*   ‘`\n`’, where `n` is a digit

    This stands for the text that matched the `n`th subexpression in the original regexp. Subexpressions are those expressions grouped inside ‘`\(…\)`’. If the `n`th subexpression never matched, an empty string is substituted.

*   ‘`\\`’

    This stands for a single ‘`\`’ in the replacement text.

*   ‘`\?`’

    This stands for itself (for compatibility with `replace-regexp` and related commands; see [Regexp Replace](https://www.gnu.org/software/emacs/manual/html_node/emacs/Regexp-Replace.html#Regexp-Replace) in The GNU Emacs Manual).

Any other character following ‘`\`’ signals an error.

The substitutions performed by ‘`\&`’ and ‘`\n`’ occur after case conversion, if any. Therefore, the strings they substitute are never case-converted.

If `subexp` is non-`nil`, that says to replace just subexpression number `subexp` of the regexp that was matched, not the entire match. For example, after matching ‘`foo \(ba*r\)`’, calling `replace-match` with 1 as `subexp` means to replace just the text that matched ‘`\(ba*r\)`’.

### Function: **match-substitute-replacement** *replacement \&optional fixedcase literal string subexp*

This function returns the text that would be inserted into the buffer by `replace-match`, but without modifying the buffer. It is useful if you want to present the user with actual replacement result, with constructs like ‘`\n`’ or ‘`\&`’ substituted with matched groups. Arguments `replacement` and optional `fixedcase`, `literal`, `string` and `subexp` have the same meaning as for `replace-match`.
