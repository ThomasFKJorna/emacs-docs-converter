

### 39.26 Bidirectional Display

Emacs can display text written in scripts, such as Arabic, Farsi, and Hebrew, whose natural ordering for horizontal text display runs from right to left. Furthermore, segments of Latin script and digits embedded in right-to-left text are displayed left-to-right, while segments of right-to-left script embedded in left-to-right text (e.g., Arabic or Hebrew text in comments or strings in a program source file) are appropriately displayed right-to-left. We call such mixtures of left-to-right and right-to-left text *bidirectional text*. This section describes the facilities and options for editing and displaying bidirectional text.

Text is stored in Emacs buffers and strings in *logical* (or *reading*) order, i.e., the order in which a human would read each character. In right-to-left and bidirectional text, the order in which characters are displayed on the screen (called *visual order*) is not the same as logical order; the characters’ screen positions do not increase monotonically with string or buffer position. In performing this *bidirectional reordering*, Emacs follows the Unicode Bidirectional Algorithm (a.k.a. UBA), which is described in Annex #9 of the Unicode standard (<http://www.unicode.org/reports/tr9/>). Emacs provides a “Full Bidirectionality” class implementation of the UBA, consistent with the requirements of the Unicode Standard v9.0. Note, however, that the way Emacs displays continuation lines when text direction is opposite to the base paragraph direction deviates from the UBA, which requires to perform line wrapping before reordering text for display.

### Variable: **bidi-display-reordering**

If the value of this buffer-local variable is non-`nil` (the default), Emacs performs bidirectional reordering for display. The reordering affects buffer text, as well as display strings and overlay strings from text and overlay properties in the buffer (see [Overlay Properties](Overlay-Properties.html), and see [Display Property](Display-Property.html)). If the value is `nil`, Emacs does not perform bidirectional reordering in the buffer.

The default value of `bidi-display-reordering` controls the reordering of strings which are not directly supplied by a buffer, including the text displayed in mode lines (see [Mode Line Format](Mode-Line-Format.html)) and header lines (see [Header Lines](Header-Lines.html)).

Emacs never reorders the text of a unibyte buffer, even if `bidi-display-reordering` is non-`nil` in the buffer. This is because unibyte buffers contain raw bytes, not characters, and thus lack the directionality properties required for reordering. Therefore, to test whether text in a buffer will be reordered for display, it is not enough to test the value of `bidi-display-reordering` alone. The correct test is this:

```lisp
 (if (and enable-multibyte-characters
          bidi-display-reordering)
     ;; Buffer is being reordered for display
   )
```

However, unibyte display and overlay strings *are* reordered if their parent buffer is reordered. This is because plain-ASCII strings are stored by Emacs as unibyte strings. If a unibyte display or overlay string includes non-ASCII characters, these characters are assumed to have left-to-right direction.

Text covered by `display` text properties, by overlays with `display` properties whose value is a string, and by any other properties that replace buffer text, is treated as a single unit when it is reordered for display. That is, the entire chunk of text covered by these properties is reordered together. Moreover, the bidirectional properties of the characters in such a chunk of text are ignored, and Emacs reorders them as if they were replaced with a single character `U+FFFC`, known as the *Object Replacement Character*. This means that placing a display property over a portion of text may change the way that the surrounding text is reordered for display. To prevent this unexpected effect, always place such properties on text whose directionality is identical with text that surrounds it.

Each paragraph of bidirectional text has a *base direction*, either right-to-left or left-to-right. Left-to-right paragraphs are displayed beginning at the left margin of the window, and are truncated or continued when the text reaches the right margin. Right-to-left paragraphs are displayed beginning at the right margin, and are continued or truncated at the left margin.

Where exactly paragraphs start and end, for the purpose of the Emacs UBA implementation, is determined by the following two buffer-local variables (note that `paragraph-start` and `paragraph-separate` have no influence on this). By default both of these variables are `nil`, and paragraphs are bounded by empty lines, i.e., lines that consist entirely of zero or more whitespace characters followed by a newline.

