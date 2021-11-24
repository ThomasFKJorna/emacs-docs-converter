

### 34.8 Standard Regular Expressions Used in Editing

This section describes some variables that hold regular expressions used for certain purposes in editing:

### User Option: **page-delimiter**

This is the regular expression describing line-beginnings that separate pages. The default value is `"^\014"` (i.e., `"^^L"` or `"^\C-l"`); this matches a line that starts with a formfeed character.

The following two regular expressions should *not* assume the match always starts at the beginning of a line; they should not use ‘`^`’ to anchor the match. Most often, the paragraph commands do check for a match only at the beginning of a line, which means that ‘`^`’ would be superfluous. When there is a nonzero left margin, they accept matches that start after the left margin. In that case, a ‘`^`’ would be incorrect. However, a ‘`^`’ is harmless in modes where a left margin is never used.

### User Option: **paragraph-separate**

This is the regular expression for recognizing the beginning of a line that separates paragraphs. (If you change this, you may have to change `paragraph-start` also.) The default value is `"[ \t\f]*$"`, which matches a line that consists entirely of spaces, tabs, and form feeds (after its left margin).

### User Option: **paragraph-start**

This is the regular expression for recognizing the beginning of a line that starts *or* separates paragraphs. The default value is `"\f\\|[ \t]*$"`, which matches a line containing only whitespace or starting with a form feed (after its left margin).

### User Option: **sentence-end**

If non-`nil`, the value should be a regular expression describing the end of a sentence, including the whitespace following the sentence. (All paragraph boundaries also end sentences, regardless.)

If the value is `nil`, as it is by default, then the function `sentence-end` constructs the regexp. That is why you should always call the function `sentence-end` to obtain the regexp to be used to recognize the end of a sentence.

### Function: **sentence-end**

This function returns the value of the variable `sentence-end`, if non-`nil`. Otherwise it returns a default value based on the values of the variables `sentence-end-double-space` (see [Definition of sentence-end-double-space](Filling.html#Definition-of-sentence_002dend_002ddouble_002dspace)), `sentence-end-without-period`, and `sentence-end-without-space`.
