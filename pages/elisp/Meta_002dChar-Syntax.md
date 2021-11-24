

#### 2.4.3.4 Meta-Character Syntax

A *meta character* is a character typed with the `META` modifier key. The integer that represents such a character has the 2\*\*27 bit set. We use high bits for this and other modifiers to make possible a wide range of basic character codes.

In a string, the 2\*\*7 bit attached to an ASCII character indicates a meta character; thus, the meta characters that can fit in a string have codes in the range from 128 to 255, and are the meta versions of the ordinary ASCII characters. See [Strings of Events](Strings-of-Events.html), for details about `META`-handling in strings.

The read syntax for meta characters uses ‘`\M-`’. For example, ‘`?\M-A`’ stands for `M-A`. You can use ‘`\M-`’ together with octal character codes (see below), with ‘`\C-`’, or with any other syntax for a character. Thus, you can write `M-A` as ‘`?\M-A`’, or as ‘`?\M-\101`’. Likewise, you can write `C-M-b` as ‘`?\M-\C-b`’, ‘`?\C-\M-b`’, or ‘`?\M-\002`’.
