

### 32.13 Adaptive Fill Mode

When *Adaptive Fill Mode* is enabled, Emacs determines the fill prefix automatically from the text in each paragraph being filled rather than using a predetermined value. During filling, this fill prefix gets inserted at the start of the second and subsequent lines of the paragraph as described in [Filling](Filling.html), and in [Auto Filling](Auto-Filling.html).

### User Option: **adaptive-fill-mode**

Adaptive Fill mode is enabled when this variable is non-`nil`. It is `t` by default.

### Function: **fill-context-prefix** *from to*

This function implements the heart of Adaptive Fill mode; it chooses a fill prefix based on the text between `from` and `to`, typically the start and end of a paragraph. It does this by looking at the first two lines of the paragraph, based on the variables described below.

Usually, this function returns the fill prefix, a string. However, before doing this, the function makes a final check (not specially mentioned in the following) that a line starting with this prefix wouldn’t look like the start of a paragraph. Should this happen, the function signals the anomaly by returning `nil` instead.

In detail, `fill-context-prefix` does this:

1.  It takes a candidate for the fill prefix from the first line—it tries first the function in `adaptive-fill-function` (if any), then the regular expression `adaptive-fill-regexp` (see below). The first non-`nil` result of these, or the empty string if they’re both `nil`, becomes the first line’s candidate.
2.  If the paragraph has as yet only one line, the function tests the validity of the prefix candidate just found. The function then returns the candidate if it’s valid, or a string of spaces otherwise. (see the description of `adaptive-fill-first-line-regexp` below).
3.  When the paragraph already has two lines, the function next looks for a prefix candidate on the second line, in just the same way it did for the first line. If it doesn’t find one, it returns `nil`.
4.  The function now compares the two candidate prefixes heuristically: if the non-whitespace characters in the line 2 candidate occur in the same order in the line 1 candidate, the function returns the line 2 candidate. Otherwise, it returns the largest initial substring which is common to both candidates (which might be the empty string).

### User Option: **adaptive-fill-regexp**

Adaptive Fill mode matches this regular expression against the text starting after the left margin whitespace (if any) on a line; the characters it matches are that line’s candidate for the fill prefix.

The default value matches whitespace with certain punctuation characters intermingled.

### User Option: **adaptive-fill-first-line-regexp**

Used only in one-line paragraphs, this regular expression acts as an additional check of the validity of the one available candidate fill prefix: the candidate must match this regular expression, or match `comment-start-skip`. If it doesn’t, `fill-context-prefix` replaces the candidate with a string of spaces of the same width as it.

The default value of this variable is ``"\\`[ \t]*\\'"``, which matches only a string of whitespace. The effect of this default is to force the fill prefixes found in one-line paragraphs always to be pure whitespace.

### User Option: **adaptive-fill-function**

You can specify more complex ways of choosing a fill prefix automatically by setting this variable to a function. The function is called with point after the left margin (if any) of a line, and it must preserve point. It should return either that line’s fill prefix or `nil`, meaning it has failed to determine a prefix.
