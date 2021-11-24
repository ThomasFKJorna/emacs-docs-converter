

### 39.22 Character Display

This section describes how characters are actually displayed by Emacs. Typically, a character is displayed as a *glyph* (a graphical symbol which occupies one character position on the screen), whose appearance corresponds to the character itself. For example, the character ‘`a`’ (character code 97) is displayed as ‘`a`’. Some characters, however, are displayed specially. For example, the formfeed character (character code 12) is usually displayed as a sequence of two glyphs, ‘`^L`’, while the newline character (character code 10) starts a new screen line.

You can modify how each character is displayed by defining a *display table*, which maps each character code into a sequence of glyphs. See [Display Tables](Display-Tables.html).

|                                                     |    |                                                  |
| :-------------------------------------------------- | -- | :----------------------------------------------- |
| • [Usual Display](Usual-Display.html)               |    | The usual conventions for displaying characters. |
| • [Display Tables](Display-Tables.html)             |    | What a display table consists of.                |
| • [Active Display Table](Active-Display-Table.html) |    | How Emacs selects a display table to use.        |
| • [Glyphs](Glyphs.html)                             |    | How to define a glyph, and what glyphs mean.     |
| • [Glyphless Chars](Glyphless-Chars.html)           |    | How glyphless characters are drawn.              |
