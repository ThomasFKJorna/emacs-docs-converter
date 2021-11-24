

#### 2.4.3.5 Other Character Modifier Bits

The case of a graphic character is indicated by its character code; for example, ASCII distinguishes between the characters ‘`a`’ and ‘`A`’. But ASCII has no way to represent whether a control character is upper case or lower case. Emacs uses the 2\*\*25 bit to indicate that the shift key was used in typing a control character. This distinction is possible only when you use X terminals or other special terminals; ordinary text terminals do not report the distinction. The Lisp syntax for the shift bit is ‘`\S-`’; thus, ‘`?\C-\S-o`’ or ‘`?\C-\S-O`’ represents the shifted-control-o character.

The X Window System defines three other modifier bits that can be set in a character: *hyper*, *super* and *alt*. The syntaxes for these bits are ‘`\H-`’, ‘`\s-`’ and ‘`\A-`’. (Case is significant in these prefixes.) Thus, ‘`?\H-\M-\A-x`’ represents `Alt-Hyper-Meta-x`. (Note that ‘`\s`’ with no following ‘`-`’ represents the space character.) Numerically, the bit values are 2\*\*22 for alt, 2\*\*23 for super and 2\*\*24 for hyper.
