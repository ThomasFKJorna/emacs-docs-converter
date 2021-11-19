

Next: [Autoload](Autoload.html), Previous: [Library Search](Library-Search.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 16.4 Loading Non-ASCII Characters

When Emacs Lisp programs contain string constants with non-ASCII characters, these can be represented within Emacs either as unibyte strings or as multibyte strings (see [Text Representations](Text-Representations.html)). Which representation is used depends on how the file is read into Emacs. If it is read with decoding into multibyte representation, the text of the Lisp program will be multibyte text, and its string constants will be multibyte strings. If a file containing Latin-1 characters (for example) is read without decoding, the text of the program will be unibyte text, and its string constants will be unibyte strings. See [Coding Systems](Coding-Systems.html).

In most Emacs Lisp programs, the fact that non-ASCII strings are multibyte strings should not be noticeable, since inserting them in unibyte buffers converts them to unibyte automatically. However, if this does make a difference, you can force a particular Lisp file to be interpreted as unibyte by writing ‘`coding: raw-text`’ in a local variables section. With that designator, the file will unconditionally be interpreted as unibyte. This can matter when making keybindings to non-ASCII characters written as `?vliteral`.