### Variable: **bidi-paragraph-start-re**

If non-`nil`, this variable’s value should be a regular expression matching a line that starts or separates two paragraphs. The regular expression is always matched after a newline, so it is best to anchor it, i.e., begin it with a `"^"`.

### Variable: **bidi-paragraph-separate-re**

If non-`nil`, this variable’s value should be a regular expression matching a line separates two paragraphs. The regular expression is always matched after a newline, so it is best to anchor it, i.e., begin it with a `"^"`.

If you modify any of these two variables, you should normally modify both, to make sure they describe paragraphs consistently. For example, to have each new line start a new paragraph for bidi-reordering purposes, set both variables to `"^"`.

By default, Emacs determines the base direction of each paragraph by looking at the text at its beginning. The precise method of determining the base direction is specified by the UBA; in a nutshell, the first character in a paragraph that has an explicit directionality determines the base direction of the paragraph. However, sometimes a buffer may need to force a certain base direction for its paragraphs. For example, buffers containing program source code should force all paragraphs to be displayed left-to-right. You can use following variable to do this:

### User Option: **bidi-paragraph-direction**

If the value of this buffer-local variable is the symbol `right-to-left` or `left-to-right`, all paragraphs in the buffer are assumed to have that specified direction. Any other value is equivalent to `nil` (the default), which means to determine the base direction of each paragraph from its contents.

Modes for program source code should set this to `left-to-right`. Prog mode does this by default, so modes derived from Prog mode do not need to set this explicitly (see [Basic Major Modes](Basic-Major-Modes.html)).

### Function: **current-bidi-paragraph-direction** *\&optional buffer*

This function returns the paragraph direction at point in the named `buffer`. The returned value is a symbol, either `left-to-right` or `right-to-left`. If `buffer` is omitted or `nil`, it defaults to the current buffer. If the buffer-local value of the variable `bidi-paragraph-direction` is non-`nil`, the returned value will be identical to that value; otherwise, the returned value reflects the paragraph direction determined dynamically by Emacs. For buffers whose value of `bidi-display-reordering` is `nil` as well as unibyte buffers, this function always returns `left-to-right`.

Sometimes there’s a need to move point in strict visual order, either to the left or to the right of its current screen position. Emacs provides a primitive to do that.

### Function: **move-point-visually** *direction*

This function moves point of the currently selected window to the buffer position that appears immediately to the right or to the left of point on the screen. If `direction` is positive, point will move one screen position to the right, otherwise it will move one screen position to the left. Note that, depending on the surrounding bidirectional context, this could potentially move point many buffer positions away. If invoked at the end of a screen line, the function moves point to the rightmost or leftmost screen position of the next or previous screen line, as appropriate for the value of `direction`.

The function returns the new buffer position as its value.

Bidirectional reordering can have surprising and unpleasant effects when two strings with bidirectional content are juxtaposed in a buffer, or otherwise programmatically concatenated into a string of text. A typical problematic case is when a buffer consists of sequences of text fields separated by whitespace or punctuation characters, like Buffer Menu mode or Rmail Summary Mode. Because the punctuation characters used as separators have *weak directionality*, they take on the directionality of surrounding text. As result, a numeric field that follows a field with bidirectional content can be displayed *to the left* of the preceding field, messing up the expected layout. There are several ways to avoid this problem:

\- Append the special character U+200E LEFT-TO-RIGHT MARK, or LRM, to the end of each field that may have bidirectional content, or prepend it to the beginning of the following field. The function `bidi-string-mark-left-to-right`, described below, comes in handy for this purpose. (In a right-to-left paragraph, use U+200F RIGHT-TO-LEFT MARK, or RLM, instead.) This is one of the solutions recommended by the UBA.

\- Include the tab character in the field separator. The tab character plays the role of *segment separator* in bidirectional reordering, causing the text on either side to be reordered separately.

\- Separate fields with a `display` property or overlay with a property value of the form `(space . PROPS)` (see [Specified Space](Specified-Space.html)). Emacs treats this display specification as a *paragraph separator*, and reorders the text on either side separately.

