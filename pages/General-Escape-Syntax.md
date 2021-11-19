

Next: [Ctl-Char Syntax](Ctl_002dChar-Syntax.html), Previous: [Basic Char Syntax](Basic-Char-Syntax.html), Up: [Character Type](Character-Type.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.4.3.2 General Escape Syntax

In addition to the specific escape sequences for special important control characters, Emacs provides several types of escape syntax that you can use to specify non-ASCII text characters.

1.  You can specify characters by their Unicode names, if any. `?\N{NAME}` represents the Unicode character named `NAME`. Thus, ‘`?\N{LATIN SMALL LETTER A WITH GRAVE}`’ is equivalent to `?à` and denotes the Unicode character U+00E0. To simplify entering multi-line strings, you can replace spaces in the names by non-empty sequences of whitespace (e.g., newlines).
2.  You can specify characters by their Unicode values. `?\N{U+X}` represents a character with Unicode code point `X`, where `X` is a hexadecimal number. Also, `?\uxxxx` and `?\Uxxxxxxxx` represent code points `xxxx` and `xxxxxxxx`, respectively, where each `x` is a single hexadecimal digit. For example, `?\N{U+E0}`, `?\u00e0` and `?\U000000E0` are all equivalent to `?à` and to ‘`?\N{LATIN SMALL LETTER A WITH GRAVE}`’. The Unicode Standard defines code points only up to ‘`U+10ffff`’, so if you specify a code point higher than that, Emacs signals an error.
3.  You can specify characters by their hexadecimal character codes. A hexadecimal escape sequence consists of a backslash, ‘`x`’, and the hexadecimal character code. Thus, ‘`?\x41`’ is the character `A`, ‘`?\x1`’ is the character `C-a`, and `?\xe0` is the character `à` (`a` with grave accent). You can use any number of hex digits, so you can represent any character code in this way.
4.  You can specify characters by their character code in octal. An octal escape sequence consists of a backslash followed by up to three octal digits; thus, ‘`?\101`’ for the character `A`, ‘`?\001`’ for the character `C-a`, and `?\002` for the character `C-b`. Only characters up to octal code 777 can be specified this way.

These escape sequences may also be used in strings. See [Non-ASCII in Strings](Non_002dASCII-in-Strings.html).

Next: [Ctl-Char Syntax](Ctl_002dChar-Syntax.html), Previous: [Basic Char Syntax](Basic-Char-Syntax.html), Up: [Character Type](Character-Type.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
