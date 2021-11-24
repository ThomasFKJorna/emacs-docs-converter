

#### 39.22.1 Usual Display Conventions

Here are the conventions for displaying each character code (in the absence of a display table, which can override these conventions; see [Display Tables](Display-Tables.html)).

The *printable ASCII characters*, character codes 32 through 126 (consisting of numerals, English letters, and symbols like ‘`#`’) are displayed literally.

The tab character (character code 9) displays as whitespace stretching up to the next tab stop column. See [Text Display](https://www.gnu.org/software/emacs/manual/html_node/emacs/Text-Display.html#Text-Display) in The GNU Emacs Manual. The variable `tab-width` controls the number of spaces per tab stop (see below).

### The newline character (character code 10) has a special effect: it ends the preceding line and starts a new line.

The non-printable *ASCII control characters*—character codes 0 through 31, as well as the `DEL` character (character code 127)—display in one of two ways according to the variable `ctl-arrow`. If this variable is non-`nil` (the default), these characters are displayed as sequences of two glyphs, where the first glyph is ‘`^`’ (a display table can specify a glyph to use instead of ‘`^`’); e.g., the `DEL` character is displayed as ‘`^?`’.

If `ctl-arrow` is `nil`, these characters are displayed as octal escapes (see below).

This rule also applies to carriage return (character code 13), if that character appears in the buffer. But carriage returns usually do not appear in buffer text; they are eliminated as part of end-of-line conversion (see [Coding System Basics](Coding-System-Basics.html)).

*Raw bytes* are non-ASCII characters with codes 128 through 255 (see [Text Representations](Text-Representations.html)). These characters display as *octal escapes*: sequences of four glyphs, where the first glyph is the ASCII code for ‘`\`’, and the others are digit characters representing the character code in octal. (A display table can specify a glyph to use instead of ‘`\`’.)

Each non-ASCII character with code above 255 is displayed literally, if the terminal supports it. If the terminal does not support it, the character is said to be *glyphless*, and it is usually displayed using a placeholder glyph. For example, if a graphical terminal has no font for a character, Emacs usually displays a box containing the character code in hexadecimal. See [Glyphless Chars](Glyphless-Chars.html).

The above display conventions apply even when there is a display table, for any character whose entry in the active display table is `nil`. Thus, when you set up a display table, you need only specify the characters for which you want special behavior.

The following variables affect how certain characters are displayed on the screen. Since they change the number of columns the characters occupy, they also affect the indentation functions. They also affect how the mode line is displayed; if you want to force redisplay of the mode line using the new values, call the function `force-mode-line-update` (see [Mode Line Format](Mode-Line-Format.html)).

### User Option: **ctl-arrow**

This buffer-local variable controls how control characters are displayed. If it is non-`nil`, they are displayed as a caret followed by the character: ‘`^A`’. If it is `nil`, they are displayed as octal escapes: a backslash followed by three octal digits, as in ‘`\001`’.

### User Option: **tab-width**

The value of this buffer-local variable is the spacing between tab stops used for displaying tab characters in Emacs buffers. The value is in units of columns, and the default is 8. Note that this feature is completely independent of the user-settable tab stops used by the command `tab-to-tab-stop`. See [Indent Tabs](Indent-Tabs.html).