### Function: **bidi-string-mark-left-to-right** *string*

This function returns its argument `string`, possibly modified, such that the result can be safely concatenated with another string, or juxtaposed with another string in a buffer, without disrupting the relative layout of this string and the next one on display. If the string returned by this function is displayed as part of a left-to-right paragraph, it will always appear on display to the left of the text that follows it. The function works by examining the characters of its argument, and if any of those characters could cause reordering on display, the function appends the LRM character to the string. The appended LRM character is made invisible by giving it an `invisible` text property of `t` (see [Invisible Text](Invisible-Text.html)).

The reordering algorithm uses the bidirectional properties of the characters stored as their `bidi-class` property (see [Character Properties](Character-Properties.html)). Lisp programs can change these properties by calling the `put-char-code-property` function. However, doing this requires a thorough understanding of the UBA, and is therefore not recommended. Any changes to the bidirectional properties of a character have global effect: they affect all Emacs frames and windows.

Similarly, the `mirroring` property is used to display the appropriate mirrored character in the reordered text. Lisp programs can affect the mirrored display by changing this property. Again, any such changes affect all of Emacs display.

The bidirectional properties of characters can be overridden by inserting into the text special directional control characters, LEFT-TO-RIGHT OVERRIDE (LRO) and RIGHT-TO-LEFT OVERRIDE (RLO). Any characters between a RLO and the following newline or POP DIRECTIONAL FORMATTING (PDF) control character, whichever comes first, will be displayed as if they were strong right-to-left characters, i.e. they will be reversed on display. Similarly, any characters between LRO and PDF or newline will display as if they were strong left-to-right, and will *not* be reversed even if they are strong right-to-left characters.

These overrides are useful when you want to make some text unaffected by the reordering algorithm, and instead directly control the display order. But they can also be used for malicious purposes, known as *phishing*. Specifically, a URL on a Web page or a link in an email message can be manipulated to make its visual appearance unrecognizable, or similar to some popular benign location, while the real location, interpreted by a browser in the logical order, is very different.

Emacs provides a primitive that applications can use to detect instances of text whose bidirectional properties were overridden so as to make a left-to-right character display as if it were a right-to-left character, or vice versa.

### Function: **bidi-find-overridden-directionality** *from to \&optional object*

This function looks at the text of the specified `object` between positions `from` (inclusive) and `to` (exclusive), and returns the first position where it finds a strong left-to-right character whose directional properties were forced to display the character as right-to-left, or for a strong right-to-left character that was forced to display as left-to-right. If it finds no such characters in the specified region of text, it returns `nil`.

The optional argument `object` specifies which text to search, and defaults to the current buffer. If `object` is non-`nil`, it can be some other buffer, or it can be a string or a window. If it is a string, the function searches that string. If it is a window, the function searches the buffer displayed in that window. If a buffer whose text you want to examine is displayed in some window, we recommend to specify it by that window, rather than pass the buffer to the function. This is because telling the function about the window allows it to correctly account for window-specific overlays, which might change the result of the function if some text in the buffer is covered by overlays.

When text that includes mixed right-to-left and left-to-right characters and bidirectional controls is copied into a different location, it can change its visual appearance, and also can affect the visual appearance of the surrounding text at destination. This is because reordering of bidirectional text specified by the UBA has non-trivial context-dependent effects both on the copied text and on the text at copy destination that will surround it.

Sometimes, a Lisp program may need to preserve the exact visual appearance of the copied text at destination, and of the text that surrounds the copy. Lisp programs can use the following function to achieve that effect.

### Function: **buffer-substring-with-bidi-context** *start end \&optional no-properties*

This function works similar to `buffer-substring` (see [Buffer Contents](Buffer-Contents.html)), but it prepends and appends to the copied text bidi directional control characters necessary to preserve the visual appearance of the text when it is inserted at another place. Optional argument `no-properties`, if non-`nil`, means remove the text properties from the copy of the text.
